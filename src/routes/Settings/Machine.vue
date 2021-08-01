<template>
  <v-row class="pa-3">
    <v-col cols="12" md="4">
      <v-row>
        <v-col cols="12" sm="6" md="12">
          <settings-machine-panel></settings-machine-panel>
        </v-col>
        <v-col cols="12" sm="6" md="12">
          <settings-communication-panel></settings-communication-panel>
        </v-col>
      </v-row>
    </v-col>

    <v-col cols="12" md="8">
      <settings-list-items-panel></settings-list-items-panel>
    </v-col>
  </v-row>
</template>

<script>
"use strict";

import { registerRoute, MachineSettingTabs } from "..";

export default {
  install() {
    // Register a route via Settings -> Machine
    registerRoute(this, {
      Settings: {
        Machine: {
          icon: "mdi-cogs",
          caption: "menu.settings.machine",
          path: "/Settings/Machine",
        },
      },
    });
  },
  computed: {
    tabs: () => MachineSettingTabs,
  },
};
</script>
