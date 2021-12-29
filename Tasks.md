## Porting admin tasks
* Make a comprehensive list of [[Legacy Wynntils features]]

## Starter features to implement
### 1. Wyncraft connection button
* **Rationale:** Adding button to existing menu. Loading texture resource. Working with Minecraft connection internals. Easy to test and debug, just start Minecraft.

### 2. World map
* **Rationale:** create own screen GUI. Load static content from Athena. No WC server connection truly needed. Load Wynntils texture resource for the arrow representing the player's location. sUseful functionality.
* **Initial limitations:** Only show the PNG, no POI markers, no Waypoint markers, no labels, no compass target.

### 3. Spell casting buttons
*  **Rationale:** Integration with key bindings and Minecraft client. Getting character status from server. Useful functionality.

### 4. Game ticker overlay + capture "Your inventory is full"
* **Rationale:** Rendering overlays. Rendering fonts. Capturing and parsing chat messages. Building framework for sending messages to ticker. Initial use (inventory full message) is just the simplest possible use case.
* **Initial limitations:** No free placement of overlay on the screen.

### 5. Color item rarity background
* **Rationale:** Hooks into Inventory UI. Rendering changes in pre-existing GUIs. Item parsing from Wynncraft server. Useful functionality.
* **Initial limitations:** Only pre-defined colors.

### 6. Lootruns
* **Rationale:** In-world rendering. Tight integration with Minecraft internals to track movement. Command handling using Brigadier. Architectural decisions about large amount of abstract code. Useful functionality.
* **Initial limitations:** Do not draw lootrun on world map. No chest detection or display needed.
