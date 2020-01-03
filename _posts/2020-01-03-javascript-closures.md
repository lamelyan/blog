---
title: Javascript Closures
description: 
tags: [javascript]
---

Out of all the blog posts, [the one by Mozilla](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Closures) is probably the best explanation of closures. 


A prerequisite to understanding closure is to understand lexical scoping. 

>lexical scoping uses the location where a variable is declared within the source code to determine where that variable is available.


I especially like the section about the practicality of closures. Since I primarily use Typescript, I have the convenience of OOP syntactical sugar. But it is good to be able to understand how this is achieved. 


>Closures are useful because they let you associate some data (the lexical environment) with a function that operates on that data. This has obvious parallels to object-oriented programming, where objects allow us to associate some data (the object's properties) with one or more methods.


Check out using closures in the loop.  It's a good example of why the let keyword is useful. 

Also check out the following:

- [Model Pattern](https://coryrylan.com/blog/javascript-module-pattern-basics) - one of the most common design patterns used in JavaScript which creates encapsulation of our code.

- [Object Model](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Details_of_the_Object_Model) - understanding the difference between Class-based vs. prototype-based languages.
