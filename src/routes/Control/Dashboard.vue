<style scoped>
#grid {
  display: grid;
  grid-template-columns: repeat(1, 90%);
  grid-template-rows: repeat(1, 80vh);
  grid-gap: 4vh;
  margin-top: 3vh;
  margin-left: 15vh;
}
</style>

<template>
  <div id="grid">
    <v-row>
      <temperature-chart></temperature-chart>
    </v-row>
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
          path: "/",
        },
      },
    });
  },
};
</script>
