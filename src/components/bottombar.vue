<template>
  <div class="bottomBar" :style="getHeight()">
    <span class="version-id">{{version}}</span>
  </div>
</template>

<script>
export default {
  name: "bottombar",
  computed: {
    app() {
      return this.$root.$children[0];
    },
    version() {
      return this.app.identity ? this.app.identity.extVersion : null;
    }
  },
  methods: {
    getHeight() {
      return `
        height: ${this.app.getCSS("bottombar-height")}px;
      `;
    }
  }
};
</script>

<style>
.bottomBar {
  /* color: red; */
  /* border: 2px solid red; */
  position: absolute;
  bottom: 0px;
  display: flex;
  justify-content: flex-end;
  padding-right: 10px;
  align-items: center;
  user-select: none;
  width: calc(100vw - 2px);
  border: 0px;
  background-color: var(--color-dark-accent);
}

.version-id {
  letter-spacing: 0.75ch;
  cursor: default;
}
</style>
