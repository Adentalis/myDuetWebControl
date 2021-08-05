<template>
  <v-row>
    <v-col v-if="authenticated">
      <system-file-list></system-file-list>
    </v-col>

    <v-col v-else>
      <v-row justify="center">
        <v-dialog v-model="unauthenticated" persistent max-width="300">
          <v-card>
            <v-card-title class="text-h5"> System Settings </v-card-title>

            <v-card-text>
              Enter Admin Password to continue
              <v-text-field
                label="Admin Password"
                v-model="code"
                required
              ></v-text-field>
              <p class="red--text">{{ message }}</p>
            </v-card-text>

            <v-card-actions>
              <button id="goBackButton" @click="$router.back(-1)">Close</button>
              <v-spacer></v-spacer>

              <v-btn color="green darken-1" text @click="submit">
                Submit
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
      unauthenticated: true,
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
  beforeRouteEnter(to, from, next) {
    next((vm) => {
      vm.unauthenticated = true;
    });
  },
  beforeRouteLeave(to, from, next) {
    this.authenticated = false;
    this.unauthenticated = false;
    this.code = "";
    next();
  },
  mounted() {},
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
