---
layout: post
title:      "Using the equal sign (=) in JavaScript"
date:       2019-07-31 18:07:34 +0000
permalink:  using_the_equal_sign_in_javascript
---




There are three versions of = in JavaScript:

=

==

=== 

The first, a single equal sign (=), is used to assign a value to a variable.

Let x = 10 or let y = “ten” 

console.log(x)  // logs the number 10
console.log(y)  // logs the string ten

The other two, double and triple equal signs, are used to compare values. They return true or false and allow you to check if variables or values are equal.

Let’s try them out using these assignments:

let x = 10
let y = “ten”
let z = “10”


Double equal (==) determines if two variables or values are equal and returns true if they are equal REGARDLESS OF TYPE.

x == 10 returns true.
x == z returns true.
x == y returns false. Since the value is different the comparison returns false.

Triple equal (===) returns true only if the value and type is the same.

x === 10 returns true. 

x === z returns false.  Since z equals “10” which is a string, x and z have different types so the comparison returns false.

X === y returns false.


