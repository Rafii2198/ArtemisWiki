# Regex_Find function

## Aliases

None

## Description

Replaces all matches of given regex in a string with given string

## Arguments

This function has 2 **required** arguments.

- source (String) → The string to be checked.
- regex (String) → The string which is regex pattern.
- replacement (String) → The string that will replace all matches

## Schematics

```js
REGEX_REPLACE(<source>;<regex>;<replacement>) → (String)
```

## Example

```js
REGEX_REPLACE("I am a noob";"noob";"Pro") → "I am a Pro"
```
