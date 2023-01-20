All Artemis code resides in a package starting with `com.wynntils`. The rest of this discussion will ignore that part of the package name.

The top level contain these packages:
* `commands`
* `core`
* `features`
* `functions`
* `gui`
* `handlers`
* `mc`
* `models`
* `utils`

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

