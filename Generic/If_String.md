# If_String function

## Aliases

- IF_STR

## Description

Based on condition, returns the first value if the comparison is true, otherwise return second value.

## Arguments

This function has 3 **required** arguments.

- condition (Boolean) → The condition to be met.
- ifTrue (String) → The value which will be returned when the condition is true.
- ifFalse (String) → The value which will be returned when the condition is false.

## Schematics

```js
IF_STRING(<condition>; <ifTrue>; <ifFalse>) → (String)
```

## Example

```js
IF_STRING(TRUE;"To be";"Or not to be") → To be
```
