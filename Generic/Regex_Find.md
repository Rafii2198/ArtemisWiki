# Regex_Find function

## Aliases

None

## Description

Checks if given regex finds the string

## Arguments

This function has 2 **required** arguments.

- source (String) → The string to be checked.
- regex (String) → The string which is regex pattern.

## Schematics

```js
REGEX_FIND(<source>;<regex>) → (Boolean)
```

## Example

```js
REGEX_FIND("testing, testing"; "test") → TRUE
```
