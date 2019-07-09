<template>
  <!-- https://vuetifyjs.com/en/components/progress#progress -->
  <v-progress-linear
    id="progressbar"
    v-model="value"
    :active="show"
    :indeterminate="query"
    :query="true"
    :style="getProgressStyle()"
    height="4"
    :color="getProgressColor()"
  ></v-progress-linear>
</template>

<script>
export default {
  name: "progressbar",
  data: () => ({
    value: 0,
    show: true,
    query: false
  }),
  computed: {
    app() {
      return this.$root.$children[0];
    }
  },
  mounted() {
    this.app.progressbar = this;
  },
  methods: {
    getProgressColor() {
      if (!this.query && this.value == 0) return "rgb(21,21,21)";
      // else
      return "primary";
    },
    getProgressStyle() {
      return `
        position: absolute;
        top: ${this.app.getCSS("toolbar-height")}px;
        margin-top: 0px;
        left: 0px;
        z-index: 2000;
      `;
    },
    startIndeterminateProgress() {
      this.query = true;
      this.show = true;
      this.value = 0;
    },
    startStepProgress() {
      this.show = true;
      this.value = 0;
    },
    stepInProgress(index, max) {
      this.value = Math.floor(((index + 1) / max) * 100);
    },
    stopProgress() {
      this.query = false;
      this.show = false;
      this.value = 0;
    }
  }
};
</script>

<style>
.v-progress-linear {
  margin: 0px;
}
</style>
