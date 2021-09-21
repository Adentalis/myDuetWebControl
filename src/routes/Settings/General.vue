<style scoped>
.marginLeft {
  margin-left: 2vh;
}
.marginBottomLeft {
  margin-left: 2vh;
  margin-bottom: 2vh;
}
</style>

<template>
  <div>
    <settings-apperance-panel></settings-apperance-panel>
    <settings-notifications-panel></settings-notifications-panel>
    <v-card outlined>
      <v-card-title>Color Preferences</v-card-title>
      <v-row>
        <v-col md="6">
          <h4 class="marginLeft">Control</h4>
          <div>
            <p class="marginLeft">Background</p>
            <v-color-picker
              v-model="newControl.background"
              hide-canvas
            ></v-color-picker>
          </div>

          <div>
            <p class="marginLeft">Border</p>
            <v-color-picker
              v-model="newControl.border"
              hide-canvas
            ></v-color-picker>
          </div>
        </v-col>
        <v-col md="6">
          <h4>Settings</h4>
          <div>
            <p>Background</p>
            <v-color-picker
              v-model="newSettings.background"
              hide-canvas
            ></v-color-picker>
          </div>

          <div>
            <p>Border</p>
            <v-color-picker
              v-model="newSettings.border"
              hide-canvas
            ></v-color-picker>
          </div>
        </v-col>
        <v-col>
          <v-btn
            class="marginBottomLeft"
            @click="saveChanges"
            depressed
            color="primary"
          >
            Save changes
          </v-btn>
        </v-col>
      </v-row>
    </v-card>
  </div>
</template>


<script>
"use strict";

import { registerRoute } from "..";
import { mapState, mapActions } from "vuex";

export default {
  computed: {
    ...mapState(["setting", "control"]),
  },
  data() {
    return {
      newControl: {
        border: "",
        background: "",
      },
      newSettings: {
        border: "",
        background: "",
      },
    };
  },
  methods: {
    ...mapActions(["updateColourPanels"]),
    saveChanges() {
      this.$store.dispatch("updateColourPanels", {
        setting: this.newSettings,
        control: this.newControl,
      });
    },
  },
  mounted() {
    this.newSettings.background = this.setting.background;
    this.newSettings.border = this.setting.border;
    this.newControl.background = this.control.background;
    this.newControl.border = this.control.border;
  },
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