<style scoped>
#grid {
  display: grid;
  grid-template-columns: repeat(60%, 30%);
  grid-template-rows: repeat(2, 45vh);
  grid-gap: 4vh;
}
.v-tooltip__content {
  z-index: 5000000 !important;
}
</style>

<template>
  <div id="grid">
    <tools-panel></tools-panel>
    <extrude-panel></extrude-panel>
    <div>
      <v-col>
        <v-row justify="center">
          <v-dialog v-model="showOffsetModel" persistent max-width="600">
            <v-card style="overflow: hidden; padding: 10px 30px">
              <v-row>
                <v-col md="12">
                  <div class="text-center d-flex align-center">
                    <v-tooltip bottom>
                      <template v-slot:activator="{ on, attrs }">
                        <v-icon
                          style="position: absolute; top: 10px; right: 20px"
                          color="primary"
                          dark
                          v-bind="attrs"
                          v-on="on"
                          >mdi-information-outline</v-icon
                        >
                      </template>
                      <span>Co</span>
                    </v-tooltip>
                  </div>
                </v-col>
              </v-row>
              <v-row justify="center">
                <v-col md="3" sm="4">
                  <v-text-field
                    type="number"
                    label="Tool"
                    v-model="tool"
                  ></v-text-field>
                  <!-- <v-select
                  v-model="select"
                  :items="items"
                  label="Tool"
                  data-vv-name="select"
                ></v-select> -->
                </v-col>
              </v-row>
              <v-row justify="center">
                <v-col md="3" class="p4">
                  <v-text-field
                    type="text"
                    label="X:"
                    v-model="x"
                  ></v-text-field>
                </v-col>
                <v-col md="3">
                  <v-text-field
                    type="text"
                    label="Y:"
                    v-model="y"
                  ></v-text-field>
                </v-col>
                <v-col md="4">
                  <v-text-field
                    type="text"
                    label="Z:"
                    v-model="z"
                  ></v-text-field>
                </v-col>
              </v-row>

              <v-card-actions>
                <v-spacer></v-spacer>

                <code-btn
                  :code="`G10 P${tool} X${x} Y${y} Z${z} \nM500 P10`"
                  :log="true"
                  @click="showOffsetModel = false"
                  >Set offset</code-btn
                >
                <v-btn
                  color="red darken-1"
                  text
                  @click="showOffsetModel = false"
                >
                  Cancel
                </v-btn>
              </v-card-actions>
            </v-card>
          </v-dialog>
        </v-row>
      </v-col>
    </div>
  </div>
</template>

<script>
"use strict";

import { registerRoute } from "..";

export default {
  data() {
    return {
      showOffsetModel: false,
      tool: "",
      x: "",
      y: "",
      z: "",
    };
  },

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
