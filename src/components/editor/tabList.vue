<template>
  <v-layout class="clipped-container-list">
    <v-flex xs12 sm6 @wheel="scrollTabs" class="clipped-list">
      <v-btn-toggle mandatory v-model="activeTab" active-class="active-tab">
        <v-btn v-for="(tab, i) in tabs" :key="i" flat @click="changeRoute(tab)">
          <v-icon color="#f7df1e" class="pr-1">mdi-language-javascript</v-icon>
          <span class="text-none">{{tab.label}}</span>
          <v-icon @click="closeTab(tab.label)">mdi-close</v-icon>
        </v-btn>
      </v-btn-toggle>
      <div @click="newTab()" class="add-btn">
        <v-icon>mdi-plus</v-icon>
      </div>
    </v-flex>
  </v-layout>
</template>

<script>
export default {
  name: "tabslist",
  data: () => ({
    tabs: [
      {
        label: "untitled.jsx",
        value: "/untitled.jsx"
      }
    ],
    activeTab: null,
    tabElt: null
  }),
  computed: {
    app() {
      return this.$root.$children[0];
    },
    activeItem() {
      this.tabs.forEach((tab, i) => {
        if (i == this.activeTab) return tab;
      });
    }
  },
  mounted() {
    this.tabElt = document.getElementsByClassName("clipped-list")[0];
    this.activeTab = 0;
  },
  methods: {
    getGenericTitle() {
      let untitleds = this.tabs.filter(tab => {
        return /untitled/.test(tab.label);
      });
      return `untitled${untitleds.length ? untitleds.length + 1 : ""}.jsx`;
    },
    changeRoute(tab) {
      this.$router.push({
        name: "home",
        params: {
          id: tab.label
        }
      });
    },
    newTab() {
      let newname = this.getGenericTitle();
      let newtab = {
        label: newname
      };
      this.tabs.push(newtab);
      setTimeout(() => {
        this.activeTab = this.tabs.length - 1;
        setTimeout(() => {
          this.$router.push({
            name: "home",
            params: {
              id: newname
            }
          });
        }, 10);
      }, 10);
    },
    closeTab(label) {
      this.tabs = this.tabs.filter(tab => {
        return tab.label !== label;
      });
      this.app.storage.setItem(label, "");
      if (this.tabs.length < this.activeTab)
        this.activeTab = this.tabs.length - 1;
      console.log(this.activeTab);
    },
    scrollTabs(evt) {
      if (evt.deltaY > 0) {
        this.tabElt.scrollLeft =
          this.tabElt.scrollLeft <= window.innerWidth
            ? this.tabElt.scrollLeft + window.innerWidth / 4
            : window.innerWidth;
      } else {
        this.tabElt.scrollLeft =
          this.tabElt.scrollLeft >= 0
            ? this.tabElt.scrollLeft - window.innerWidth / 4
            : 0;
      }
    }
  }
};
</script>


<style>
.clipped-list,
.clipped-container-list {
  max-height: 36px;
  box-sizing: border-box;
}

.add-btn {
  min-width: 36px;
  min-height: 36px;
  padding: 0px 6px;
  display: flex;
  justify-content: center;
  align-items: center;
  cursor: pointer;
  background-color: rgb(66, 66, 66);
  transition: 0.3s cubic-bezier(0.25, 0.8, 0.5, 1);
}

.add-btn:hover {
  background-color: rgb(75, 75, 75);
}

.clipped-list {
  overflow-x: auto;
  overflow-y: hidden;
  display: flex;
  flex-wrap: nowrap;
  justify-content: flex-start;
  /* transform: rotate(-90deg);
  transform-origin: right top; */
}

.clipped-list::-webkit-scrollbar {
  display: none;
}

.v-btn-toggle--selected {
  -webkit-box-shadow: 0px;
  box-shadow: none !important;
}

.v-btn--active {
  background-color: var(--adobe-color-scrollbar-thumb);
}

.active-tab {
  background-color: var(--adobe-color-scrollbar-thumb-hover);
}
</style>
