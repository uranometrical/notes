---
title: Mantle
lastUpdated: "27 December 2021"
---
# Mantle
- Website: https://mantle.gg/
- Supported Versions: All
- FOSS status: Closed-source, trivially easy to replicate.

## Information
Mantle describes itself as an "awesome cape system which works with your favorite clients"[^1], as works on many different clients, such as Badlion, Lunar, Forge, and OptiFine[^1]. Mantle replaces OptiFine capes with its own capes.

Mantle depends on its own cape server, which can be found [here]("https://capes.mantle.gg") (https://capes.mantle.gg).

To access someone's cape, follow the endpoint request format: `https://capes.mantle.gg/capes/{Username}.png`. If there is no image found, then the specified user does not have a cape.

### Mantle - In Theory
**The information presented here is only speculative, and should not be cited as matter-of-fact.**
There are a couple of ways to achieve what Mantle accomplishes.

##### Method 1: Redireting Web Requests
Through one way or another, redirecting requests to `s.optifine.net` to `capes.mantle.gg` (or your own cape server). This can be done through different methods depending on your operating system as well as other variables.

The simplest method would be to connect through a proxy which would manage these requests. If this is not an available option, there are methods such as editing your hosts file to redirect `s.optifine.net` to `capes.mantle.gg` or some other, self-hosted cape services[^2].

##### Method 2: Java Bytecode Manipulation
Manipulating Java bytecode, whether it be at runtime or by permanently editing a `.class` file (located in `.jar`s, which are glorified `.zip` files) is an alternative to redirecting web requests.

OptiFine specifies its cape server as `http://s.optifine.net`. Simply searching for this and replacing it with your own (i.e. `http://capes.mantle.gg`) is all that is needed.

Full text to search for: `LDC "http://s.optifine.net"`.

### Hosting a Mantle Alternative
The bare minimum required to host your own alternative to Mantle is to simply host your own program that would accet HTTP GET requests at `/capes/{Username}.png`, and returning an image if the specified user has an associated cape.

### Natively Incorporating Mantle
Natively implementing Mantle in your modification or client alternative is very simple in concept as long as OptiFine is loaded, since Mantle directly depends on it.

Follow the method two listed in "Mantle - In Theory" above to replicate Mantle. If you want to incorporate Mantle alongside OptiFine while allowing overlap, that is more challenging and would require tweaking OptiFine. The cleanest way to implement this would be to override the code that OptiFine uses to resolve its capes, and check at your own endpoint (such as `capes.mantle.gg`) if `s.optifine.net` fails to resolve a cape.

[^1]: https://mantle.gg/
[^2]: https://github.com/sadcenter/mantle#what-i-have-to-do-on-the-client-side