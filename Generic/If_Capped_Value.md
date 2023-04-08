# If_Capped_Value function

## Aliases

- IF_CAPPED
- IF_CAP

## Description

Based on condition, returns the first value if the comparison is true, otherwise return second value.

## Arguments

This function has 3 **required** arguments.

- condition (Boolean) → The condition to be met.
- ifTrue (CappedValue) → The value which will be returned when the condition is true.
- ifFalse (CappedValue) → The value which will be returned when the condition is false.

## Schematics

```js
IF_CAPPED_VALUE(<condition>; <ifTrue>; <ifFalse>) → (CappedValue)
```

## Example

```js
IF_CAPPED_VALUE(FALSE;CAPPED_HEALTH;CAPPED_MANA) → 100/100
```
