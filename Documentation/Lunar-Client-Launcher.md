---
title: Lunar Client Launcher Documentation
---
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

The consistent user-agent used by the launcher is `Lunar Client Launcher v{Launcher App Version}`.

#### Blog Posts
Properties:
* `title`, `string`.
* `author`, `string`.
* `image`, `string`.
* `excerpt`, `string`.
* `link`, `string`.

Head textures are retrieved automatically.

![&lt;image of blog post anatomy&gt;](https://loli.tomat.dev/%E2%80%8D%E2%80%8C%E2%80%8D%E2%80%8B%E2%80%8C%E2%81%A0%E2%80%8B%E2%80%8B/direct.png)
1. The author text, which has a head texture retrieved automatically (based on username, works with any username).
2. The image, which gets displayed based on the url given.
3. The excerpt, which acts as a description.
4. The "Read more" button, which will open a browser window pointing to the link given.

The title property isn't featured here since it is only displayed as the alternate text display, not intended to normally be seen.

#### Folders & Files
The launcher shares numerous folder and file locations with the actual client.

It cares about:
* `.lunarclient/settings/`
* `.lunarclient/jre/`
* `.lunarclient/offline/`
* `.lunarclient/textures/`
* `.lunarclient/launcher-cache/`
* `.lunarclient/licenses`

In the `luancher-cache` folder lies a text file called `hwid-private-do-not-share`, which contains a generated hardware ID used by the client, which is included in web requests.

The hardware ID is a string composed of 512 characters, and contains a randomized combination of the characters `0-9A-Za-z`.

#### Error Throwing
The launcher will record thrown exceptions and will aid you in handling some of them.

##### Out-of-Memory Errors
If the exception contains a message stating "Could not reerve enough space for object heap", "Could not reserve enough space for" <!-- yes, it ends here -->, or "bitmaps for parallel garbage collection for the requested", the luancher will assume your game ran out of memory and will recommend you to allocate more.

If the exception contains "org.lwjgl.LWJGLException: Pixel format not accelerated", it will tell you to visit https://lunarclient.com/pixel-format.

If the exception does not fit either of these, the launcher will record the data and send it to Lunar's servers automatically. If it fits one of these above exceptions, it will not send a report.

JSON data sent upon launch failure:
* `type`:
* `timeToInit`: Initialization time.
* `timeToStarted:` Start time.
* `os`: Your platform.
* `version`: The game version, short-form (1.8, 1.7, 1.16, etc.).
* `logs`: The most relevant 100 lines of logging.

#### Web Requests
The API endpoint is `https://api.lunarclientprod.com/launcher/`.

##### Report Launch Status
* Endpoint: `/reportLaunchStatus`
* Method: `POST`
* Headers:
  * `User-Agent`: Standard
* Body:
  * JSON file of the following:
    * `hwid`: machineIDsync
    * `hwid-private`: Hardware ID as specified above in `Folders & Files`.
    * `os`: Your platform;
    * `arch`: Your process architecture.
    * `launcher_version`: The launcher version.
    * `success`: Determined by a passed parameter. True of launched successfully, otherwise false.
    * `data`: Determined by a passed parameter. The purpose can vary.

##### Launch
* Endpoint: `/launch`
* Method: `POST`
* Headers:
  * `User-Agent`: Standard
* Body:
  * JSON file of the following:
    * `hwid`: machineIDsync
    * `hwid-private`: Hardware ID as specified above in `Folders & Files`.
    * `os`: Your platform;
    * `arch`: Your process architecture.
    * `launcher_version`: The launcher version.
    * `version`: The ID of a larger JSON object, i.e. `1.8`. Further documentation required.
    * `branch`: The currently-selected branch (experimental options).
    * `launch_type`: Typically `OFFLINE`, never seen it as anything else. Unsure of its purpose.
    * `classifier`: The classifier, such as `optifine` if OptiFine is running.

##### Download Licenses

### `styles.css`