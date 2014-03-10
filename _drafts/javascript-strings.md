---
title: "A Brief Introduction to JavaScript"
layout: post
comments: true
excerpt:
categories: [javascript, node, tutorials]
---

## Overview

This brief introduction to JavaScript is part of my Node mini-primer. The goal is simply to introduce enough of the language to be able to follow my blog and hopefully stimulate an interest in Node as a platform for API development.

## JavaScript types

Without becoming pedantic in any sort of academically rigorous way, JavaScript is generally considered to be a dynamically typed language. Type information is associated with values at runtime, not with variables which can have values of any type assigned to them. Even after a variable has been assigned a value of one type, it can subsequently be reassigned a value of another type -- whether that's a good practice is another matter entirely, but there is no language-enforced restriction against doing so.

In JavaScript, there is a difference between a variable that has no value (`undefined`) and one that has been assigned a `null` value. Aside from undefined or null values, JavaScript supports strings, numbers, boolean, objects, and arrays.

### Primitive types

The primitive types are `string`, `number`, and `boolean`, along with two others that are considered the only instances of their special types, `null` and `undefined`.

### Object type

If a type is not a primitive type, then it is an `object`. You can think of an object as an unordered map of names to values of any type. These name-value paries are called `properties`.

When a variable is assigned a null value, the type of the variable becomes associated with the `object` type. In fact, `typeof null` returns `'object'`. This may or may not make sense to you. We mentioned earlier that `null` is a primitive type and this is because it cannot be inspected like an object -- it is not a collection of name-value pairs -- it is an intrinsic value whose only purpose is to signify a null object (which is altogether different from being undefined or having any specific primitive value, such as 0, false, or '').

### Array type

There is a specialization of an object that is an ordered map of numbers to values of any type. There is special syntax for handling arrays, and arrays exhibit a bit of special behavior distinguishing them from ordinary objects, but as you familarize yourself with JavaScript, be careful to make no assumptions about arrays being just like arrays you might already be familiar with from other programming languages. These number-value pairs are called `elements`.

### Example

This the complete list of types:

* undefined
* null
* string
* number
* boolean
* object
* array

This is illustration to show these types in use.

    var x;                     // typeof x => 'undefined'
    x = null;                  // typeof x => 'object'

    var name = 'Alice';        // typeof name => 'string'
    var age = 30;              // typeof age => 'number'
    var likesSecurity = true;  // typeof likesSecurity => 'boolean'
    var anArray = [];          // typeof anArray => 'array'
    var anObject = {};         // typeof anObject => 'object'

    // typeof skills => 'array'
    var skills = [ 'ciphers', 'public key encryption', 'hacking' ];

    // typeof person => 'object'
    var person = {
      name: 'Alice',
      age: 30,
      likesSecurity: true,
      skills: [ 'ciphers', 'public key encryption', 'hacking' ],
      bestFriend: { name: 'Bob', age: 30, likesSecurity: true },
      greet: function() { console.log('Hi, I'm Alice'); }
    };

JavaScript is dynamically typed. A variable can have a value of any type assigned to it, and can subsequently be assigned another value of another type.

    var x = 9;   // typeof x => 'number'
    x = 'hello'; // typeof x => 'string'

### Strings

You can use either matching single quotes or double quotes to delimit strings.

    var name1 = 'alice';
    var name2 = "bob";
    var greeting1 = "how's life";
    var greeting2 = 'how\'s life';
    var statement = 'he said, "JavaScript is cool";

Use whichever is more convenient to you. Single quotes seem to be slightly more favored (perhaps for the keyboards that don't require pressing shift). Don't forget when writing JSON that the specification dictates double quotes.

You can convert any value to a string value, as shown below:

    var x = 5;
    var s1 = String(x);
    var s2 = x.toString();

    x = null;
    s1 = String(x);    // 'null'
    s2 = x.toString(); // raises an error

Strings can be concatenated.

    var greeting = 'Hello, ' + 'world';

When concatenating, if the first value is a string, all other values will be converted to strings.

    var announcement1 = 'liftoff in' + 3 + 2 + 1;  // 'liftoff in 321'
    var announcement2 = 3 + 2 + 1 + ' liftoff!';   // '6 liftoff!'

#### A few useful string functions

##### split

Splits a string according to the specified delimiter and returns an array of string elements.

    var items = 'item1,item2,item3'.split(',');
    => [ 'item1', 'item2', 'item3' ]



### Numbers


 


