<template>
  <div id="app">
    <BackgroundWindow v-if="window === names.BACKGROUND" />
    <DesktopWindow v-if="window === names.DESKTOP" />
    <IngameWindow v-if="window === names.INGAME" />
  </div>
</template>

<script lang="ts">
import Vue from "vue";
import { Component } from "vue-property-decorator";
import BackgroundWindow from "./windows/background/Background.vue";
import DesktopWindow from "./windows/desktop/Desktop.vue";
import IngameWindow from "./windows/ingame/Ingame.vue";
import {
  // OWGames,
  // OWGameListener,
  OWWindow
} from '@overwolf/overwolf-api-ts';
import { WindowNames } from './constants/Windows'

@Component({
  components: {
    BackgroundWindow,
    DesktopWindow,
    IngameWindow,
  }
})
export default class App extends Vue {
  private windows = {}
  private currentWindow = ''
  async mounted() {
    this.currentWindow = await this.getCurrentWindow()

  }
  get window() {
    return this.currentWindow
  }
  get windowNames() {
    return WindowNames
  }
  private getCurrentWindow(): Promise<string> {
    return new Promise(resolve =>
      overwolf.windows.getCurrentWindow(result => {
        resolve(result.window.name);
      })
    )
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
  margin-top: 60px;
}
</style>
