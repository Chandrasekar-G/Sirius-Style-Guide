# jQuery Style Guide

## Purpose

The Purpose of this Style guide is to serve as both an opionionated and un-opionionated guide for medium sized projects which use jQuery extensively. Few pointers can be applied to vannila JS projects too.
## Table of Contents

TODO

### 1. Variable Declaration

1.1 Use camelCase for naming the variables. It is easier to type, readable, shorter and concurs with most JS framework conventions
<sup id="1.1">[1](#f1.1)</sup>
```js
var firstName;
```
1.2 Prefix the variables which reference jQuery Objects with a $. This will be helpful to differentiate it with other JS variables.
```js
var $dialogBox = $("#dialog-box");
```
1.3 Initialise the variables when declaring them whenever possible. Group them logically to make the code much more readable.<sup id="1.3">[2](#f1.3)</sup>
```js
var firstName = "John",
    lastName = "Doe",
    company = "Sirius";

```
### 2. DOM Manipulation

2.1 Cache jQuery objects if you have to use it repeatedly<sup id="2.1">[3](#f2.1)</sup>. Chain it if possible because most jQuery methods will return the current jQuery object.
But keep in mind that its better to query and use the element directly in single use scenarios since this would be an overkill.

<b>Bad</b>
```js
    $("#dialog-box").addClass("centre");
    $("#dialog-box").css("color","red");
    $("#dialog-box").height("100px");
    $("#dialog-box").show();
```

<b>Good</b>
```js
    var $dialogBox = $("#dialog-box");

    $dialogBox.addClass("centre")
            .css("color", "red")
            .height("100px")
            .show();
```

2.2 Detcah DOM elements if you are planning to perform complex manipulations. DOM manipulations are slow when compared to ordinart program execution. So, its a better option to detach the DOM element while we work with it and re-attach it after our manipulations<sup id="2.2">[4](#f2.2)</sup>.
```js
var $table = $("#my-table"),
    $parent = table.parent();
 
$table.detach();
 
// ... add lots and lots of rows to table
 
$parent.append($table);
```
## References

<b id="f1.1">1. </b>[ camelCase vs under_scores](https://whathecode.wordpress.com/2011/02/10/camelcase-vs-underscores-scientific-showdown/) [↩](#1.1)

<b id="f1.3">2. </b>[ W3Schools - JS best practces](https://www.w3schools.com/js/js_best_practices.asp) [↩](#1.3)

<b id="f2.1">3. </b>[ Selector caching in jQuery](https://ttmm.io/tech/selector-caching-jquery/) [↩](#2.1)

<b id="f2.2">4. </b>[ jQuery Detach](http://learn.jquery.com/performance/detach-elements-before-work-with-them/) [↩](#2.2)