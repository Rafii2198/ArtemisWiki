What has changed between 1.12 and 1.16?

There is not only one change between 1.12 and 1.16, but changes in different areas, that affect porting.

I identify four problematic areas, that affect Wynntils and hat require different approaches to handle.

1) Obfuscated name mappings
2) Hook refactorings
3) Forge APIs
4) Mojang "utility" code refactorings

### Obfuscated name mappings

For 1.12, we have used MCP mappings. These are not available for 1.16. For 1.16, I strongly recommend using official mojang mappings.
These are not available for 1.12. So, even though this is really non-ideal, we are forced to switch mappings as part of the port.

The effect of this is that most, if not all, calls to Mojang code, will need to be modified, even if it is essentially unchanged in Mojangs code base.

**How to handle:** In theory, there might be tools that help us with this transition. But since we're also going from 1.12 to 1.16, and not just switching mappings for the same version, I have not found any. Also, a lot of this work has acttually been done in my proof of concept port.

### Hook refactorings

Apart from the "surface" change of new mappings, some functionality in Minecraft that we rely on, has been refactored or changed between 1.12 and 1.16. It might be that we subclass a Mojang class and override a method, and that method has gotten changed signature. Then our override does not work anymore. Or we might be messing around with Minecraft internals that has been rewritten, removed or refactored.

**How to handle:** To solve this, we need to understand the purpose of the current code (not always obvious, unfortunately :()), understand how the Minecraft code has changed, and re-implement the corresponding type of functionality in the new environment. In theory, some changes can actually make our solutions impossible, so we might need to have to change stuff at a higher level.

### Forge APIs

This is less of an issue if we are going to rewrite from scratch using Architectory, but in my prooof-of-concept port this was a major headache. Forge itself has changed a lot from 1.12 to 1.16, and the documentation is dismal. :( So stuff like setting up the mod itself, registering bus events, etc, is heavily broken in my PoC.

**How to handle:** Ignore the Forge API and the old version. Write a new Architectury-based mod from the scratch.

### Mojang "utility" code refactorings

This is related to, but in my eyes still separate from, issue 2. Even if we don't mess around with Minecraft internal structures, we have used a lot of "utility style" functions from the Minecraft codebase. For reading and converting chat text messages. For printing text. For drawing with GL. For rendering widgets. Etc. While this sounded good in theory, to reuse existing code, it also means we are susceptible to Mojangs refactorings. :(

**How to handle:** Either we go with the flow and just update our code to the new format of these methods. This will effectively block backporting to 1.12 though. Or we choose actively not to call Minecraft functionality directly, but instead call our internal functions (which then might in turn pass it on to Minecraft). There's certainly a charm in having our own abstraction layer, and I started implementing something along those lines with the McIf ("minecraft interface") class, which was developed in the 1.16 PoC and 1.12 code base in tandem.

However, this also has an additional cost, slightly in terms of performance but mostly in terms of work needed to keep the code base adhere to this practice. I can promise that just about every PR will try to violate this abstraction layer. And the question is if it's worth trying to keep it up.
