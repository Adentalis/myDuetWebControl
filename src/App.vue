<style>
#title:not(:hover) {
  color: inherit;
}
#title {
  margin-right: 20px;
}

.empty-table-fix td {
  padding-left: 0px !important;
  padding-right: 0px !important;
}

.global-control.theme--light {
  background-color: #f5f5f5 !important;
}
#global-container .v-card.theme--light {
  background-color: #f5f5f5 !important;
}
.global-control.theme--dark {
  background-color: #515151 !important;
}
#global-container .v-card.theme--dark {
  background-color: #515151 !important;
}

input[type="number"] {
  -moz-appearance: textfield;
}

input::-webkit-outer-spin-button,
input::-webkit-inner-spin-button {
  -webkit-appearance: none;
}

a:not(:hover) {
  text-decoration: none;
}

textarea {
  line-height: 1.25rem !important;
}

.theme--dark textarea {
  caret-color: #fff;
}

.v-item-group.theme--dark .v-btn__content {
  color: #fff !important;
}

.v-card__title {
  font-size: 1rem;
}

.navRouter {
  border: 2px solid red;
  width: 200px;
}

.nav-item {
  width: 50% !important;
  margin: 0px auto !important;
  padding: 5px 2px;
  cursor: pointer;
  font-size: 22px;
}

.v-navigation-drawer {
  position: fixed !important;
}

.nav-btn {
  text-align: center;
  margin-top: 3vh;
}

#title {
  margin-left: 5vh;
  font-size: 24px;
}

#currentStatus {
  width: 8vh;
  text-align: center;
  transform: scale(1.6);
}
</style>

<template>
  <v-app>
    <!--Start of left navbar-->
    <v-navigation-drawer
      v-model="drawer"
      permanent
      clipped
      fixed
      app
      width="180"
    >
      <div class="nav-item">
        <div class="nav-btn" @click="goTo('/')">
          <v-img src="/img/icons/home.png"></v-img>
          {{ $t("routes.home") }}
        </div>

        <div class="nav-btn" @click="goTo('/Job')">
          <v-img src="/img/icons/print.png"></v-img>
          {{ $t("routes.print") }}
        </div>

        <div class="nav-btn" @click="goTo('/Control')">
          <v-img src="/img/icons/hand.png"></v-img>
          {{ $t("routes.control") }}
        </div>

        <div class="nav-btn" @click="goTo('/Settings')">
          <v-img src="/img/icons/settings.png"></v-img>
          {{ $t("routes.settings") }}
        </div>
        <div class="nav-btn" @click="goBack()">
          <v-img class="a" src="/img/icons/back.png"></v-img>
        </div>
      </div>
    </v-navigation-drawer>
    <!--End of left navbar-->

    <!--Start of statusbar-->
    <v-app-bar id="statusbar" ref="appToolbar" app clipped-left>
      <!--Name of printer-->
      <v-toolbar-title>
        <div id="title">{{ name }}</div>
      </v-toolbar-title>
      <v-spacer></v-spacer>

      <!--connect Button-->
      <connect-btn v-if="isLocal" class="hidden-xs-only"></connect-btn>
      <v-spacer></v-spacer>

      <!-- Status-->
      <status-label id="currentStatus" v-if="status"></status-label>
      <v-spacer></v-spacer>

      <!-- current route-->
      <h2>{{ getRoute }}</h2>
      <v-spacer></v-spacer>

      <!--Time-->
      <h2>{{ getTime }}</h2>
      <v-spacer></v-spacer>

      <!--Stop Btn-->
      <emergency-btn class="hidden-xs-only"></emergency-btn>
    </v-app-bar>
    <!--End of top statusbar-->

    <v-main id="content">
      <v-container fluid>
        <keep-alive>
          <router-view></router-view>
        </keep-alive>
      </v-container>
    </v-main>

    <connect-dialog></connect-dialog>
    <connection-dialog></connection-dialog>
    <file-transfer-dialog></file-transfer-dialog>
    <messagebox-dialog></messagebox-dialog>

    <component
      v-for="component in injectedComponentNames"
      :is="component"
      :key="component"
    ></component>
  </v-app>
</template>

<script>
"use strict";
import Plugins from "./plugins";
import Piecon from "piecon";
import { mapState, mapGetters, mapActions } from "vuex";

import { Menu, Routes } from "./routes";
import { isPrinting } from "./store/machine/modelEnums.js";

