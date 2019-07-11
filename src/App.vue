<template>
  <v-app dark>
    <notification :text="notification.text" :color="notification.color" />
    <stylizer />
    <identity />
    <menus />
    <navbar v-if="hasNavbar" />
    <alert />
    <v-content :style="getContentStyle()">
      <router-view />
    </v-content>
    <bottombar />
    <!-- <loadingscreen /> -->
  </v-app>
</template>

<script>
import identity from "./components/main/identity.vue";
import stylizer from "./components/main/stylizer.vue";
import menus from "./components/main/menus.vue";
import alert from "./components/main/alert";
import notification from "./components/main/notification.vue";
import navbar from "./components/main/navbar.vue";
import loadingscreen from "./components/main/loadingscreen.vue";
import bottombar from "./components/bottombar.vue";

export default {
  name: "App",
  components: {
    identity,
    notification,
    stylizer,
    navbar,
    menus,
    bottombar,
    loadingscreen,
    alert
  },
  computed: {
    storage() {
      return window.localStorage;
    }
  },
  data: () => ({
    csInterface: null,
    identity: null,
    stylizer: null,
    progress: null,
    loadingscreen: null,
    navbar: null,
    menus: null,
    monaco: null,
    editor: null,
    console: null,
    hasNavbar: true,
    notification: {
      text: "Extension is mounted",
      color: "primary",
      state: false
    },
    home: null,
    isMounted: false
  }),
  mounted() {
    console.clear();
    this.csInterface = new CSInterface();
    // Utility components are already mounted prior to this

    console.log(this.console);
    // this.console.init();

    console.log(
      `${this.identity.extName} ${this.identity.extVersion} : ${
        this.identity.isDev ? "DEV" : "BUILD"
      }`
    );
    this.isMounted = true;

    this.loadUniversalScripts();

    // Vue Router must be manually initialized in CEP:
    this.$router.push({ name: "home" });
  },
  methods: {
    runEditor() {
      this.progress.startIndeterminateProgress();
      // this.csInterface.evalScript(`alert('Hello?')`);
      this.csInterface.evalScript(this.monaco.editor.getValue());
      setTimeout(() => {
        this.progress.stopProgress();
      }, 1000);
    },
    dispatchEvent(name, data) {
      var event = new CSEvent(name, "APPLICATION");
      event.data = data;
      this.csInterface.dispatchEvent(event);
    },
    getContentStyle() {
      // Sets height of <v-content> and "page" element.
      // This is done to allow for interchangeable main content with certain universal components (e.g. toolbars like <navbar>)
      return `
        overflow: auto;
        margin-top: ${
          this.$route.name == "home"
            ? this.hasNavbar
              ? this.getCSS("toolbar-height") - 2
              : "0"
            : "0"
        }px;
        padding: 0px 0px 0px 0px;
        max-height: calc(100vh - ${
          this.hasNavbar ? this.getCSS("toolbar-height") : "0"
        }px);
      `;
    },
    loadScript(path) {
      // Correctly loads a script regardless of whether Animate or regular CEP app
      if (!/FLPR/.test(this.identity.appName))
        this.csInterface.evalScript(`$.evalFile('${path}')`);
      else
        this.csInterface.evalScript(
          `fl.runScript(FLfile.platformPathToURI("${path}"))`
        );
    },
    loadUniversalScripts() {
      // Preloads any script located inside ./src/host/universal
      let utilFolder = window.cep.fs.readdir(
        `${this.identity.root}/src/host/universal/`
      );
      if (!utilFolder.err) {
        let valids = utilFolder.data.filter(file => {
          return /\.(jsx|jsfl)$/.test(file);
        });
        valids.forEach(file => {
          this.loadScript(`${this.identity.root}/src/host/universal/${file}`);
        });
      }
      // Preloads any script located in ./src/host/[appName]/
      let hostFolder = window.cep.fs.readdir(
        `${this.identity.root}/src/host/${this.identity.appName}/`
      );
      if (!hostFolder.err) {
        let valids = hostFolder.data.filter(file => {
          return /\.(jsx|jsfl)$/.test(file);
        });
        valids.forEach(file => {
          this.loadScript(
            `${this.identity.root}/src/host/${this.identity.appName}/${file}`
          );
        });
      } else {
        console.log(
          `${this.identity.root}/src/host/${this.identity.appName} has no valid files or does not exist`
        );
      }
    },
    consoler(msg) {
      // Catches all console.log() usage in .jsx files via CSEvent
      console.log(`${this.identity.appName}: ${msg.data}`);
    },
    getCSS(prop) {
      // Returns current value of CSS variable
      // prop == typeof String as name of variable, with or without leading dashes:
      // this.getCSS('color-bg') || this.getCSS('--scrollbar-width')
      return window
        .getComputedStyle(document.documentElement)
        .getPropertyValue(`${/^\-\-/.test(prop) ? prop : "--" + prop}`);
    },
    setCSS(prop, data) {
      // Sets value of CSS variable
      // prop == typeof String as name of variable, with or without leading dashes:
      // this.setCSS('color-bg', 'rgba(25,25,25,1)') || this.setCSS('--scrollbar-width', '20px')
      document.documentElement.style.setProperty(
        `${/^\-\-/.test(prop) ? prop : "--" + prop}`,
        data
      );
    }
  }
};
</script>


<style>
:root {
  --quad: cubic-bezier(0.48, 0.04, 0.52, 0.96);
  --quart: cubic-bezier(0.76, 0, 0.24, 1);
  --quint: cubic-bezier(0.84, 0, 0.16, 1);
  --toolbar-height: 30;
  --bottombar-height: 20;
  --offset-height: calc(var(--toolbar-height) + var(--bottombar-height));

  --color-dark-accent: #2a2a2a;

  --mtk1: #cdcdcd;
  --mtk2: red;
  --mtk3: green;
  --mtk4: #fca369;
  --mtk5: #92d192;
  --mtk6: #f2777a;
  --mtk7: #676767;
  --mtk8: #f2777a;
  --mtk9: #fff;
  --mtk10: #cdcdcd;

  height: 100vh;
}
</style>