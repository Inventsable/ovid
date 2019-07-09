<template>
  <div id="container">
    <monacoeditor :code="fakeCode" lang="javascript" />
  </div>
</template>

<script>
const require = cep_node.require || require;
const fs = require("fs");

import monacoeditor from "@/components/monacoeditor.vue";

export default {
  name: "home",
  components: {
    monacoeditor
  },
  computed: {
    app() {
      return this.$root.$children[0];
    },
    root() {
      return decodeURI(window.__adobe_cep__.getSystemPath("extension")).replace(
        /file\:\/{1,}/,
        ""
      );
    }
  },
  data: () => ({
    fakeCode: "text"
  }),
  async created() {
    // this.fakeCode = fs.readFileSync(`${this.root}/src/sample.js`, {
    //   encoding: "utf-8"
    // });

    this.fakeCode = this.app.storage.getItem("doc");
  }
};
</script>

<style>
.home {
  width: 100%;
  display: flex;
  justify-content: center;
  flex-wrap: wrap;
}
</style>
