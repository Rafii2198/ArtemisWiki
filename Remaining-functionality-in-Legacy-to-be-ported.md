This list is by no means complete, but it's a kind of simple reminder of functionality in the Legacy module that we (might) want to port to Artemis.

* Banners for level up, discovery found
* Keep track of friends and guild members logging in (notify?)
* Sounds: horse whistle, war horn, quest book page turning etc
* Support for game mode like iron man (do to what? shown in character selector)
* Save settings to cloud (?)
* Load resource pack at startup (done but still needs a reload)
* Transliterate Wynnic/Gavellian
* Automatic update of mod (including `/forceupdate` command)
  - chose between stable and beta (cutting edge) channels
* Music from MP3 instead
* Chat manager:
   - timestamp on messages
   - play sound on mention (getMasterRecord(SoundEvents.BLOCK_NOTE_PLING, 1.0F)) and mark chat tab)
   - change chat background transparency
   - stack repeating messages
   - change chat history length
   - transliterate Wynnic/Gavellian, possibly in hover
   - convert your message to wynnic/gavellian
* chat tab manager gui
   - create, delete, reorder tabs
   - change which content goes to which tab
* print message to chat when clicking with soul point or compass. (do we need this?)
* changelog functionality, incl `/wynntils changelog`
* Automatically turn off /toggle music and /toggle autotrack
* totem tracker
   - track mob totem location and time
   - track shaman totem location (?) and time
* detect gathering and damage (used for damage measuring, and gathering crowd sourcing)
* party management GUI
* user cosmetics (cape, ears)
* ping manager (measure time from sending `/toggle` to getting response)
* player list ("tab" key pressed) replacement
