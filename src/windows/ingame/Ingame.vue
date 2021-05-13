<template>
  <div>
    <Header />
    <h1>Ingame</h1>
    <!--
    <h3>Game loading..</h3>
    <h3>Game in progress</h3> -->
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
    // this.logLine(this._infoLog, info, false);
  }

  // Special events will be highlighted in the event log
  // eslint-disable-next-line @typescript-eslint/no-explicit-any
  private onNewEvents(event: any) {
    console.log(JSON.stringify(event));

    // const shouldHighlight = e.events.some((event) => {
    //   switch (event.name) {
    //     case "kill":
    //     case "death":
    //     case "assist":
    //     case "level":
    //     case "matchStart":
    //     case "matchEnd":
    //       return true;
    //   }

    //   return false;
    // });
  }
}
</script>

<style scoped></style>
