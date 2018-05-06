# jQuery Style Guide

## Purpose

The Purpose of this Style guide is to serve as both an opionionated and un-opionionated guide for medium sized projects which use jQuery extensively. Few pointers can be applied to vannila JS projects too.

<a name="Variable-Declaration"></a>
### 1. Variable Declaration

1.1 Use camelCase for naming the variables. It is easier to type, readable, shorter and concurs with most JS framework conventions
<sup id="1.1">[1](#f1.1)</sup>
```js
var firstName;
```
1.2 Prefix the variables which reference jQuery Objects with a $. This will be helpful to differentiate it with other plain JS variables.
```js
var $dialogBox = $("#dialog-box");
```
1.3 Initialise the variables when declaring them whenever possible. Group them logically to make the code much more readable.<sup id="1.3">[2](#f1.3)</sup>
```js
var firstName = "John",
    lastName = "Doe",
    company = "Sirius";

```
<a name="DOM-Manipulation"></a>
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

2.3 In cases where you're appending a lot of elements to the Dom, append them all at once instead of one at a time because touching the Dom comes at a cost<sup id="2.3">[5](#f2.3)</sup>.

<b>Bad</b>
```js
var $userList = $("#user-list"),
    users = ["Greg", "Peter", "Kyle", "Danny", "Mark"];

$.each(users, function(user) {
    $userList.append("<li>" + user + "</li>");
});

```
<b>Good</b>
```js
var $userList = $("#user-list"),
    users = ["Greg", "Peter", "Kyle", "Danny", "Mark"],
    userDom = "";

$.each(users, function(user) {
    userDom += "<li>" + user + "</li>";
});

$userList.append(userDom);

```

<a name="Selectors"></a>
### 3. Selectors

3.1 Leveraging IDs is by far the fastest way to select an element<sup id="3.1">[6](#f3.1)</sup>. Though its not possible in all the cases, its preferable to use them when you can.

3.2 When using class selectors don't use the element type since, it'll slow down the selection<sup id="3.2">[7](#f3.2)</sup>.
```js
var $products = $("div.products");      // SLOW
var $products = $(".products");         // FAST
```

3.3 Give a context to your selectors. By default, when you pass a selector into jQuery, it traverses the entire DOM. There’s an underused,
second possible context argument into jQuery that limits that search to a specific section of the
DOM. In fact, the best way is to use .find(). Its faster than the context by 40% <sup id="3.3">[8](#f3.3)</sup>


```js
$(".className");                    // SLOW :: This traverses the whole DOM
$(".className","#id")               // FASTER
$("#id").find(".className")         // FASTEST
```

3.4 Avoid excess specificity and usage of universal selectors.

<a name="Event-Handling"></a>
### 4. Event Handling

4.1 Using the same anonymous functions in multiple event handlers violates the DRY(Don't repeat yourself) principle. When we have to make modifications we'll have to edit similar code that lives in many different places. A better approach would be to craete a callable function accessible from anywhere<sup id="4.1">[9](#f4.1)</sup>. However it's okay to go for anonymous functions when its not going to be used anywhere else.

<b>Bad</b>
```js
$("#user-dialog-box").on("click", function() {
    // Logic
});
$("#guest-dialog-box").on("click", function() {
    // Same Logic
});
```

<b>Good</b>
```js
function dialogHandler() {
    // Logic
};
$("#user-dialog-box").on("click", dialogHandler);
$("#guest-dialog-box").on("click", dialogHandler);
```

We could further simplify this code by grouping common selectors together,
```js
$("#user-dialog-box", "#guest-dialog-box")
    .on("click", dialogHandler);
```

4.2 While attaching multiple events to an element, it's more readable to pass an object instead of attaching each of the handlers individually<sup id="4.2">[10](#f4.2)</sup>. 

<b>Bad</b>
```js
$("#user-list").on("mouseenter",function(){
  // mouseenter action
});

$("#user-list").on("mouseleave",function(){
  // mouseleave action
});
```
<b>Good</b>
```js
$("#user-list").on({
  "mouseenter": function(){
    // mouseenter action
  },
  "mouseleave": function(){
    // mouseleave action
  }
});
```

<a name="Code-Organization"></a>
### 5. Code Organization

5.1 Singleton / object literal Pattern

It is a common practice in smaller jQuery projects to wrap the entire JS code within a $document.Ready(). When the size grows, this block will have tens or even hundreds of methods and properties. 
This problem will further be aggrevated whith multiple js files used in the project, populated with multiple variables.

One solution to this is to use a single object literal to contain the properties and variables of a JS file which will act as a namespace to access the methods and properties.

```js
var userController = {
    
    // Variable initialization
    firstName : "",
    lastName : "",
    company : "",

    init : function(){
        //code to initialize your application
    },

    listeners: function() {
        //your listeners will go here
    },

    getUserDetails: function() {
        // Logic
    },

    showUserDetails: function() {
        // Logic
    }
    ...
    ...
};

$(document).ready(function(){
    userController.init();
    userController.listeners();
});
```

5.2 Module Pattern

One downside of using the object literal pattern is it does not offer privacy for variables and functions. Module pattern build upon the singleton pattern by offering privacy through a self-executing anonymous function that returns an object.

```js
var userController = (function(){
    var firstName = "",
        lastName = "",
        company = "";

    var init = function(){
        //code to initialize your application
    },

    var listeners = function() {
        //your listeners will go here
    },

    var getUserDetails = function() {
        // Logic
    },

    var showUserDetails = function() {
        // Logic
    }

    return {
        init: init,
        listeners: listeners
    }

})();

$(document).ready(function() {
    userController.init();
    userController.listeners();
});
``` 

Here, only the init and listener methods are exposed and the other variables are within the scope of the controller.

## References

<b id="f1.1">1. </b>[ camelCase vs under_scores](https://whathecode.wordpress.com/2011/02/10/camelcase-vs-underscores-scientific-showdown/) [↩](#1.1)

<b id="f1.3">2. </b>[ W3Schools - JS best practces](https://www.w3schools.com/js/js_best_practices.asp) [↩](#1.3)

<b id="f2.1">3. </b>[ Selector caching in jQuery](https://ttmm.io/tech/selector-caching-jquery/) [↩](#2.1)

<b id="f2.2">4. </b>[ jQuery Detach](http://learn.jquery.com/performance/detach-elements-before-work-with-them/) [↩](#2.2)

<b id="f2.3">5. </b>[ Append outside the loop](http://learn.jquery.com/performance/append-outside-loop/) [↩](#2.3)

<b id="f3.1">6. </b>[ Performance Comparison of ID and other Selector](https://jsperf.com/book-selector-tests) [↩](#3.1)

<b id="f3.2">7. </b>[ Performance Comparison of class selectors](https://jsperf.com/jquery-selector-comparison-123) [↩](#3.2)

<b id="f3.3">8. </b>[ Find  vs Context](https://jsperf.com/jquery-find-vs-context-2/13) [↩](#3.3)

<b id="f4.1">9. </b>[ jQuery - Don't repeat yourself](http://learn.jquery.com/code-organization/dont-repeat-yourself/)  [↩](#4.1)

<b id="f4.2">10. </b>[ Client side jQuery Best practices](https://github.com/holidayextras/culture/blob/master/clientside-jquery-best-practices.md#bad-5)
