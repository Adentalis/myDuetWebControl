<style scoped>
#homegrid {
  display: grid;
  grid-template-columns: repeat(4, 35vh);
  grid-template-rows: minmax(30vh, 30vh);
  grid-gap: 10vh 5vh;
  margin: 5vh;
}

.bigRouter {
  background-color: rgb(126, 123, 123);
  color: rgb(68, 68, 241);
  height: 34vh;
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
  <div id="homegrid">
    <router-link class="bigRouter" to="/Control/Tools">Tools</router-link>
    <router-link class="bigRouter" to="/Control/Effector">Effector</router-link>
    <router-link class="bigRouter" to="/Job/GCodeViewer"
      >PrintViewer</router-link
    >

    <router-link class="bigRouter" to="/Files/Extrusion"
      >Filament - in Tools</router-link
    >
    <router-link class="bigRouter" to="/Control/Fans">Fans</router-link>
    <router-link class="bigRouter" to="/HeightMap">Heightmap</router-link>

    <router-link class="bigRouter" to="/Control/Macros">Macros</router-link>

    <router-link class="bigRouter" to="/Control/Console">Console</router-link>
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
          path: "/Control",
        },
      },
    });
  },
};
</script>
