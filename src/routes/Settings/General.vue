<template>
  <div>
    <settings-apperance-panel></settings-apperance-panel>
    <settings-notifications-panel></settings-notifications-panel>
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
          caption: "Tools",
          path: "/Settings/General",
        },
      },
    });
  },
};
</script>