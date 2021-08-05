<template>
  <v-row>
    <v-col v-if="authenticated">
      <system-file-list></system-file-list>
    </v-col>

    <v-col>
      <v-row justify="center">
        <v-btn color="primary" dark @click.stop="dialog = true">
          Click to authorize
        </v-btn>

        <v-dialog v-model="dialog" max-width="290">
          <v-card>
            <v-card-title class="text-h5"> System settings </v-card-title>

            <v-card-text>
              Enter Password to continue
              <v-text-field
                label="Authorization Code"
                v-model="code"
                required
              ></v-text-field>
              <p class="red--text">{{ message }}</p>
            </v-card-text>

            <v-card-actions>
              <v-spacer></v-spacer>

              <v-btn color="green darken-1" text @click="submit">
                Submit
              </v-btn>

              <v-btn color="green darken-1" text @click="dialog = false">
                Cancel
              </v-btn>
            </v-card-actions>
          </v-card>
        </v-dialog>
      </v-row>
    </v-col>
  </v-row>
</template>

<script>
"use strict";

import { registerRoute } from "..";

export default {
  data() {
    return {
      authenticated: false,
      code: "",
      message: "",
      dialog: true,
    };
  },
  methods: {
    submit() {
      if (this.code === this.$PASSWORD) {
        this.authenticated = true;
        this.dialog = false;
      } else {
        this.message = "Code match failed";
      }
    },
  },
  beforeRouteLeave(to, from, next) {
    this.authenticated = false;
    this.code = "";
    next();
  },
  mounted() {
    this.dialog = true;
  },
  install() {
    // Register a route via Files -> System
    registerRoute(this, {
      Files: {
        System: {
          icon: "mdi-cog",
          caption: "menu.files.system",
          path: "/Settings/System",
        },
      },
    });
  },
};
</script>
