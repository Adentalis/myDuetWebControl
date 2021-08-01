<template>
  <div>
    <settings-apperance-panel></settings-apperance-panel>
    <settings-notifications-panel></settings-notifications-panel>
    <settings-logo-panel></settings-logo-panel>
  </div>
</template>


<script>
"use strict";

import { registerRoute } from "..";
import SettingsLogoPanel from "../../components/panels/SettingsLogoPanel.vue";

export default {
  components: { SettingsLogoPanel },
  install() {
    // Register a route via Files -> Jobs
    registerRoute(this, {
      Files: {
        Jobs: {
          icon: "mdi-play",
          caption: "Tools",
          path: "/Settings/General",
        },
      },
    });
  },
};
</script>