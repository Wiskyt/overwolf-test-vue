<template>
</template>

<script lang="ts">
import Vue from 'vue';
import { Component } from "vue-property-decorator";
import store from "../../store";
import {
  // OWGames,
  // OWGameListener,
  OWWindow
} from '@overwolf/overwolf-api-ts';

@Component
export default class Background extends Vue {
  // private windows: Record<string, OWWindow> = {}

  // mounted() {
  //   this.windows = {
  //     DESKTOP: new OWWindow('DESKTOP'),
  //     INGAME: new OWWindow('INGAME')
  //   }
  // }
  // async created() {
  //   this.windows['DESKTOP'].restore()
  // }
  
  get currentWindow() {
    return store.state.isInGame ? "ingame" : "desktop";
  }
}
</script>

<style scoped></style>
