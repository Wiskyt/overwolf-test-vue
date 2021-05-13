<template>
  <div>
    <Header />
    <h1>Ingame</h1>
    <h3 v-if="matchStarted">Game in progress</h3>
    <h3 v-else>Game loading..</h3>
  </div>
</template>

<script lang="ts">
import Vue from "vue";
import { Component } from "vue-property-decorator";
import { OWGamesEvents } from "@overwolf/overwolf-api-ts";
import Header from "../../components/Header.vue";
import { GAME_FEATURES } from "../../constants/Features";

@Component({
  components: {
    Header,
  },
})
export default class Ingame extends Vue {
  private gameEventListener: OWGamesEvents | undefined;
  private matchStarted = false;

  created(): void {
    this.gameEventListener = new OWGamesEvents(
      {
        onInfoUpdates: this.onInfoUpdates.bind(this),
        onNewEvents: this.onNewEvents.bind(this),
      },
      GAME_FEATURES
    );
    this.gameEventListener.start();
  }
  // eslint-disable-next-line @typescript-eslint/no-explicit-any
  private onInfoUpdates(info: any) {
    console.log(JSON.stringify(info));
    if (info?.game_info?.matchStarted !== undefined) {
      this.matchStarted = info.game_info.matchStarted;
    }
    // this.logLine(this._infoLog, info, false);
  }

  // eslint-disable-next-line @typescript-eslint/no-explicit-any
  private onNewEvents(event: any) {
    console.log(JSON.stringify(event));
  }
}
</script>

<style scoped>
h1,
h3 {
  color: black;
}
</style>
