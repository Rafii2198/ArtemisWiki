All Artemis code resides in a package starting with `com.wynntils`. The rest of this discussion will ignore that part of the package name.

The top level contain these packages:
* `core`
* `utils`
* `mc`
* `handlers`
* `models`
* `features`
* `functions`
* `commands`
* `gui`

They all have well-specified content.

### `core`

Core is responsible for the framework/skeleton upon which the rest of the Wynntils mod depends. It does not deal with anything specifically related to Wynncraft, and could in theory be used to build another, similar mod for other kinds of servers. 

Core contains:
* `chat` -- support for chat handling
* `commands` -- framework for commands, not the individual commands
* `components` -- framework for managers, handlers and models
* `config` -- framework for config options and config files
* `events` -- essential additions to the Forge event bus
* `features` -- framework for features, not the individual features
* `functions` -- framework for functions, not the individual features
* `keybinds` -- support custom keybinds
* `mod` -- general stuff that is needed for modding purposes
* `net` -- network interaction and downloading, and also some essential network services
* `notifications` -- handle our internal notification system
* `splashes`

Core also contains the main entry point WynntilsMod, which also provides some high-level functionality to the entire system such as logging.

### `util`

Util contains general utility classes, and general support types. To belong here, a class must not depend on anything specific to Minecraft or to Wynncraft. Examples include math, string manipulation, file system utils, and abstract functional interfaces. 

Minecraft-specific utils belong in `mc.utils`. Wynncraft-specific utils belong in `wynn.util` (subject to change!).

### `mc`

Mc contains the parts of Artemis that is highly tied to Minecraft internals. As a rule of thumb, if we port to a new Minecraft version, and we need to change the code, then it belongs in `mc`.

Mc contains:
* `mixin`
* `event`
* `objects`
* `utils`

The `mc.mixin` package contains all our mixins, and only mixins. (There are also two subdirectories `accessors` and `invokers`, for pure accessor and invoker mixins).

Mixin classes should be `public abstract`, and mixin methods should be `private`. `@Inject` mixins should be used, if at all possible. The method name should be the target method name followed by `Pre` or `Post`, depending on if the injection is at `HEAD` or `RETURN`. The mixin method should only call `EventFactory.on<BaseEventName>`, and possibly cancel the mixin if the event is cancelled. The `EventFactory.on<BaseEventName>` method should create and post a new event of type `<BaseEventName>Event`. Exceptions from these rules are allowed if absolutely needed, but should be kept to a minimum. Keeping to this system serves both to minimize conflict with other mods, to make the code easy to follow due to the conventions, and to facilitate debugging and hot-swapping of code.

The `mc.event` package contain all events sent from the mixins. `utils` contains some Minecraft-tied utilities, like `Component` to `String` conversion, `ItemStack` manipulation etc, and `objects` finally contain a few helper classes that are tightly coupled to Minecraft internals.
