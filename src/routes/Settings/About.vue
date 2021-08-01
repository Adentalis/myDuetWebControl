<template>
  <div>
    <settings-about-panel></settings-about-panel>
    <settings-electronics-panel></settings-electronics-panel>
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
          path: "/Settings/About",
        },
      },
    });
  },
};
</script>