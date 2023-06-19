# Capped_String function

## Aliases

- CAP_STR
- STR_CAP

## Description

Returns a capped value converted into a string with custom delimiter.

## Arguments

This function has 2 **required** arguments.

- value(CappedValue) → The current value.
- delimiter(String) → A string between current and max value.

## Schematics

```js
CAPPED_STRING(<value>;<delimiter>) → (String)
```

## Example

```js
CAPPED_STRING(capped_health;" out of ") → "4589 out of 6980"
```
