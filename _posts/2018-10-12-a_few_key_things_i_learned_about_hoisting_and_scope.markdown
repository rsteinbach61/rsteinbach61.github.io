---
layout: post
title:      "A few key things I learned about Hoisting and Scope"
date:       2018-10-12 22:09:51 +0000
permalink:  a_few_key_things_i_learned_about_hoisting_and_scope
---


Since hoisting and scope are vital to JavaScript I decided I had better make sure I understood both concepts. 

According to W3Schools:

 “Scope determines the accessibility (visibility) of these variables.” and
 “Hoisting is JavaScript's default behavior of moving declarations to the top.”

So variables declared with var are hoisted to the top of their scope, but what happens during this hoist is what really matters. A var variable when hoisted is initialized with a value of undefined. Let and const act as if they are not hoisted at all. This means that let and const can’t be used before they are declared, while var can be used before declaration.

```
h = “hoist”;
console.log(h);  //outputs "hoist" to the console even though it isn’t declared yet
var h;
```

Const and let produce a reference error if used before declaration, so it is best to always declare and initialize variables at the top of their scope.

Functions are hoisted too and can be used before they are declared.

```
const c = “const”;	//could use let here too, scope is global

hoist();			//function called before declaration logs “const” to the console

function hoist(){
    console.log(c);
}
```

Functions declared as an expression cannot be hoisted. 
```
let h = function hoist(){}	//Won’t be hoisted  
```


In this code there are three different kinds of scope: global, function, and block.

```
g = "global";
let s = "scope";		//global scope
const c = "const";

function blockScope(){
  g = "GLOBAL";		//global scope

  if (g === "GLOBAL"){
    let s = "SCOPE";		//block scope
  }

  const c = "CONST"		//function scope
  return s + " " + c + " " + g;
}
console.log(blockScope()	//logs “scope CONST GLOBAL”

```

The output uses the lowercase global definition of s because the uppercase definition is scoped inside the if block, and uses the uppercase definition of c because it is scoped inside the function blockScope(). Variables declared with let and const can be function or block scoped. Var variables are only function scoped, so if used in a block they are still available outside of the block.

  


