/* eslint-disable @typescript-eslint/no-explicit-any */
<template>
  <div>
    <Header />
    <h1>Desktop</h1>
    <h3>{{ summonerName }}</h3>
    <h3 v-if="isInChampSelect">In champ select</h3>
    <h3 v-if="isInGameStats">Game finished (stats)</h3>
  </div>
</template>

<script lang="ts">
import Vue from "vue";
import { Component } from "vue-property-decorator";
import { OWGamesEvents } from "@overwolf/overwolf-api-ts";
import Header from "../../components/Header.vue";
import { LAUNCHER_FEATURES } from "../../constants/Features";

@Component({
  components: {
    Header,
  },
})
export default class Dekstop extends Vue {
  private gameEventListener: OWGamesEvents | undefined;
  private summonerName = "Not connected";
  public isInChampSelect = false;
  public isInGameStats = false;

  created(): void {
    overwolf.games.launchers.onLaunched.addListener(() => {
      this.registerEvents();
      setTimeout(this.setFeatures, 1000);
    });
    overwolf.games.launchers.getRunningLaunchersInfo((res) => {
      if (this.launcherRunning(res)) {
        this.registerEvents();
        setTimeout(this.setFeatures, 1000);
      }
      console.log("getRunningLaunchersInfo: " + JSON.stringify(res));
    });
  }
  registerEvents(): void {
    const onErrorListener = (info: any) => {
      console.log("Error: " + JSON.stringify(info));
    };
    const onInfoUpdates2Listener = (info: any) => {
      console.log("Info UPDATE: " + JSON.stringify(info));
      if (info?.info?.summoner_info?.display_name) {
        this.summonerName = info.info.summoner_info.display_name;
      } else if (info?.info?.game_flow?.phase) {
        switch (info.info.game_flow.phase) {
          case "ChampSelect":
            this.isInChampSelect = true;
            break;
          case "GameStart":
            this.isInChampSelect = false;
            break;
          case "WaitingForStats":
          case "PreEndOfGame":
          case "EndOfGame":
            this.isInChampSelect = false;
            this.isInGameStats = true;
            break;
          default:
            this.isInChampSelect = false;
            this.isInGameStats = false;
        }
      }
    };
    const onNewEventsListener = (info: any) => {
      console.log("EVENT FIRED: " + JSON.stringify(info));
    };
    overwolf.games.events.onError.addListener(onErrorListener);
    overwolf.games.launchers.events.onInfoUpdates.addListener(
      onInfoUpdates2Listener
    );
    overwolf.games.events.onNewEvents.addListener(onNewEventsListener);
  }
  //  unregisterEvents() {
  //     overwolf.games.events.onError.removeListener(onErrorListener);
  //     overwolf.games.events.onInfoUpdates2.removeListener(onInfoUpdates2Listener);
  //     overwolf.games.events.onNewEvents.removeListener(onNewEventsListener);
  //   }
  launcherRunning(launcherInfo: { launchers: { id: number }[] }): boolean {
    if (!launcherInfo) {
      return false;
    }
    if (!launcherInfo.launchers[0]) {
      return false;
    }
    if (Math.floor(launcherInfo.launchers[0].id / 10) != 10902) {
      return false;
    }
    return true;
  }

  setFeatures(): void {
    overwolf.games.launchers.events.setRequiredFeatures(
      10902,
      LAUNCHER_FEATURES,
      // (res: overwolf.games.launchers.events.SetRequiredFeaturesResult) => {
      () => {
        overwolf.games.launchers.events.getInfo(10902, (info) => {
          console.log("catchup infos", JSON.stringify(info));
          this.summonerName = info.res?.summoner_info?.display_name;
        });
      }
    );
  }
}
</script>

<style scoped></style>
