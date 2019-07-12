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
  watch: {
    $route() {
      this.retrieveFromStorage();
    }
  },
  data: () => ({
    fakeCode: "text"
  }),
  created() {
    // console.log(this.$router)
    if (!this.$route.params.id)
      this.$router.push({
        name: "home",
        params: {
          id: "untitled.jsx"
        }
      });
  },
  methods: {
    retrieveFromStorage() {
      let label = this.$route.params.id;
      if (this.app.storage.getItem(label))
        this.fakeCode = this.app.storage.getItem(label);
      // this.fakeCode = JSON.parse(this.app.storage.getItem(label));
      else this.fakeCode = "";
    }
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
#container {
  height: 100%;
}

.v-content__wrap {
  display: flex;
  flex-direction: column;
  justify-content: flex-start;
}
</style>
