# Lunar Client Launcher Documentation
- Get the Lunar Client launcher here: https://www.lunarclient.com/download

## Introduction
Lunar Client's launcher is an Electron-based application used to launch Lunar Client.

It has numerous functions, mainly asset downloading and handling certain Lunar tasks, as well as managing hardware IDs and the like.

## Documentation
The launcher is heavily obfuscated; however, the obfuscation appears to not change (at least not heavily) between versions.

All code is stored in `/resources/app.asar`, which can be unpacked using a multitude of different tools that have been developed overtime.

The launcher was develped in TypeScript and transpiled to JavaScript, which we can tell thanks to the `.map` files supplied.

All the JavaScript is compiled to two files, which can make the code difficult to decipher.

### `main.js`
`main.js` contains minimal JavaScript, essentially an `index.js` that handles the launching and some patth organization.

It handles setting up the menu pages as well as constructing the window and initializing Electron settings.

The auto-updater is also handled here.

#### Auto-Updater
Lunar's auto-updater uses the `electron-builder` [`autoUpdater`](https://www.electron.build/auto-update.html).

* Feed URL: `Deprecated. Do not use it.`
* Request Headers:
  * `User-Agent`: `Lunar Client Launcher v{Launcher App Version}`

`app-update.yml`:
```yml
provider: generic
url: https://launcherupdates.lunarclientcdn.com
publishAutoUpdate: true
useMultipleRangeRequest: false
updaterCacheDirName: lunarclient-updater
publisherName:
  - Moonsworth, LLC
```

### `renderer.js`
`renderer.js` is the real "meat and potatoes", so to speak. It contains much of the core functionality of the launcher, as well as constructing the elements used for displaying pages.

#### Blog Posts
Properties:
* `title`, `string`.
* `author`, `string`.
* `image`, `string`.
* `excerpt`, `string`.
* `link`, `string`.

Head textures are retrieved automatically.

![&lt;image of blog post anatomy&gt;][https://loli.tomat.dev/%E2%80%8D%E2%80%8C%E2%80%8D%E2%80%8B%E2%80%8C%E2%81%A0%E2%80%8B%E2%80%8B/direct.png]
1. The author text, which has a head texture retrieved automatically (based on username, works with any username).
2. The image, which gets displayed based on the url given.
3. The excerpt, which acts as a description.
4. The "Read more" button, which will open a browser window pointing to the link given.

The title property isn't featured here since it is only displayed as the alternate text display, not intended to normally be seen.

### `styles.css`