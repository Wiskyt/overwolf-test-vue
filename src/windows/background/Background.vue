<template>
  <div></div>
</template>

<script lang="ts">
import Vue from "vue";
import { Component } from "vue-property-decorator";
import { OWGameListener, OWGames, OWWindow } from "@overwolf/overwolf-api-ts";
import { WINDOW_NAMES } from "@/constants/Windows";

@Component
export default class Background extends Vue {
  private windows: Record<string, OWWindow> = {};
  private gameListener: OWGameListener | undefined;

  async created(): Promise<void> {
    this.windows[WINDOW_NAMES.DESKTOP] = new OWWindow(WINDOW_NAMES.DESKTOP);
    this.windows[WINDOW_NAMES.INGAME] = new OWWindow(WINDOW_NAMES.INGAME);

    this.windows[WINDOW_NAMES.DESKTOP].restore();

    this.gameListener = new OWGameListener({
      onGameStarted: this.toggleWindows.bind(this),
      onGameEnded: this.toggleWindows.bind(this),
    });

    this.gameListener.start();
    const currWindow = (await this.isLolRunning())
      ? WINDOW_NAMES.INGAME
      : WINDOW_NAMES.DESKTOP;
    this.windows[currWindow].restore();
  }
  // I usually never disable eslint on a line but for this sample lets roll with it
  // eslint-disable-next-line @typescript-eslint/no-explicit-any
  private toggleWindows(info: any) {
    if (!info || !this.isGameLol(info)) {
      return;
    }

    if (info.isRunning) {
      this.windows[WINDOW_NAMES.DESKTOP].close();
      this.windows[WINDOW_NAMES.INGAME].restore();
    } else {
      this.windows[WINDOW_NAMES.INGAME].close();
      this.windows[WINDOW_NAMES.DESKTOP].restore();
    }
  }

  private async isLolRunning(): Promise<boolean> {
    const info = await OWGames.getRunningGameInfo();

    return info && info.isRunning && this.isGameLol(info);
  }

  // Identify whether the RunningGameInfo object we have references Fortnite
  // eslint-disable-next-line @typescript-eslint/no-explicit-any
  private isGameLol(info: any) {
    return info.classId === 5426;
  }
}
</script>
