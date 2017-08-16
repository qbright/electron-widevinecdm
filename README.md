# electron-widevinecdm

[![Travis Build Status](https://travis-ci.org/webcatalog/electron-widevinecdm.svg?branch=master)](https://travis-ci.org/webcatalog/electron-widevinecdm)
[![AppVeyor Build Status](https://ci.appveyor.com/api/projects/status/github/webcatalog/electron-widevinecdm?branch=master&svg=true)](https://ci.appveyor.com/project/webcatalog/electron-widevinecdm/branch/master)
[![MIT License](http://img.shields.io/:license-mit-blue.svg)](https://github.com/webcatalog/electron-widevinecdm/blob/master/LICENSE)
[![Donate with Paypal](https://img.shields.io/badge/Donate-PayPal-green.svg)](https://www.paypal.com/cgi-bin/webscr?cmd=_donations&business=JZ2Y4F47ZMGHE&lc=US&item_name=electron-widevinecdm&item_number=webcatalog&currency_code=USD)

WidevineCDM for Electron - Allows you to run Netflix and other streaming websites in your Electron apps.

Only support 64-bit platforms.

## Installation
1. Install from npm registry
  ```bash
  yarn add electron-widevinecdm
  ```
  or
  ```bash
  npm install electron-widevinecdm --save
  ```
2. Load the plugin:
  ```js
  const { app, dialog } = require('electron');
  const widevine = require('electron-widevinecdm');

  // location to store widevine files
  const widevinePath = path.join(app.getPath('appData'), 'widevine');

  // widevineCDM needs to start running before the ready event
  // try to load widevine from widevinePath
  // it will return a boolean to tell you if the files exist or not.
  const widevineExisted = widevine.load(app, widevinePath);

  app.on('ready', () => {
    // when the app is ready
    // try to download widevine files if they don't exist.
    widevineCDM.downloadAsync(widevinePath)
      .then(() => {
        // the user needs to relaunch the app
        app.relaunch();

        dialog.showMessageBox({
          message: 'You need to relaunch the app to use widevineCDM',
          buttons: [
            'Relaunch now',
            'Cancel',
          ],
          defaultId: 0,
          cancelId: 1,
        }, (response) => {
          if (response === 0) {
            app.quit();
          }
        });
      })
      .catch((err) => {
        console.log(err);
      });
  })
  ```
