# Regex_Match function

## Aliases

None

## Description

Checks if given regex matches the string

## Arguments

This function has 2 **required** arguments.

- source (String) → The string to be matched.
- regex (String) → The string which is regex pattern.

## Schematics

```js
REGEX_MATCH(<source>;<regex>) → (Boolean)
```

## Example

```js
REGEX_MATCH("345"; "[0-9][0-9][0-9]") → TRUE
```
