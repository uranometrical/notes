---
title: Lunar Client Launcher Documentation
wip: true
stub: true
messy: true
lastUpdated: "27 December 2021"
---
# Lunar Client Launcher Documentation
- Get the Lunar Client launcher here: https://www.lunarclient.com/download

## Introduction
The *Lunar Client launcher* (from here on out, "LCL"), is an Electron-based TypeScript program developed under the React framework, transpiled to raw JavaScript.

LCL serves several major functions:
* Downloading assets for Lunar to use.
* Verifying the integrity of asset files.
* Updating and maintaining the JVM Lunar uses as well as Lunar itself.
* Blocking access to the client in case of something such as a HWID ban.

## Documentation
LCL's source code is difficulty to navigate due to two reasons: firstly, it is heavily obfuscated, secondly, it is transpiled from TSX, a combination of TypeScript and JSX, which both already compile into harder-to-read JavaScript that is not intended to actually be observed by developers.

Assuming you are in the root directory of the launcher, all code is stored in `/resources/app.asar`, which can be unpacked using a multitude of different tools that have been developed overtime.

Documented below are the various packaged files and their functions, as long as notable contents within. This documentation includes API requests and functionality of certain core methods.

### `main.js`
`main.js` contains minimal code, essentially an `index.js`-esque file that handles the launching and some path organization.

It handles setting up the menu pages as well as constructing the window and instantiating Electron. The auto-updater is handled here as well.

#### Auto-Updater
LCL's auto-updater uses the `electron-builder` [`autoUpdater`](https://www.electron.build/auto-update.html). It is important to note that the LCL auto-updater is for the launcher itself. The launcher internally handles updating Lunar Client elsewhere.

~~* Feed URL: `Deprecated. Do not use it.`~~ Documentation of this is questionable. The `electron-builder` library deprecates this away.
~~* Request Headers:~~ See above.
  ~~* `User-Agent`: `Lunar Client Launcher v{Launcher App Version}`~~

Contents of `app-update.yml`, which are used by `electron-builder`:
```yml
provider: generic
url: https://launcherupdates.lunarclientcdn.com
publishAutoUpdate: true
useMultipleRangeRequest: false
updaterCacheDirName: lunarclient-updater
publisherName:
  - Moonsworth, LLC
```
The given `url` value is the source of installer packages.

### `renderer.js`
The `renderer.js` file contains much of the core functionality of the program, outside of the initial launch sequence.

#### Defining Variables
Before specific information is presented, consider understanding various variables that will be thrown around to lessen boilerplate and repetetiveness:
* The consistent user-agent used by the launcher is `Lunar Client Launcher v{Launcher App Version}`.
* The root Lunar Client API endpoint is `https://api.lunarclientprod.com/launcher/`.

Furthermore, JSON objects are defined in the following format:
```json
{
  "object": "description of value, as well as type",
  "other object [optional]": "if an object is annotated with \"[optional]\", it means that it is not always specified",
  "etc": {
    "if an object is written like this": {
      "that means that it is a collection, such as an array, list, or dictionary"
    }
  }
}
```

Take, for example, the `blogPost` JSON format:
```json
{
  "title": "The title of the blog post (only displayed if an image fails to load), string",
  "author": "The Minecraft username of the author, string",
  "image": "A link to an image which is displayed, string (uri format)",
  "excerpt": "The blog post's description, string",
  "link": "The link to open when \"Read more\" is pressed, string"
}
```

Now that everything is covered, please continue.

#### Blog Posts
Blog posts are the three tiled images that appear under the "Recent News" header in the LCL.

Pictured below is the anatomy of a blog post:
![&lt;image of blog post anatomy&gt;](https://loli.tomat.dev/%E2%80%8D%E2%80%8C%E2%80%8D%E2%80%8B%E2%80%8C%E2%81%A0%E2%80%8B%E2%80%8B/direct.png)
1. The "author" text, which has a head texture retrieved automatically (based on username, works with any username).
2. The image, which gets displayed based on the supplied image url.
3. The excerpt, which acts as a description.
4. The "Read more" button, which will open a browser window pointing to the link given.

The title property isn't featured here since it is only displayed as the alternate text display, not intended to normally be seen.

##### JSON Format
```json
{
  "title": "The title of the blog post (only displayed if an image fails to load), string",
  "author": "The Minecraft username of the author, string",
  "image": "A link to an image which is displayed, string (uri format)",
  "excerpt": "The blog post's description, string",
  "link": "The link to open when \"Read more\" is pressed, string"
}
```
This JSON is supplied alongside numerous other entries in a larger JSON file. More information further below.

<!-- This is as far as the rewrite currently goes. -->

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