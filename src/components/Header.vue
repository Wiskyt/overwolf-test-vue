<template>
  <header id="header" class="app-header">
    <img src="/img/header_icon.svg" />
    <h1>Sample App</h1>
    <div class="window-controls-group">
      <button class="icon window-control" id="minimizeButton" @click="minimize">
        <svg>
          <use xlink:href="/img/header_icons.svg#window-control_minimize" />
        </svg>
      </button>
      <button class="icon window-control" id="maximizeButton" @click="maximize">
        <svg>
          <use xlink:href="/img/header_icons.svg#window-control_maximize" />
        </svg>
      </button>
      <button
        class="icon window-control window-control-close"
        id="closeButton"
        @click="close"
      >
        <svg>
          <use xlink:href="/img/header_icons.svg#window-control_close" />
        </svg>
      </button>
    </div>
  </header>
</template>

<script lang="ts">
import { WINDOW_NAMES } from "@/constants/Windows";
import { OWWindow } from "@overwolf/overwolf-api-ts";
import { Component } from "vue-property-decorator";
import Vue from "vue";

const HeaderProps = Vue.extend({
  props: {
    windowName: String,
  },
});

@Component
export default class Header extends HeaderProps {
  private mainWindow: OWWindow | undefined;
  private currWindow: OWWindow | undefined;
  private maximized = false;

  created(): void {
    this.mainWindow = new OWWindow(WINDOW_NAMES.BACKGROUND);
    this.currWindow = new OWWindow(this.windowName);
  }
  minimize(): void {
    this.currWindow?.minimize();
  }
  maximize(): void {
    if (!this.maximized) this.currWindow?.maximize();
    else this.currWindow?.restore();
    this.maximized = !this.maximized;
  }
  close(): void {
    this.mainWindow?.close();
  }
}
</script>

<style scoped>
.icon {
  position: relative;
  width: var(--size, 30px);
  height: var(--size, 30px);
  transition: 0.15s;
}
.icon svg {
  width: 100%;
  height: 100%;
}
.app-header {
  display: flex;
  background-color: #272727;
  align-items: center;

  z-index: 9999;
}

.app-header h1 {
  font-family: "Roboto";
  font-style: normal;
  font-weight: normal;
  color: #8d8d8d;
  font-size: 14px;
  padding-left: 8px;
  margin: 0;
}
.app-header.draggable,
.app-header.draggable h1 {
  user-select: none;

  cursor: move;
  cursor: grab;
  cursor: -moz-grab;
  cursor: -webkit-grab;
}
.app-header img {
  padding-left: 10px;
  margin: 0;
}

.window-controls-group {
  z-index: 1001;
  margin-left: auto;
  display: flex;
  --color: #a8a8a8;
  --color-hover: #dedede;
  --bg-color-hover: #464646;
}

.window-control {
  color: var(--color);
  background-color: transparent;
}
.window-control:hover,
.window-control:active {
  color: var(--color-hover);
  background-color: var(--bg-color-hover);
}

.window-control-close {
  --color-hover: #dedede;
  --bg-color-hover: #c21913;
}

.light-theme {
  background-color: #f6f6f6;
}
.light-theme .window-controls-group {
  --color: #828282;
  --color-hover: #4f4f4f;
  --bg-color-hover: #dedede;
}

.tooltip {
  visibility: hidden;
  position: absolute;
  margin-top: -3px;
  width: 200px;
  right: 0px;
  background: #464646;
  padding: 30px 5px;
  line-height: 20px;
  font-family: "Roboto";
  font-style: normal;
  font-weight: normal;
  font-size: 14px;
  color: #d5d5d5;
}

.tooltip a {
  font-family: "Roboto";
  font-style: normal;
  font-weight: normal;
  font-size: 14px;
  color: #d5d5d5;
}

.tooltip hr {
  border-color: #272727;
  border-width: 0.5px;
}

.tooltip a:visited {
  color: #d5d5d5;
}

.support:hover .tooltip {
  visibility: visible;
}

.socialIcons {
  margin-top: 20px;
  margin-right: 10px;
}

.socialIcons img {
  vertical-align: middle;
}

.socialIcons a {
  text-decoration: none;
}
</style>
