---
layout: post
title: "A look inside of the jQuery library"
date: 2016-03-29 21:44:45 -0400
comments: true
categories: ["Flatiron&nbsp;School","jQuery"]
---

After working with jQuery a bit, I appreciate how streamlined it makes traversing and manipulating the DOM. In this post, I want to understand some of the bigger picture domain components that make it possible. We will take a look at the DOM, NodeLists, and how to implement the jQuery library in your code.

# A few basics

> jQuery is a fast, small, and feature-rich JavaScript library. It makes things like HTML document traversal and manipulation, event handling, animation, and Ajax much simpler with an easy-to-use API that works across a multitude of browsers.

* $() is the equivalent of jQuery() - it's just a function
* jQuery functions usually take one argument - either a selector or something on the document
* jQuery is built on top of the DOM domain object model
* placing $() around an item turns it into a jQuery object


## Overview of the DOM
The DOM (or Document Object Model) is a programming interface for HTML and XML documents. It provides a structured representation of the document and defines a way that the structure can be accessed so the document structure, style and content can be manipulated. The DOM provides a representation of the document as a structured group of nodes and objects that have properties and methods.

{% img center http://www.w3schools.com/js/pic_htmltree.gif 850 %}

Essentially, it connects web pages to scripts or programming languages. When a webpage loads, the browser creates a Document Object Model of the page. The tree is composed of objects!

Side note: The jQuery function $ and many of the jQuery methods like click and animate return a jQuery object or NodeList, which is part object and part array. A NodeList is a collection of DOM elements, which has array-like tendencies. It is missing a lot of JavaScript functionality though, such as true Array iteration and Prototypal methods.



## Using jQuery selectors and functions

To add the jQuery library to your application, include a script tag in your HTML.

Option 1: Download a jQuery file and store in your application.
```HTML
<head>
<script src="jquery-1.12.0.min.js"></script>
</head>
```
Option 2: Link to a file listed on a Content Delivery Network.
```HTML
<head>
<script src="https://ajax.googleapis.com/ajax/libs/jquery/1.12.0/jquery.min.js"></script>
</head>
```

The basic syntax is: ```$(selector).action()```

* A $ sign to define and access the jQuery functionality
* A (selector) to find HTML elements
* A jQuery action() function to be performed on selected element(s)

The funtion below accepts a string containing a CSS selector, and will find DOM elements matching this selector. The second optional argument can be passed to set the context of what this should be within the inner function's scope.

```Javascript
$( selector [, context ] )
```

There are a ton of jQuery function categories. We've worked with the event functions like .on(), .toggle() and .change(). jQuery event functions trigger updates to the DOM based on user interaction.

```Javascript
.hover( handlerIn, handlerOut )
```
The above funcion binds two handlers to the matched elements, to be executed when the mouse pointer enters and leaves the elements.

Below is shorthand for the jQuery.ready function, which will run the inner function / callback after the page loads.
```Javascript
$(function() {
  // Document is ready
});
```

Since each jQuery function returns a modified jQuery object, you can execute a method chain. ```$("div").animate(…).append(…);```, the animation happens on all of the ```<div>``` elements in the jQuery object, and they are all passed to the next function in the chain as part of the jQuery object. Although this is tempting, chaining many functions like can cause issues and be challenging to debug. 

Now that we've talked a big picture, here's a shortlist of nifty selectors you can use:

* $(this) : Selects the current HTML element
* $("p.hello") : Selects all ```<p>``` elements with ```class="hello"```
* $("p:first") : Selects the first ```<p>``` element
* $("ul li:first") : Selects the first ```<li>``` element of the first ```<ul>```
* $("ul li:first-child") : Selects the first ```<li>``` element of every ```<ul>``` 
* $("[href]") : Selects all elements with an href attribute
* $("a[name='whatever']") : Selects all ```<a>``` elements with a name attribute equal to "whatever"
* $("a[name!='whatever']") : Selects all ```<a>``` elements with a name attribute NOT equal to "whatever"
* $(":button")  Selects all ```<button>``` elements and ```<input>``` elements of type="button"