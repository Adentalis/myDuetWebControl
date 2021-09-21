<style scoped>
#settingsgrid {
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  grid-template-rows: repeat(2, 1fr);
  grid-gap: 10vh 5vh;
  margin: 5vh;
}

.bigRouter {
  color: rgb(255, 255, 255);
  height: 120%;
  text-decoration: none;
  text-transform: uppercase;
  display: inline-block;
  text-align: center;
  background: rgb(167, 88, 219);
  border: 5px rgb(76, 23, 119) solid;
  font-size: 4vh;
  border-radius: 3vh;
}

.bigRouter img {
  max-width: 18%;
  margin-left: auto;
  margin-right: auto;
  margin-top: 2%;
  padding-top: 3%;
}

.bigRouter h3 {
  padding-top: 5%;
}

.bigRouter:hover {
  background-color: #853be6;
  color: white;
  cursor: pointer;
}
</style>


<template>
  <div id="settingsgrid">
    <router-link class="bigRouter" to="/Settings/General">
      <div>
        <img src="/img/icons/preferences.png" />
        <h3 style="text-align: center">{{ $t("routes.generalSettings") }}</h3>
      </div>
    </router-link>

    <router-link class="bigRouter" to="/Settings/Machine">
      <div>
        <img src="/img/icons/machine.png" />
        <h3 style="text-align: center">{{ $t("routes.machine") }}</h3>
      </div>
    </router-link>

    <router-link class="bigRouter" to="/Settings/System">
      <div>
        <img src="/img/icons/folder.png" />
        <h3 style="text-align: center">{{ $t("routes.system") }}</h3>
      </div>
    </router-link>

    <router-link class="bigRouter" to="/Settings/About">
      <div>
        <img src="/img/icons/info.png" />
        <h3 style="text-align: center">{{ $t("routes.about") }}</h3>
      </div>
    </router-link>
  </div>
</template>

<script>
"use strict";

import { mapState } from "vuex";

import { registerRoute } from "..";
import { MachineMode } from "../../store/machine/modelEnums.js";

export default {
  computed: {
    ...mapState("machine/model", {
      atxPower: (state) => state.state.atxPower,
      machineMode: (state) => state.state.machineMode,
    }),
    isFFForUnset() {
      return !this.machineMode || this.machineMode === MachineMode.fff;
    },
  },
  install() {
    // Register a route via Control -> Dashboard
    registerRoute(this, {
      Control: {
        Dashboard: {
          icon: "mdi-view-dashboard",
          caption: "menu.control.dashboard",
          path: "/Settings",
        },
      },
    });
  },
};
</script>
