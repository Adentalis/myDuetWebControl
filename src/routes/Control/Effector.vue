<style scoped>
#grid {
  display: grid;
  grid-template-rows: repeat(2, 40vh);
  grid-gap: 3vh;
  margin: 3vh;
}
</style>

<template>
  <v-row>
    <v-col>
      <div id="grid">
        <status-panel></status-panel>
        <movement-panel></movement-panel>
      </div>
    </v-col>
  </v-row>
</template>

<script>
"use strict";

import { registerRoute } from "..";

export default {
  install() {
    // Register a route via Files -> Macros
    registerRoute(this, {
      Files: {
        Macros: {
          icon: "mdi-polymer",
          caption: "menu.files.macros",
          path: "/Control/Effector",
        },
      },
    });
  },
};
</script>
