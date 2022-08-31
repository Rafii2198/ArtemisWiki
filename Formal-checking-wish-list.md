Things we'd like to verify formally/automatically:

* Non-final fields in Features, that is not annotated with `@Config`, needs to be reset in an `onWorldStateChange(WorldStateEvent event)` function.

* Keybind and Overlay fields in Features must have the proper annotation and must not be static.

* Event listener methods (methods taking one argument which is-a Event) should be annotated `@SubscribeEvent`, and must not be static.