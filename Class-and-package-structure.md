## Overall structure

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

The content of each of these packages is discussed in detail in this document.

### Components

Artemis consists of a well-specified hierarchy of "components". It looks like this:
```
                Managers
                   ^
                Handlers
                   ^
                Models
                   ^
Features, Functions, Commands, Screens
```

**Managers** are responsible for providing basic functionality to the mod. It could be network connectivity, handling configuration, crash reporting, etc. Managers are singletons, that are created and kept by the `core.components.Managers` class. As such, the correct way to reference a manager is e.g. `Manager.Net`. (Despite the upper case initial, this is an instance variable.) Managers are located in the parts of the `core` package they belong to.

**Handlers** are an intermediate step between Managers and Models. See [handlers](#handlers).

**Models** are the high level representation of Artemis full knowledge about the Wynncraft world. See [models](#models).

Finally, **Features, Functions, Commands, Screens** are what the user will actually interact with. They use primarily Models, but sometimes also Managers, to provide their functionality. 

* **Features** is the main basic unit for the added-value functionality that Wynntils provide the Wynncraft player. See [features](#features).

* **Functions** can be used to provide information to "info boxes". See [functions](#functions).

* **Commands** are the chat `/commands` that Wynntils provides. See [commands](#commands).

* **Screens** are the basic GUI unit of Minecraft. See [gui](#gui).

### General remarks

In the code, whenever we need to hard-code a list where the order does not matter, we strive to keep such lists sorted alphabetically. This can be for instance in the registration of components, or the i18n translations in the json resource files. Always check surrounding code for signs of alphabetic sorting, and adhere to it if present, when adding code to such lists.

## Structure of components

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

Util contains general utility classes, and general support types. 

Util contains:

* `mc`
* `wynn`
* `render`
* `colors`
* `type`

They have the following contents:

* In `util.mc`, there are Minecraft-specific utilities. Examples include `Component` to `String` conversion, or `ItemStack` manipulation. `util.mc.type` is for datatype classes that are tightly coupled to Minecraft internals.

* In `util.wynn`, there are utilities related to the Wynncraft model. Examples include parsing coordinates, or generating a percentage color scale.

* The `util.render` package contain our rendering code. 

* `util.colors` include code related to color handling, outside that of Minecraft's color handling.

* The `util.type` package contains generic datatype constructions, and functional interfaces.

* And finally, the top-level `util` package itself contains generic utility classes. To belong here, a class must not depend on anything specific to Minecraft or to Wynncraft. Examples include math, string manipulation, file system utils, and abstract functional interfaces. 

### `mc`

Mc contains the parts of Artemis that is highly tied to Minecraft internals. As a rule of thumb, if we port to a new Minecraft version, and we need to change the code, then it belongs in `mc`.

Mc contains:
* `mixin`
* `event`
* `objects`
* `utils`

The `mc.mixin` package contains all our mixins, and only mixins. (There are also two subdirectories `accessors` and `invokers`, for pure accessor and invoker mixins).

Mixin classes should be `public abstract`, and mixin methods should be `private`. `@Inject` mixins should be used, if at all possible. The method name should be the target method name followed by `Pre` or `Post`, depending on if the injection is at `HEAD` or `RETURN`. The mixin method should only call `EventFactory.on<BaseEventName>`, and possibly cancel the mixin if the event is cancelled. The `EventFactory.on<BaseEventName>` method should create and post a new event of type `<BaseEventName>Event`. Exceptions from these rules are allowed if absolutely needed, but should be kept to a minimum. Keeping to this system serves both to minimize conflict with other mods, to make the code easy to follow due to the conventions, and to facilitate debugging and hot-swapping of code.

The `mc.event` package contain all events sent from the mixins.

### `handlers`

**Handlers** are an intermediate step between Managers and Models. They are needed when a single Minecraft feature (such as the action chat line, boss bars or scoreboard) is of interest to  several unrelated Wynncraft models. They serve as a switchboard, splitting up the Minecraft part according to Wynncraft rules, and dispatching the parts out to different models. Handles should ideally know as little as possible about the *content* of these Minecraft structures, but it will need to know the Wynncraft *structure*, e.g. how coordinates are displayed in the middle of the action bar, or how the scoreboard contains different segments, such as the active quest and daily objectives.

Handlers should only be used by Models, not Features.

### `models`

**Models** are the high level representation of Artemis full knowledge about the Wynncraft world. The models should represent concepts that a Wynncraft player would use to describe the game, e.g. terms like "mana", "water damage", "haul spell", "mythic", "hub" or "daily crates".
Models can, and will likely need to, use Managers and Handlers, but also listen to events from `mc`, and occasionally also interact more directly with other aspects of Minecraft internals. They should not, however, have anything to do with e.g. how these concepts are presented on the screen. The basic representation of models is an abstract one, with classes tailored to describing the Wynncraft world in Wynncraft terms.

The `models` package contains a number of subpackages, each pertaining to a different area of the Wynncraft world. Examples include `ingredients`, `discoveries`, `map` and `character`. This division is somewhat arbitrarily and subjective, but it helps keep apart unrelated areas, and keep related functionality together.

In general, each such package will have a Model class. For instance, in `emeralds` you would expect an `EmeraldModel`, which is the main entrypoint for Features (or other Models) that need to interact with this model. Models are singletons, that are created and kept by the `core.components.Models` class. As such, the correct way to reference a model is e.g. `Models.Emeralds`. (Despite the upper case initial, this is an instance variable.)

Each individual model packages might contain any of these standard subpackages:

* `event` -- for externally visible events (i.e. sent to listeners outside the model)
* `type` -- for basic types used by the model (more or less the enums of the model)
* `profile` -- for data read from json online sources 

Additional subpackages are implementation dependent, and is used to organize the specific model in question.

### `features`

**Features** is the main basic unit for the added-value functionality that Wynntils provide the Wynncraft player. Features can be individually turned on and off, and can also have internal configuration values to tweak their behavior. Features are classified according to a Category, which is used to help the user navigate the configuration screen. A prototypical Feature queries one or more Models, and interacts with Minecraft internals, typically by listening to `mc.event` events, and updates the Minecraft client experience in a way that makes the Wynncraft gameplay better.

The majority of the Features reside in the `features.user` package. Apart from this, there are `features.debug` and `features.statemanaged`. The status of these Features is contested, since they do not really fit well into the architecture. It is likely this will change in the future, so Feature will become synonymous with what is today "UserFeature".

Features with a category is placed in a sub-package corresponding to their category. Features without a category is placed directly in `features.user`. 

Currently, the design policy has been that a feature is self-contained within a single class/file. This has lead some more complex features to become quite large. A policy for how to split up such features might be needed in the future.

Features should extend one of the three base implementations of `Feature`: `UserFeature`, `DebugFeature` or `StateManagedFeature`, corresponding to which kind of feature this is.

Every feature must be registered in `core.features.FeatureRegistry`.

### `functions`

**Functions** can be used to provide information to "info boxes". They are typically either simple util-like classes, or a shallow view of a Model.

Each individual function is represented by a class. To avoid cluttering the package with hundereds of small classes, they are combined according to area in a handful of "holder" classes, which contain nothing but nested (static inner) classes, all extending `Function`.

Every function must be registered in `core.functions.FunctionManager`.

### `commands`

**Commands** are the chat `/commands` that Wynntils provides. 

The `commands` package contain one class per command, all extending `CommandBase`.

Every command must be registered in `core.commands.ClientCommandManager`.


### `gui` (**Screens**)

**Screens** are the basic GUI unit of Minecraft. We provide several custom screens, like the Main Map, or Character Selector. Screens are initiated (and in a sense "owned") by Features.

The `gui` package has not yet been subject to as clear structure rules as the rest of the code base. For instance, this package currently also contains `gui.render`, which is our rendering support. This is subject to change.