<style scoped>
#settingsgrid {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  grid-template-rows: minmax(30vh, 30vh);
  grid-gap: 5vh;
  margin: 5vh;
}

.bigRouter {
  background-color: rgb(126, 123, 123);
  color: rgb(68, 68, 241);
  height: 30vh;
  text-decoration: none;
  text-transform: uppercase;
  display: inline-block;
  text-align: center;
  padding-top: 23vh;
  background: cornflowerblue;
  border: 5px rgb(68, 68, 241) solid;
}

.bigRouter:hover {
  background-color: #2e71ec;
  color: white;
  cursor: pointer;
}
</style>


<template>
  <div id="settingsgrid">
    <router-link class="bigRouter" to="/Settings/General">General</router-link>
    <router-link class="bigRouter" to="/Settings/Preferences"
      >Preferences</router-link
    >
    <router-link class="bigRouter" to="/Settings/Configuration"
      >Configuration</router-link
    >
    <router-link class="bigRouter" to="/Files/Macros">x</router-link>

    <router-link class="bigRouter" to="/Settings/Plugins">PlugIns</router-link>

    <router-link class="bigRouter" to="/Settings/About">About</router-link>
  </div>
</template>

<script>
"use strict";

import { mapState } from "vuex";

import { registerRoute } from "..";
import { MachineMode } from "../../store/machine/modelEnums.js";

export default {
  computed: {
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
          path: "/Settings",
        },
      },
    });
  },
};
</script>
