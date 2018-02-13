# jQuery-Style-Guide

## Purpose

The Purpose of this Style guide is to serve as both an opionionated and un-opionionated guide for medium sized projects which use jQuery extensively.

## Table of Contents

TODO

### Variable Declaration

1. Use camel Case for naming the variables
```js
var firstName;
```
2. Prefix the variables which reference jQuery Objects with $
```js
var $dialogBox = $("#dialog-box");
```
3. Initialise the variables when declaring them whenever possible. Group them to make the code much more readable.
```js
var firstName = "Chandrasekar",
    lastName = "Gokulanathan",
    company = "Sirius";
```


