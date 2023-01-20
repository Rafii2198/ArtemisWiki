In the `wc.custom.item` package exists framework for transforming and applying properties to in-game items. This framework can be used to implement many different item-related features, such as item highlights or advanced item identifications. This page aims to describe the different aspects of this item framework and how it can be used or expanded.

## ItemStackTransformerFeature

Any custom items or item properties must be registered in this feature. It handles all transformation and property application as the items are sent to the client - this essentially replaces the item on the client, but from the server's perspective it is unchanged and does not result in any changes in behavior.

Each item or property has a corresponding match function defined, so that it only applies to the appropriate Wynncraft items e.g. crafted items, powders, etc.

## Item Properties & Property Types

An item property describes a particular aspect of a certain type of item. A property could be general and apply to lots of different items, like durability or level requirement, or it could apply only to specific items, like the tier/type of a powder. Items may have multiple properties attached to them, but not multiple instances of the same property.

Property and property type classes should be defined in `ItemProperty` so they can be referenced elsewhere. This is so they can be checked for and extracted from items using `WynnItemStack#hasProperty`, `WynnItemStack#getProperty`, and `WynnItemStack#getProperties`.

### Property Types

Some item properties might necessitate separate implementations or apply to very different items, but accomplish a similar goal that ultimately uses the same feature code. An existing example of this is item highlights, which apply to many different items that could not easily be captured in a single item property. This is where property types can be used. An item property can implement a property type to indicate it is within a "class" of properties, so that all properties of that type can be handled universally. For example, any property implementing `HighlightProperty` will have a `getHighlightColor` defined, which `ItemHighlightFeature` can use to draw the correct item highlight without having to know anything about the item itself.

An item property can implement multiple different property types, and an item may have multiple properties that implement a common type - whether each property can be handled independently depends on the type in question.

## Custom ItemStacks

In general, items are converted to `WynnItemStack` so that properties can be applied to them and accessed elsewhere in code. This is a generic `ItemStack` wrapper that adds no additional behavior on its own. For some items, however, it is desirable to more extensively manipulate or add behavior. In these cases, a custom item stack, extending `WynnItemStack`, can be defined.

Custom ItemStacks should only be used when significant behaviors or changes on an item are required, such as identified gear items. When possible, it is more desirable to implement behavior via item properties instead.