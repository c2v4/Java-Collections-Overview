#Java Collection Overview
In order to launch the presentation you have to have `node.js` installed

## To run:
1. Install _static-server_ on your machine: `sudo npm -g install static-server`
2. Install dependencies while being in project's root: `npm install`
3. Run: `npm start`

##Development:
To ease the process of checking how does the presentation look like it is advised to use livereload. In order to do so please install it by running `sudo npm install -g browser-sync`.

After installation simply run `npm run serve`.

In case you run into trouble saving files with Intellij please disable _safe write_ option. This option basically creates temporary file and replaces original with it, however this manner does not work well with file monitoring.
[Intellij Cannot save files](https://intellij-support.jetbrains.com/hc/en-us/community/posts/115000329604-Updated-to-2017-1-3-now-getting-Cannot-save-files-error)