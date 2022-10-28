Here are some, hopefully non-controversial, suggestions on how we should think about a new port.

## Intermediate layer for mixins

* To facilitate supporting both Fabric and Forge, we should keep to the common ground as much as possible. That means no reliance on Forge events, or Fabric API, if possible. In practice, this means writing and maintaining our own set of mixins. 

* To make this as future-proof as possible we should strive to:
  * Minimize the number of mixins to the bare minimum.
  * Select our mixin spots with care.
  * Never ever do any hard logic in the mixin code, but just pass control on to our intermediate layer.
  * The intermediate layer will abstract the functionality to a usable API. Somewhat like how Fabric API/Forge bus events works, but we will own and control the API ourselves.
  * The mixin classes should only call the intermediate layer, and go no deeper into the product code.
  * Only the intermediate layer should know about the mixins. All other classes access this functionality through the intermediate layer.

## Modelling / Overall architecture

* A lot of the current code mixes handling input from Wynncraft with building and maintaining a model with sending output to Wynncraft with modifying the UI or with interacting with the Minecraft client. We need to separate those, so they become easier to maintain, reuse and update to future versions. 

* As I see it, we have two conceptual models we need to handle: The Wynncraft world, and the Minecraft client/UI. The litmus test for determining if it's the one or the other is that if Wynncraft updates, we should update our Wynncraft model, but if Minecraft updates, we should update the Minecraft model. **These need to be cleanly separated** or we will get into a support matrix hell. For the rest of the document, I'll assume we have `com.wynntils.wynn` for the Wynncraft model, and `com.wynntils.mc` for the minecraft model. (These names is perhaps too similar, I'm open for other suggestions. `com.wynntils.model` and `com.wynntils.client` was my first choice, maybe that's better.)

* The Minecraft model will need to handle input and output and a lot that's neither. It will need to handle stuff like adding key bindings, a play button to the in-game menu, how to draw overlays, how to send spells (or at least how to send fake mouse presses; the actual spell order is strictly a Wynncraft model knowledge), etc.

* The Wynncraft model will need to handle input from Wynncraft (which typically comes either as Minecraft packages or text strings), parse/handle this input, and build a local model. In general, I believe we have been too bad at caching the data we have successfully parsed. I propose that we make the caching compulsary by designing it into the model. So let's say if we want to check if we are riding on a horse. Whenever we mount a horse, our Wynncraft model will detect this happening, and update the local model to set `isRidingHorse` to `true`. (And when dismounting, resetting it to `false`.) If some part of the code, perhaps code that wants to spawn the horse (or maybe new functionality that warns you if you try to cast spells while riding), then they will query the model, and just check the local `isRidingHorse` variable.

Apart from understanding the Wynncraft world in Wynncraft terms (are you on the hub? Are you tracking a quest? Is this an NPC dialogue?), we also need to be able to affect Wynncraft, e.g. by sending chat commands (/toggle music, /ping) or raw packages.

This all means that the Wynncraft model breaks down in three parts: 
  * Parsing (input)
  * Storing/caching
  * Changing (output)

## Enabling/disabling of individual features

We currently have a quite okay way of enabling/disabling individual features. I believe this is good, almost essential, for Wynntils. With that said, I think it could be worth to spend a bit of effort to make sure we:
* Has found the right place to check if a feature is enabled or disabled
* Really has a proper way of splitting down the functionality.

At the very least, I believe the current structure of "modules", and putting almost all functionality into "overlays", serves little purpose, and only confuses the code base and the settings UI. My suggestion is basically starting with a flat set of booleans to enable/disable certain functionality, and then we can work with finding a logical way of grouping these in the Settings UI. 

## External data

Static, non-code resources that can change independently of the code base, like the world map, item database, map POIs, etc, should not be included in the mod itself, but downloaded and cached locally.

Such resources should be downloaded from a single Resource Provider instance that we control (read: Athena). If the source of the data is some third party (the Wynncraft API counts as a third party in this case), we will still download the data from the third party and store it on our Resource Provider. We will *not* access static data directly from any other instance.

Data stored on our Resource Provider should be organized in a comprehensible way, and should use standard format. For most textual data, this means JSON.

Dynamic data (like server uptime) will still need to be provided by external services (unless we choose to proxy/cache even such data?). Such data retrieval needs to be handled with extra care. We can't affect its availability. It can change format suddenly and unexpectedly. So we must handle failures in retrieving and parsing such data gracefully.