<template>
  <v-card outlined>
    <v-card-title> Logo </v-card-title>

    <v-card-text class="d-flex flex-column">
      <upload-btn fab dark target="filaments" color="primary">
        <v-icon>mdi-cloud-upload</v-icon>
      </upload-btn>
    </v-card-text>
  </v-card>
</template>

<script>
"use strict";

import { mapState, mapMutations } from "vuex";

export default {
  computed: {
    ...mapState(["settings"]),
    notificationErrorsPersistent: {
      get() {
        return this.settings.notifications.errorsPersistent;
      },
      set(value) {
        this.update({ notifications: { errorsPersistent: value } });
      },
    },
    notificationTimeout: {
      get() {
        return this.settings.notifications.timeout;
      },
      set(value) {
        if (this.isNumber(value) && value >= 0) {
          this.update({ notifications: { timeout: value } });
        }
      },
    },
  },
  methods: mapMutations("settings", ["update"]),
};
</script>
