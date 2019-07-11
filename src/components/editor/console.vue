<template>
  <div id="console" class="push-up">
    <div class="console-tab">Console</div>
    <div v-if="text.length" class="console-drawer" v-html="text"></div>
    <div v-else class="console-placeholder">Use console.log( ) to display information here</div>
    <div v-if="text.length" class="console-clear" @click="clearConsole()">
      <v-btn icon flat>
        <v-icon>mdi-close</v-icon>
      </v-btn>
    </div>
  </div>
</template>

<script>
export default {
  name: "consoler",
  data: () => ({
    hidden: false,
    text: "",
    height: 20
  }),
  computed: {
    app() {
      return this.$root.$children[0];
    }
  },
  mounted() {
    this.app.console = this;
    this.init();
  },
  watch: {
    text(contents) {
      // let consoleelt = document.getElementById("console");
      // consoleelt.scrollTop = consoleelt.scrollHeight;
      // console.log(`${consoleelt.scrollTop} => ${consoleelt.scrollHeight}`);
      this.scrollToBottom();
    }
  },
  methods: {
    processText() {
      let temptext = this.text;
      temptext = temptext.replace("let", "var");
      temptext = temptext.replace("const", "var");
    },
    getStyle() {},
    handleResize(num) {
      this.height = `${num}px`;
      let consoleelt = document.getElementById("console");
      consoleelt.style.height = this.height;
    },
    init() {
      this.app.csInterface.addEventListener("console", this.logData);
      this.app.csInterface.addEventListener("clearconsole", this.clearConsole);
    },
    hide() {
      this.show = false;
    },
    show() {
      this.show = true;
    },
    clearConsole() {
      this.text = "";
    },
    logData(msg) {
      this.text += `> ${msg.data}` + "<br>";
    },
    scrollToBottom() {
      var div = document.getElementById("console");
      div.scrollTop = div.scrollHeight - div.clientHeight;
    }
  }
};
</script>


<style>
.push-up {
  position: absolute;
  bottom: 0px;
}

#console {
  background-color: var(--color-dark-accent);
  width: 100%;
  /* min-height: 100px; */
  transform: rotate(0);
}

.console-tab {
  position: absolute;
  box-sizing: border-box;
  top: -20px;
  user-select: none;
  cursor: default;
  height: 20px;
  /* border: 2px solid red; */
  min-width: 50px;
  display: flex;
  justify-content: center;
  background-color: var(--color-dark-accent);
  align-items: flex-end;
  border-radius: 10px 10px 0px 0px;
}

.console-clear {
  position: absolute;
  top: 0px;
  right: 10px;
}

.console-drawer {
  /* border: 2px solid red; */
  padding: 10px;
  font-size: 1.5rem;
  height: 100%;
  overflow-y: scroll;
}

.console-placeholder {
  display: flex;
  justify-content: center;
  align-items: center;
  color: var(--adobe-color-scrollbar-thumb-hover);
  width: 100%;
  height: 100%;
  user-select: none;
  cursor: default;
  font-size: 2rem;
}
</style>
