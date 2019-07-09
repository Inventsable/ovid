<template>
  <div class="editorContainer">
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
  </div>
</template>
 
<script>
// import MonacoEditor from "monaco-editor-vue";
// import MonacoEditor from "./monaco-editor-vue/src/index";
import MonacoEditor from "./monaco-adobe/monaco.js";
export default {
  name: "App",
  props: {
    code: String,
    lang: String
  },
  components: {
    MonacoEditor
  },
  mounted() {
    this.handleEditorSize();
    this.app.monaco = this;

    this.editor = this.$refs.editor._getEditor();
    this.theme = this.editor._theme;
    // console.log(this.editor);
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
    checkKey(evt) {
      console.log(evt);
      if (evt.key == "Enter" && evt.altKey) {
        console.log("Run!");
        this.app.runEditor();
      }
    },
    onChange(value) {
      // console.log(value);
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
      this.editorW = window.innerWidth - this.wOffset;
      this.editorH =
        window.innerHeight -
        this.hOffset -
        this.masterHOffset -
        this.topPadding;
      console.log(
        `${window.innerHeight} - ${this.hOffset} == ${window.innerHeight -
          this.hOffset}`
      );
      window.addEventListener("resize", () => {
        this.editorW = window.innerWidth - this.wOffset;
        this.editorH =
          window.innerHeight -
          this.hOffset -
          this.masterHOffset -
          this.topPadding;
      });
    },
    restyleEditor(editor) {
      // let targets = ["monaco-editor-background", "margin"];

      // let elt = document.getElementsByClassName("monaco-editor-background");
      // elt = elt[0];
      // // elt.style.backgroundColor = "red";

      let theme = editor._themeService._theme;
      theme.colors["editor.background"].rgba = { r: 10, g: 10, b: 10, a: 1 };
      console.log(theme);
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
  margin-top: 6px;
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
