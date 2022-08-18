This list is by no means complete, but it's a kind of simple reminder of functionality in the Legacy module that we (might) want to port to Artemis.


* Banners for level up, discovery found
* Keep track of friends and guild members logging in (notify?)
* Sounds: horse whistle, war horn, quest book page turning etc
* Support for game mode like iron man (do to what? shown in character selector)
* Inventory data, such as total amount of money, ingredients etc
* Settings GUI
* Save settings to cloud (?)
* Item database
* Load resource pack at startup
* Transliterate Wynnic/Gavellian
* Automatic update of mod (including `/forceupdate` command)
  - check at startup
  - check during gameplay and allow immediate download (I don't recommend we do this)
  - chose between stable and beta (cutting edge) channels
* Music from MP3 instead
* Quest book:
   - open on wiki
* Chat manager:
   - timestamp on messages
   - play sound on mention (getMasterRecord(SoundEvents.BLOCK_NOTE_PLING, 1.0F)) and mark chat tab)
   - change chat background transparency
   - stack repeating messages
   - change chat history length
   - chat tabs
   - filter chat tabs per content / regexp match
   - clickable coordinates
   - clickable partyy join/trade/duel requests (present in Wynncraft now???)
   - transliterate Wynnic/Gavellian, possibly in hover
   - convert your message to wynnic/gavellian
* chat tab manager gui
   - create, delete, reorder tabs
   - change which content goes to which tab
* ~~play popoff sound (blaze hurt) if you can't put on some armor~~ *Reason: Added by Wynncraft*
* print message to chat when clicking with soul point or compass. (do we need this?)
* /compass command: `/compass [<x> [<y>] <z> | <direction> | clear | share [location] [guild|party|user]`
* changelog functionality, incl `/wynntils changelog`
* Automatically turn of /toggle music and /toggle autotrack
* totem tracker
   - track mob totem location and time
   - track shaman totem location (?) and time
* detect gathering and damage (used for damage measuring, and gathering crowd sourcing)
* custom class/character selector
* party management GUI
* keep track of current location in background
* user cosmetics (cape, ears)
* track other players (are they mutual friends with us; track position on server)
* ping manager (measure time from sending `/toggle` to getting response)
* player list ("tab" key pressed) replacement