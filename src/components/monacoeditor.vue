<template>
  <div class="editorContainer">
    <split-pane
      v-on:resize="resize"
      id="contents"
      :min-percent="0"
      :default-percent="80"
      split="horizontal"
    >
      <template id="paneL" slot="paneL">
        <div :style="padTop()"></div>
        <MonacoEditor
          id="editor"
          ref="editor"
          :width="editorW"
          :height="editorH"
          theme="vs-dark"
          :value="code"
          :language="lang"
          :options="options"
          @change="onChange"
          @keyup="checkKey"
        ></MonacoEditor>
      </template>
      <template id="paneR" slot="paneR">
        <consoler />
      </template>
    </split-pane>
  </div>
</template>
 
<script>
import splitPane from "vue-splitpane";

import consoler from "./editor/console";
import MonacoEditor from "./monaco-adobe/monaco.js";
export default {
  name: "App",
  props: {
    code: String,
    lang: String
  },
  components: {
    MonacoEditor,
    consoler,
    "split-pane": splitPane
  },
  mounted() {
    this.handleEditorSize();
    this.app.monaco = this;
    this.editor = this.$refs.editor._getEditor();
    this.theme = this.editor._theme;
    this.restyleEditor(this.editor);
    let editor = document.getElementById("editor");
    editor.addEventListener("keyup", evt => {
      this.checkKey(evt);
    });
  },
  data: () => ({
    wOffset: 0,
    editorW: 200,
    editorH: 200,
    masterHOffset: 4,
    topPadding: 8,
    editor: null,
    theme: null,
    options: {
      //Monaco Editor Options
      scrollBeyondLastLine: false,
      lineDecorationsWidth: "0px",
      lineNumbersMinChars: 4,
      autoIndent: true,
      formatOnPaste: true,
      formatOnType: true
    }
  }),
  computed: {
    app() {
      return this.$root.$children[0];
    },
    hOffset() {
      return (
        +this.app.getCSS("toolbar-height") +
        +this.app.getCSS("bottombar-height")
      );
    }
  },
  methods: {
    resize(evt) {
      this.handleEditorSize();
      this.app.console.handleResize(
        this.$el.children.contents.children[2].offsetHeight
      );
    },
    checkKey(evt) {
      if (evt.key == "Enter" && evt.altKey) {
        this.app.runEditor();
      }
    },
    onChange(value) {
      this.app.storage.setItem("doc", value);
    },
    padTop() {
      return `
        background-color: #1e1e1e;
        width: 100%;
        height: ${this.topPadding}px
      `;
    },
    handleEditorSize() {
      let paneL = document.getElementById("paneL");
      this.editorW = window.innerWidth - this.wOffset;
      this.editorH = this.$el.children.contents.children[0].offsetHeight - 7;
      this.app.console.handleResize(
        this.$el.children.contents.children[2].offsetHeight
      );
      window.addEventListener("resize", () => {
        this.editorW = window.innerWidth - this.wOffset;
        this.editorH = this.$el.children.contents.children[0].offsetHeight - 7;
      });
    },
    restyleEditor(editor) {
      let theme = editor._themeService._theme;
      theme.colors["editor.background"].rgba = { r: 10, g: 10, b: 10, a: 1 };
    }
  }
};
</script>


<style>
.editorContainer {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: centers;
  width: 100%;
  height: calc(100vh - 56px);
  margin-top: 6px;
  overflow: hidden;
}

.monaco-editor,
.monaco-editor-background,
.monaco-editor .inputarea.ime-input {
  background-color: red;
}

.current-line {
  border: 0px solid transparent !important;
  background-color: var(--color-dark-accent);
}

.mtk1 {
  color: var(--mtk1) !important;
}

.mtk2 {
  color: var(--mtk2) !important;
}

.mtk3 {
  color: var(--mtk3) !important;
}

.mtk4 {
  color: var(--mtk4) !important;
}

.mtk5 {
  /* String */
  color: var(--mtk5) !important;
}

.mtk6 {
  /* Number */
  color: var(--mtk6) !important;
}

.mtk7 {
  color: var(--mtk7) !important;
}

.mtk8 {
  /* Keyword like let/var JS */
  /* tags for HTML */
  color: var(--mtk8) !important;
}

.mtk9 {
  /* Default */
  color: var(--mtk9) !important;
}

.mtk10 {
  /* Default */
  color: var(--mtk10) !important;
}
</style>
