I see two possible paths forward.

1. Keep the existing code base but upgrade it to 1.16
2. Rewrite from scratch using Architectury

Unfortunately both of these in principle will sacrifice 1.12 compatibility. :-(

## Keep the existing code base

**Pros:** In theory, it could/should be less work to get it working.

**Cons:** We will be stuck with Forge, despite a huge craving for Fabric support on 1.16 and above.
The current bad design will be with us for the forseeable future.

## Rewrite from scratch

**Pros:** We will support both Fabric and Forge. We will be able to create a new, better architecture adapted
to our needs. This will likely make future upgrades to different versions easier. It might be possible to "trivially"
support multiple Minecraft versions from 1.16 and upwards (but there's no guarantee). We can easily clean out old functionality that is not needed any longer, and trim the code base.

**Cons:** This is a major undertaking. We're basically rewriting the mod from scratch. Even if we can copy/paste some select code from the old mod, we'll have to manually "port" over each individual functionality we want to support. In practice, we'll be stuck with a 1.12 version that is usable with little man-power to work on it, while devs are spending time re-implementing the mod for 1.16, which for a long time will be less useful to users than 1.12.
