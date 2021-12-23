---
title: Lunar Client Tab List Display
wip: true
stub: true
---
# Lunar Tab List Display
A short article documenting how to programmatically detect if a user is on Lunar Client, and the generalized steps to take to display an icon next to their name the same way Lunar does.

**The following information has only confidently applies to 1.8, and has not been tested on other versions.**
### 1.8
In 1.8.9 (and I would imagine other versions), Lunar Client users communicate to each other using a custom Lunar Client packet system. Methods of receiving these packets are currently still being researched, but Lunar Client uses a `Map` to register information about the logo that should be displayed, namely the color. The color is used to change the logo color for partners (golden yellow color).
More information to come at a later date.