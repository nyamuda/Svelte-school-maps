# Media Hack Svelte starter template

A basic setup for Svelte apps created by [Media Hack Collective](https://mediahack.co.za).

You are free to use this template for your own projects but please do not use Media Hack/Outlier logos without permission.

## Use this template

Requires: Node/NPM

Use this template to start a new Svelte project:

`npx degit mediahackza/svelte-template project-directory`

Replace "project-directory" with your preferred project name.

## Notes

### Styles

- A global stylesheet is included in the "public" directory.

### Plugins & features

- [Rollup JSON plugin](https://www.npmjs.com/package/@rollup/plugin-json) & [Rollup SVG plugin](https://www.npmjs.com/package/rollup-plugin-svg) for importing these formats directly
- Google-fetch in the "scripts" directory pulls in data from either a Google Doc or Sheet (see below)
- [ArchieML](http://archieml.org/) is used for reading ArchieML formatted Google Docs into local JSON/CSV files ([example use](https://github.com/mediahackza/svelte-template/blob/main/ARCHIESTYLES.md)).
- [LayerCake](https://layercake.graphics/) is included for chart making (still in testing)
- [Svelte-spa-router](https://github.com/ItalyPaleAle/svelte-spa-router) is included for client-side routing where necessary.
- [Leaflet.js](https://leafletjs.com/) included for map making.

### Importing Google docs data

Running the gdoc script will import Google Docs or Sheets into a local folder. This is borrowed from the [Pudding Svelte starter](https://github.com/the-pudding/website). To use:

- Create a Google Doc or Sheet
- Click Share button -> advanced -> Change... -> to "Anyone with this link"
- In the address bar, grab the ID - eg: ...com/document/d/**1IiA5a5iCjbjOYvZVgPcjGzMy5PyfCzpPF-LnQdCdFI0**/edit
- Paste the ID from above "google.config.cjs", and set the filepath to where you want the file saved
- If you want to do a Google Sheet, be sure to include the gid value in the url as well.
- RUn the update script to get the latest data:

`npm run gdoc`

## Related documents, influences & sources

- [Svelte Data Viz notes](https://github.com/alastairotter/svelte-data-viz): A basic guide to working with Svelte for data visualisation.
- [The Pudding Github](https://github.com/the-pudding/website): An excellent description of the Svelte setup for the [The Pudding](https://pudding.cool/) website.
