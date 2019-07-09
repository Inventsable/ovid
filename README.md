# cep-vue-cli-plus

## Template used in [generator-cep-vue-cli](https://github.com/Inventsable/generator-cep-vue-cli)

> **NOTE:** This repo should not be `git clone`d directly because it has breaking placeholder values and will not run on it's own.

```bash
# NPM, Yeoman and generator-cep-vue-cli are required
npm install -g yo
npm install -g generator-cep-vue-cli

# In any valid CEP extension folder:
# (e.g. <USERNAME>/AppData/Roaming/CEP/extensions)
yo cep-vue-cli
```

## No setup required for:

- [Vue](https://vuejs.org/) as JS framework
- Hot Reloading (panel updates instantly on every save during `npm run serve`)
- [Vuetify](https://vuetifyjs.com/en/getting-started/quick-start) as CSS framework
- [Vue-CLI-3](https://cli.vuejs.org/) as Node.js tooling (webkit)
- Paginations via [Vue Router](https://router.vuejs.org/)
- Full typescript support for any app with pravdomil's Adobe types (same as writing .jsx scripts but access to host DOM as autocomplete while typing)
- Full [Material Design Icon](https://materialdesignicons.com/) support
- Various personal utility components that handle context/flyout menus, launching CEF debug, common errors with Webpack, matching all host app styles and more

## [Never used Node, Vue CLI-3 or Webpack?](#Getting-Started)

---

## Commands

This panel comes with 5 commands baked in ([see details here](https://github.com/Inventsable/CEP-Self-Signing-Panel#what-do-they-do)):

- `npm run help` - A full list of the commands available and descriptions.
- `npm run switch` - Reports whether in developer or production context and can switch automatically.
- `npm run update` - Reports current version of panel in manifest and prompts to update Major, Minor, or Micro.
- `npm run register` - Reports the current user data (if any) and prompts to save new info to be used in certificates.
- `npm run sign` - Automatically stages and signs the extension, placing it in a `./archive` directory within the current panel.

---

## Filetree for panel:

Base panel results in clean and simple Single File Component infrastructure. `CSInterface` exists on the level of `App.vue` and is accessible anywhere via `this.app.csInterface` (`this.$root.$children[0].csInterface`).

:file_folder: your-panel-name
<br>&nbsp;&nbsp;|\_\_&nbsp;:file_folder: CSXS
<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;|\_\_&nbsp;:page_facing_up: manifest.xml
<br>&nbsp;&nbsp;|\_\_&nbsp;:file_folder: public
<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;|\_\_&nbsp;:page_facing_up: CSInterface.js
<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;|\_\_&nbsp;:page_facing_up: index.html (**Production:** used with `npm run build`)
<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;|\_\_&nbsp;:page_facing_up: index-dev.html (**Development:** used with `npm run serve`)
<br>&nbsp;&nbsp;|\_\_&nbsp;:file_folder: src
<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;|\_\_&nbsp;:file_folder: [components](#components)
<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;|\_\_&nbsp;:file_folder: main (utility)
<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;|\_\_&nbsp;:page_facing_up: HelloWorld.vue
<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;|\_\_&nbsp;:file_folder: [host (.jsx and scripting files)](#scripting)
<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;|\_\_&nbsp;:file_folder: plugins (Vue-CLI-3 plugins)
<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;|\_\_&nbsp;:file_folder: views (Vue Router pages)
<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;|\_\_&nbsp;:page_facing_up: App.vue
<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;|\_\_&nbsp;:page_facing_up: main.js
<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;|\_\_&nbsp;:page_facing_up: router.js
<br>&nbsp;&nbsp;|\_\_&nbsp;:page_facing_up: .debug
<br>&nbsp;&nbsp;|\_\_&nbsp;:page_facing_up: .gitignore
<br>&nbsp;&nbsp;|\_\_&nbsp;:page_facing_up: package.json
<br>&nbsp;&nbsp;|\_\_&nbsp;:page_facing_up: package-lock.json
<br>&nbsp;&nbsp;|\_\_&nbsp;:page_facing_up: vue.config.js (Avoids `file not found` errors in `index.html` after `npm run build`)

---

## Contexts

You can automate this by using `npm run switch`. In case you need to do it manually:

### For development

- Ensure `index-dev.html` is uncommented in `CSXS/manifest.xml`

```xml
  <Resources>
    <MainPath>./public/index-dev.html</MainPath>
    <!-- <MainPath>./dist/index.html</MainPath> -->
```

- Run `npm run serve` in the terminal at the project root
- Launch host application and find in Window > Extensions

> Panel now updates in real time and recompiles every time you save in VSCode

### For production

- Ensure `dist/index.html` is uncommented in `CSXS/manifest.xml`

```xml
  <Resources>
    <!-- <MainPath>./public/index-dev.html</MainPath> -->
    <MainPath>./dist/index.html</MainPath>
```

- Run `npm run build` in the terminal at the project root
- Launch host application and find in Window > Extensions

> Panel is now ready to sign and certify or be used on any client

---

## Components

:file_folder: ./src/components
<br>&nbsp;&nbsp;|\_\_&nbsp;:page_facing_up: HelloWorld.vue (main `route` defined in `./src/router.js`)
<br>&nbsp;&nbsp;|\_\_&nbsp;:file_folder: main (utilities)
<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;|\_\_&nbsp;:page_facing_up: alert.vue
<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;|\_\_&nbsp;:page_facing_up: identity.vue
<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; • Collects extension name, version, localhost and all attributes
<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; • Collects all accessible information about host environment
<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;|\_\_&nbsp;:page_facing_up: loadingscreen.vue
<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; • Intro lottie animation masks panel's launch
<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;|\_\_&nbsp;:page_facing_up: menus.vue
<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; • JSON structures for context and flyout menu
<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; • Unified event callbacks
<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; • Both menus are updated in realtime whenever modified
<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;|\_\_&nbsp;:page_facing_up: navbar.vue
<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; • Universal toolbar reactively changes per active route
<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;|\_\_&nbsp;:page_facing_up: notification.vue
<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; • Timed snackbar component
<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; • Color and text easily modified as props on `this.app`
<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;|\_\_&nbsp;:page_facing_up: progress.vue
<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; • Progressbar appended to toolbar
<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; • Built in methods for indeterminate or determinate/step loading
<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;|\_\_&nbsp;:page_facing_up: stylizer.vue
<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; • Collects application theme and dresses extension accordingly
<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; • Reassigns CSS variables to handle any future theme changes
<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;|\_\_&nbsp;:page_facing_up: version.vue
<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; • Displays current version of extension as a footer at bottom of panel

---

## Scripting

:file_folder: ./src/host
<br>&nbsp;&nbsp;|\_\_&nbsp;:file_folder: universal (Preloaded before main script)
<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;|\_\_&nbsp;:page_facing_up: Console.jsx
<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; • Support for `console.log()` and `CSEvents` in .jsx files
<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;|\_\_&nbsp;:page_facing_up: json2.jsx
<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; • Support for `JSON.stringify()` and `JSON.parse()` in .jsx files
<br>&nbsp;&nbsp;|\_\_&nbsp;:file_folder: ILST (optional)
<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;|\_\_&nbsp;:page_facing_up: host.ts
<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; • Out of the box support for host DOM in autocomplete
<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; • Changes to this file compile to `host.jsx` on every save
<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;|\_\_&nbsp;:page_facing_up: host.jsx
<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; • No need to edit or touch this file
<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; • This is the file to run from any CEP/JS
<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;|\_\_&nbsp;:page_facing_up: tsconfig.json
<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; • Run `tsc: watch - ./src/host/ILST/tsconfig.json` for DOM and autocompile on save
<br>&nbsp;&nbsp;|\_\_&nbsp;:file_folder: AEFT (optional)
<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;|\_\_&nbsp;:page_facing_up: host.ts
<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;|\_\_&nbsp;:page_facing_up: host.jsx
<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;|\_\_&nbsp;:page_facing_up: tsconfig.json
<br>&nbsp;&nbsp;|\_\_&nbsp;:file_folder: PHXS (optional)
<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;|\_\_&nbsp;:page_facing_up: host.ts
<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;|\_\_&nbsp;:page_facing_up: host.jsx
<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;|\_\_&nbsp;:page_facing_up: tsconfig.json

---

## Getting Started

You don't need to understand Node, npm packages, Vue CLI-3 or webpack to use these templates, it's a good starting point to avoid all the pitfalls in having your own functional panel using them.

I was very overwhelmed when I first jumped to Single File Components rather than using Vue's CDN and writing everything in one .js file. Afterall, this looks gigantic and there are a ton of cryptic files, but after some practice and troubleshooting how to setup the environment, it's incredibly powerful to use and can be much simpler than gigantic .js files with 10k+ worth of code!

For the most part, you don't need to alter or modify any file/folder not shown below:

:file_folder: your-panel-name
<br>&nbsp;&nbsp;|\_\_&nbsp;:file_folder: CSXS
<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;|\_\_&nbsp;:page_facing_up: manifest.xml
<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; • Changes have been made to include node context. See the README in `./CSXS`
<br>&nbsp;&nbsp;|\_\_&nbsp;:file_folder: public
<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; • Any files/folders contained here will be automatically bundled in `./dist/` after `npm run build`
<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; • You can include any assets (.pngs, scripts, etc) here or `src` for use in the panel
<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;|\_\_&nbsp;:page_facing_up: CSInterface.js
<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;|\_\_&nbsp;:page_facing_up: index.html (**Production:** used with `npm run build`)
<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;|\_\_&nbsp;:page_facing_up: index-dev.html (**Development:** used with `npm run serve`)
<br>&nbsp;&nbsp;|\_\_&nbsp;:file_folder: src
<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; • This is your development folder, you can place any number of components or files here
<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;|\_\_&nbsp;:file_folder: components
<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;|\_\_&nbsp;:page_facing_up: HelloWorld.vue
<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; • This is a placeholder component for the main content of your panel
<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;|\_\_&nbsp;:page_facing_up: App.vue
<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; • This is the main entry point of your panel
<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; • You cannot change this from a `<div>` of `#app`. Add your own components inside it instead of modifying it directly.

---

## Common errors:

### require is not a function/defined

- If wanting to use `require()` or `process` in both Developer and Production, you need to assign them manually in each .vue file (due to being nested in an iframe) like so:

```js
const require = cep_node.require || require;
const fs = require("fs"); // Now available in both
```

### Panel is not updating

- Adding or reorganizing components may cause hot reloading to fail. Many times you will be warned of this in `CEF debug`'s console, fix this by hitting `^C` in your active terminal to `Terminae batch job`, then run `npm run serve` once more and refresh the panel.

### Page Not Found (cannot find page at `localhost:####` displays in panel)

- Must run `npm run serve` and have the `App running at: -Local / -Network` message in your terminal
- If you launched the app before running `npm run serve`, click the localhost URL inside the panel's error message

### Panel is white or blank

- Check your CEF client via `localhost:####` for an error thrown in your code which breaks the panel's rendering
- If in `Production` context and receiving `404` errors in your `index.html`, ensure your `dist/index.html`'s script tags have `src` attributes such as `src=./filepath` or `src=filepath` instead of `src=/filepath` (leading slash is default but will break, should be fixed via `vue.config.js`)

### Sign/Certify is failing

- Including hidden files and repositories in your ZXP/ZIP will cause a misleading error message. Be sure to delete hidden items such as `node_modules/`, `.git/`, and any other hidden files/folders prior to your sign/certify.
