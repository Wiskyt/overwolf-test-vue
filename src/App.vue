<template>
  <div id="app">
    <BackgroundWindow v-if="window === windowNames.BACKGROUND" />
    <DesktopWindow v-if="window === windowNames.DESKTOP" />
    <IngameWindow v-if="window === windowNames.INGAME" />
  </div>
</template>

<script lang="ts">
import Vue from "vue";
import { Component } from "vue-property-decorator";
import BackgroundWindow from "./windows/background/Background.vue";
import DesktopWindow from "./windows/desktop/Desktop.vue";
import IngameWindow from "./windows/ingame/Ingame.vue";
// import {
//   OWGames,
//   OWGameListener,
//   OWWindow,
// } from "@overwolf/overwolf-api-ts";
import { WINDOW_NAMES } from "./constants/Windows";

@Component({
  components: {
    BackgroundWindow,
    DesktopWindow,
    IngameWindow,
  },
})
export default class App extends Vue {
  private windows = {};
  private currentWindow = "";
  async mounted(): Promise<void> {
    this.currentWindow = await this.getCurrentWindow();
  }
  get window(): string {
    console.log(this.currentWindow);
    return this.currentWindow;
  }
  get windowNames(): Record<string, string> {
    console.log(WINDOW_NAMES.DESKTOP);
    return WINDOW_NAMES;
  }
  private getCurrentWindow(): Promise<string> {
    return new Promise((resolve) =>
      overwolf.windows.getCurrentWindow((result) => {
        resolve(result.window.name);
      })
    );
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}
</style>
