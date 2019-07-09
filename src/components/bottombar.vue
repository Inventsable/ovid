<template>
  <div class="editToolbar" :style="getHeight()">
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
.editToolbar {
  /* color: red; */
  /* border: 2px solid red; */
  display: flex;
  justify-content: flex-end;
  padding-right: 10px;
  align-items: center;
  width: calc(100% - 2);
  border: 0px;
  background-color: var(--color-dark-accent);
}

.version-id {
  letter-spacing: 0.75ch;
}
</style>