export default {
  computed: {
    ...mapState("machine", ["model"]),
    ...mapState({
      isLocal: (state) => state.isLocal,
      globalShowConnectDialog: (state) => state.showConnectDialog,

      boards: (state) => state.machine.model.boards,
      menuDirectory: (state) => state.machine.model.directories.menu,
      name: (state) => state.machine.model.network.name,
      status: (state) => state.machine.model.state.status,

      darkTheme: (state) => state.settings.darkTheme,
      webcam: (state) => state.settings.webcam,

      injectedComponents: (state) => state.uiInjection.injectedComponents,
      getTime() {
        let time = this.model.state.time;
        if (time !== null) {
          time = time.substring(0, time.length - 3);
          time = time.substring(time.length - 5);
          return time;
        }
      },
      getRoute() {
        let path = this.$route.fullPath.split("/");
        let view = "";
        switch (path[path.length - 1]) {
          case "":
            view = this.$t("routes.home");
            break;
          case "Job":
            view = this.$t("routes.print");
            break;
          case "Control":
            view = this.$t("routes.control");
            break;
          case "Settings":
            view = this.$t("routes.settings");
            break;
          case "Tools":
            view = this.$t("routes.tools");
            break;
          case "Effector":
            view = this.$t("routes.effector");
            break;
          case "Fans":
            view = this.$t("routes.fans");
            break;
          case "HeightMap":
            view = this.$t("routes.heightmap");
            break;
          case "Macros":
            view = this.$t("routes.macros");
            break;
          case "Console":
            view = this.$t("routes.console");
            break;
          case "General":
            view = this.$t("routes.generalSettings");
            break;
          case "Machine":
            view = this.$t("routes.machine");
            break;
          case "System":
            view = this.$t("routes.system");
            break;
          case "About":
            view = this.$t("routes.about");
            break;
          default:
            view = this.$t("routes.home");
            break;
        }

        return view;

        // if (path[1] !== "") return path[path.length - 1];
        //else return "Home";
      },
    }),
    ...mapGetters("machine", ["hasTemperaturesToDisplay"]),
    ...mapGetters("machine/model", ["jobProgress"]),
    categories() {
      return Object.keys(Menu)
        .map((key) => Menu[key])
        .filter(
          (item) => item.condition || item.pages.some((page) => page.condition)
        );
    },
    currentPageCondition() {
      const currentRoute = this.$route;
      let checkRoute = function (route, isChild) {
        let flag = route.path === currentRoute.path && route.condition;
        if (!flag && isChild) {
          let curPath = currentRoute.path.replace(/\/$/, "");
          if (curPath.endsWith(route.path))
            flag =
              curPath.substring(0, curPath.length - route.path.length) +
                route.path ===
                curPath && route.condition;
        }
        if (!flag && route.children != undefined)
          flag = route.children.some((child) => checkRoute(child, true));
        return flag;
      };
      return Routes.some((route) => checkRoute(route));
    },
    toggleGlobalContainerColor() {
      if (this.hideGlobalContainer) {
        return this.darkTheme ? "red darken-5" : "red lighten-4";
      }
      return this.darkTheme ? "green darken-5" : "green lighten-4";
    },
  },
  data() {
    return {
      rname: "",
      drawer: this.$vuetify.breakpoint.lgAndUp,
      hideGlobalContainer: false,
      ht: 0,
      wasXs: this.$vuetify.breakpoint.xsOnly,
      injectedComponentNames: [],
    };
  },
  methods: {
    ...mapActions(["loadDwcPlugin", "unloadDwcPlugin"]),
    ...mapActions(["connect", "disconnectAll"]),
    ...mapActions("settings", ["load"]),
    goTo(path) {
      this.$router.push({ path: path });
    },

    goBack() {
      this.$router.go(-1);
    },

    stop() {},
    async startPlugin(plugin) {
      await this.loadDwcPlugin({ name: plugin.name, saveSettings: true });
    },
    getPages(category) {
      return category.pages.filter((page) => page.condition);
    },
    isExpanded(category) {
      if (this.$vuetify.breakpoint.xsOnly) {
        const route = this.$route;
        return category.pages.some((page) => page.path === route.path);
      }
      return true;
    },
    updateTitle() {
      const jobProgress = this.jobProgress;
      const title =
        (jobProgress > 0 && isPrinting(this.status)
          ? `(${(jobProgress * 100).toFixed(1)}%) `
          : "") + this.name;
      if (document.title !== title) {
        document.title = title;
      }
    },
  },
  mounted() {
    this.rname = this.$route.fullPath;
    Plugins.forEach((element) => {
      this.startPlugin(element);
    });
    this.ht = window.innerHeight / 6;
    // Attempt to disconnect from every machine when the page is being unloaded
    window.addEventListener("unload", this.disconnectAll);

    // Connect if running on a board
    if (!this.isLocal) {
      this.connect();
    }

    // Attempt to load the settings
    this.load();

    // Validate navigation
    this.$router.beforeEach((to, from, next) => {
      if (Routes.some((route) => route.path === to.path && !route.condition)) {
        next("/");
      } else {
        next();
      }
    });

    // Set up Piecon
    Piecon.setOptions({
      color: "#00f", // Pie chart color
      background: "#bbb", // Empty pie chart color
      shadow: "#fff", // Outer ring color
      fallback: false, // Toggles displaying percentage in the title bar (possible values - true, false, 'force')
    });
  },
  watch: {
    currentPageCondition(to) {
      if (!to) {
        this.$router.push("/");
      }
    },
    darkTheme(to) {
      this.$vuetify.theme.dark = to;
    },
    status(to, from) {
      const printing = isPrinting(to);
      if (printing !== isPrinting(from)) {
        if (printing) {
          // Go to Job Status when a print starts
          if (this.$router.currentRoute.path !== "/Job/Status") {
            this.$router.push("/Job/Status");
          }
        } else {
          // Remove the Piecon again when the print has finished
          Piecon.reset();
        }
      }
    },
    name() {
      this.updateTitle();
    },
    jobProgress(to, from) {
      if (
        isPrinting(this.status) &&
        Math.round(to * 100) !== Math.round(from * 100)
      ) {
        Piecon.setProgress(to * 100);
      }
      this.updateTitle();
    },
    injectedComponents() {
      this.injectedComponents.forEach(function (item) {
        if (this.injectedComponentNames.indexOf(item.name) === -1) {
          this.$options.components[item.name] = item.component;
          this.injectedComponentNames.push(item.name);
        }
      }, this);
    },
  },
};
</script>
