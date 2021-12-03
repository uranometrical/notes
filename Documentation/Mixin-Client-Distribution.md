# Mixin Client Distribtuon
- SpongePowered Mixins: https://github.com/SpongePowered/Mixin
- Original Forum Post: https://hypixel.net/threads/screw-mcp-heres-a-quick-mixin-client-distribution-tutorial.4658725
- Prerequisites: Basic understanding of mixin functionality and Gradle.
- See also: [Plunge](https://github.com/Uranometrical/plunge), a `Newtonsoft.Json`-based C# .NET 5.0 library for parsing launcher data.

## Introduction
Long have players had to deal with MCP-based clients, but that is no longer an issue!
Long gone are the days of EULA-infringing client distributon and unintentionally-encourged piracy.

This article covers how to distribute a mixin-based client using only the `.jar` file containing the mixin classes. It serves the purpose of showing a method of legal distribution that does not expose or distribute the Minecraft source code. This is for the defualt Minecraft launcher.

**This guide does not cover automation of the instructions given, it is written with an informative intent.
You will have to programmatically implement this yourself.**

### Instructions
From here on out, I will be referring to the `.minecraft` folder as the "root folder".

The root folder contains all data the launcher stores about profiles, launch arguments, launch instructions, file locations, etc.

In the root folder lies a `versions` folder, which contains numerous subfolders which store all installed Minecraft `.jar` files. The simplest way to add your own version is to create your own folder, which we'll call it `Example`.

In `Example`, you'll want to copy over the `.json` file of the version you're basing your client off of. Let's use 1.8.9. for simplicity's sake (if you're making an installer, you'd want to include this `.json` file on a server you can download from, or store it internally in the launcher/installer).

Change the `id` property in your `.json` to whatever.

(`"id": "1.8.9"` -> `"id": "My Really Cool Client"`)

This is important because of how fetching versions works. Unsure of whether you need to rename the `.jar` file located in the versions folder as well, but better safe than sorry.

There are a few ways to go about vanilla `.jar` distribution. The safest, most EULA-compliant method is to simply have them download it prior, and then programmatically copy the file over.

After doing this, you'll want to actually load your tweaker class. To load your tweaker class, you'll want to actually load your libraries.
This can be done by adding extra entries to the `libraries` proprty.

Example of what Hyperium adds:
```json
{
    "name": "cc.hyperium:Hyperium:LOCAL"
},
{
    "name": "net.minecraft:launchwrapper:Hyperium"
},
{
    "name": "optifine:OptiFine:1.8.9_HD_U_I7"
}
```

Something you should immediately notice is the names of these libraries.
1. Hyperium, which is, of course, their modification.
2. A custom build of LegacuLauncher (internally "launchwrapper").
3. OptiFine, this is configurable in Hyperium's set-up. It is loaded ALONGSIDE Hyperium and allows both to tweak the game without fuss. This means OptiFine is not embedded in the `.jar`! Hooray!

It is important to note that these are three separate `.jar` files, nothing comes included with the game's `.jar` file.

The custom LegacyLauncher build is *not* required. To keep libraries to a minimum, you can pack all your libraries with a shadow-jar as well. This will not work if no LegacyLauncher build is supplied, by the way. You will need to specify a LegacyLauncher library yourself if you do not include it in your `.jar` through shadow-jar.

These libraries are all located inside the "libraries" folder of your root folder, where they are in folder hierarchies organized by package names.

`dev.example.product:Produect:version` <-- Name in your `.json` file.

`/dev/example/product/Product/version/Product-version.jar` is the folder path (inside `.minecraft/libraries/`) for your `.jar` files.
Here's an example pattern for names:

dev.example.product:Product:version <-- Name in your .json file.

/dev/example/product/Product/version/Product-version.jar <-- Folder path (inside the libraries folder) for your .jar files.

Now for the juicy part, actually loading your tweak class.

This is as simple as adding to the `minecraftArguments` property in your `.json` file.
An example of what Hyperium does:
`"minecraftArguments": "--username ${auth_player_name} --version ${version_name} --gameDir ${game_directory} --assetsDir ${assets_root} --assetIndex ${assets_index_name} --uuid ${auth_uuid} --accessToken ${auth_access_token} --userProperties ${user_properties} --userType ${user_type} --tweakClass=cc.hyperium.launch.HyperiumTweaker"`

The basic jist is you add `--tweakClass=your.package.YourTweakerClass`, keep in mind that only one tweaker should ever be supplied.

If your mixins work in your developer environment, they will generally work here. Be careful with reflecting mapped types, though (instead opt to use AccessTransformers). That can cause issues.
