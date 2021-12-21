If we chose to go with creating a new rewrite of the mod, this is how I suggest we do it.

1) We make some kind of rough list of features and functionality in the current mod. We try to prioritize this somewhat. There are two conflicting kinds of priorities here, what's useful for the end user wanting to switch, and what's a good learning curve for us as developers. I suggest we start by some simple functionalities just to get the hang of it, and then try to progress to the most important parts of the mod.

2) We then select a feature, and start porting it. 

    * As a first attempt, we can probably copy some chunks of code from the current mod, the part that contains most of the "real" work done by Wynntils. As an example, let's use the background in inventory UIs showing rarity color. The "real" work consists of a) identifying the rarity level of an item, and b) modifying the UI to reflect this.

    * We then need to adapt the code to the improved architecture/design of the new mod. This could mean moving the functionality that parses an ItemStack to determine rarity into something like `com.wynntils.model.item.Rarity.parse(ItemStack itemStack)`. Or rather, starting by looking if we already has created such a function (this is not unlikely -- a lot of functionality like that is current copied or duplicated all over the code base), and reuse it. Or if there is a similar function, modify it to cover this case too.

    * In parallel with this, we also need to adapt the code to the new 1.16/mixin reality. This will likely require changes to how we draw the background. Ideally, we will build up some kind of framework as we go, like `com.wynntils.ui.Inventory.postItemStackDrawdraw(<some lambda>)`. And then we need to implement this functionality using mixins/Forge/Fabric/Architectury (ideally, making as few assumptions and dependencies as possible and keeping the code as general and future-proof as possible). 

    * At this point, the functionality can be considered "Done", and we can move on to the next.

It is worth noting that point 2) here is possible to parallelize. That is, we can as a team work on multiple features at the same time, with relatively low risk of merge conflicts, as long as we pick features from the list from 1) that are not too likely to work on the same parts of the code.