<style scoped>
#homegrid {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  grid-template-rows: repeat(2, 1fr);
  grid-gap: 10vh 5vh;
  margin: 5vh;
}

.bigRouter {
  background-color: rgb(126, 123, 123);
  color: rgb(255, 255, 255);
  height: 120%;
  text-decoration: none;
  text-transform: uppercase;
  display: inline-block;
  text-align: center;
  background: cornflowerblue;
  border: 5px rgb(68, 68, 241) solid;
  font-size: 4vh;
  border-radius: 3vh;
}

.bigRouter img {
  max-width: 30%;
  margin-left: auto;
  margin-right: auto;
  margin-top: 5%;
  padding-top: 3%;
}

.bigRouter h3 {
  padding-top: 5%;
}

.bigRouter:hover {
  background-color: #2e71ec;
  color: white;
  cursor: pointer;
}
</style>


<template>
  <div id="homegrid">
    <router-link
      class="bigRouter"
      :style="{
        backgroundColor: control.background,
        borderColor: control.border,
      }"
      to="/Control/Tools"
    >
      <div>
        <img src="/img/icons/tool.png" />
        <h3 style="text-align: center">{{ $t("routes.tools") }}</h3>
      </div>
    </router-link>

    <router-link
      class="bigRouter"
      :style="{
        backgroundColor: control.background,
        borderColor: control.border,
      }"
      to="/Control/Effector"
    >
      <div>
        <img src="/img/icons/movement.png" />
        <h3 style="text-align: center">{{ $t("routes.effector") }}</h3>
      </div>
    </router-link>

    <router-link
      class="bigRouter"
      :style="{
        backgroundColor: control.background,
        borderColor: control.border,
      }"
      to="/Control/Fans"
    >
      <div>
        <img src="/img/icons/fan.png" />
        <h3 style="text-align: center">{{ $t("routes.fans") }}</h3>
      </div>
    </router-link>

    <router-link
      class="bigRouter"
      :style="{
        backgroundColor: control.background,
        borderColor: control.border,
      }"
      to="/HeightMap"
    >
      <div>
        <img src="/img/icons/map.png" />
        <h3 style="text-align: center">{{ $t("routes.heightmap") }}</h3>
      </div>
    </router-link>

    <router-link
      class="bigRouter"
      :style="{
        backgroundColor: control.background,
        borderColor: control.border,
      }"
      to="/Control/Macros"
    >
      <div>
        <img src="/img/icons/macro.png" />
        <h3 style="text-align: center">{{ $t("routes.macros") }}</h3>
      </div>
    </router-link>

    <router-link
      class="bigRouter"
      :style="{
        backgroundColor: control.background,
        borderColor: control.border,
      }"
      to="/Control/Console"
    >
      <div>
        <img src="/img/icons/console.png" />
        <h3 style="text-align: center">{{ $t("routes.console") }}</h3>
      </div>
    </router-link>
  </div>
</template>

<script>
"use strict";

import { mapState } from "vuex";

import { registerRoute } from "..";
import { MachineMode } from "../../store/machine/modelEnums.js";

export default {
  computed: {
    ...mapState(["setting", "control"]),
    ...mapState("machine/model", {
      atxPower: (state) => state.state.atxPower,
      machineMode: (state) => state.state.machineMode,
    }),
    isFFForUnset() {
      return !this.machineMode || this.machineMode === MachineMode.fff;
    },
  },
  install() {
    // Register a route via Control -> Dashboard
    registerRoute(this, {
      Control: {
        Dashboard: {
          icon: "mdi-view-dashboard",
          caption: "menu.control.dashboard",
          path: "/Control",
        },
      },
    });
  },
};
</script>
