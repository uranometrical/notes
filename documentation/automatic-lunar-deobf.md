---
title: Automatic Debofuscation of the Lunar Client - A Proposal
wip: true
lastUpdated: "4 January 2022"
---
# Preface
I have been theorizing on-and-off for some time now, and I feel that now is a good time to propose my solution to the issue of Lunar Client deobfuscation. 

## The Problem
Firstly, what's the problem? Simply put, the issue lies in the fact that Lunar Client obfuscation names change with every update - and they update frequently. Keeping up with these updates is difficult, unlike maintaining mappings for programs such as Minecraft.

Furthermore, unlike Minecraft, Moonsworth entirely obfusactes member names with each release, there was never two names that match between updates.

This is further worsed by the Lunar obfuscation naming scheme, which is `[Il](25)`, resulting in nearly-identical characters (depending on the font used) creating a line of... lines.

## The Solution
Even though names are not preserved between updates, we *do* have a guaranteed anchor point, the class containing the commit ID. Anyone can view the commit ID of the latest Lunar release, as they make it very easy to locate.

This anchor point is very powerful as we can use it to trace back to other, previously-mapped names, and work from there.

Using this knowledge, it is completely within possibility to write some simple program that could create a "mapping" of connected classes this way, as well as record significant data (i.e. methods and their parameters, fields, etc.) and match them with previously-mapped classes.

Of course, no existing mapping style of this sort exists currently, and one would have to be created. For the time being, it is still viable to use this to speed up mapping updates between versions.

If this were to be put into practice, minimal interference from humans would be needed for the bare minimum action of updating mappings before doing any required maintenance work to update mappings that could not be applied, update newly-introduced classes, and more.

## The Implementation
I will need to do further research into Enigma and Recaf to figure out a viable way of implementing this, but I fully believe this is entirely within the realm of possibility. Going even further, Lunar provides data on SRG mappings of reobfuscated Minecraft names, making an implementation even easier.

If possible, an implementation should use standard Enigma mappings and store important multi-version mapping data in its own readable text file that can be parsed for data to be used by a program.
