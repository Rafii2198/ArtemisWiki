# Capped function

## Aliases

None

## Description

Returns a capped value from current value and a cap.

## Arguments

This function has 2 **required** arguments.

- current(Number) → The current value.
- cap(Number) → The maximum value.

## Schematics

```js
CAPPED(<current>;<cap>) → (CappedValue)
```

## Example

```js
CAPPED(1;10) → 1/10
```
