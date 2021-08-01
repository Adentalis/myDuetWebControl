<style scoped>
#grid {
  display: grid;
  grid-template-columns: repeat(2, 92vh);
  grid-template-rows: repeat(2, 45vh);
  grid-gap: 4vh;
}
</style>

<template>
  <div id="grid">
    <tools-panel></tools-panel>
    <temperature-chart></temperature-chart>
    <extrude-panel></extrude-panel>
  </div>
</template>

<script>
"use strict";

import { registerRoute } from "..";

export default {
  install() {
    // Register a route via Files -> Jobs
    registerRoute(this, {
      Files: {
        Jobs: {
          icon: "mdi-play",
          caption: "Extruder",
          path: "/Control/Tools",
        },
      },
    });
  },
};
</script>
