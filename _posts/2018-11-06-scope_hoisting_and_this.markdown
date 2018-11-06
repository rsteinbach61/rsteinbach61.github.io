---
layout: post
title:      "Scope Hoisting and `this`"
date:       2018-11-06 21:00:17 +0000
permalink:  scope_hoisting_and_this
---



- What is "hoisting" in JS?

According to W3Schools:
 “Hoisting is JavaScript’s default behavior of moving declarations to the top.”

This means that some variable and function declarations are processed at the beginning of your JavaScript file no matter where they are declared in that file. I say some because there is a difference in the way JS processes variables declared using var and those that use let or const, and some functions do not get hoisted. 
```
h = “hoist”;
console.log(h);  //outputs "hoist" to the console even though it isn’t declared yet
var h;

h = “hoist”;
console.log(h);  // Throws ReferenceError: h is not defined
let h;		
```

Variables using var are hoisted so they can be initialized and used before they are declared. Variables using let or const will throw an error if used before they are declared and initialized. So always declare and initialize variables at the top of their scope. Scope???? I’ll get to that in a moment. First let’s look at hoisting functions.

Functions are hoisted too and can be used before they are declared.

```
const c = “const”;	//could use let here too, scope is global
hoist();  			 //function called before declaration logs “const” to the console
function hoist(){		//hoist() declared here
    console.log(c);		//logs “const”
}
```
Functions declared as an expression cannot be hoisted.
```
let h = function hoist(){}	//Won’t be hoisted
```  




- What is "scope" in JS?

According to W3Schools:
 “Scope determines the accessibility (visibility) of these variables.”

In this code there are three different kinds of scope: global, function, and block.
```
g = "global";
let s = "scope";		//global scope
const c = "const";

function blockScope(){
  g = "GLOBAL";		//global scope, changes g to all caps
  
if (g === "GLOBAL"){
    let s = "SCOPE";		//block scope, s is only available inside of the if(){}
  }
  const c = "CONST"	//function scope, c is only available inside the function
  return s + " " + c + " " + g;
}
console.log(blockScope()	         //logs “scope CONST GLOBAL”

```

The output uses the lowercase global definition of s because the uppercase definition is scoped inside the if block, and uses the uppercase definition of c because it is scoped inside the function blockScope(). Variables declared with let and const can be function or block scoped. Variables declared with var are only function scoped, so if used in a block they are still available outside of the block.



- What is "context" in JS?  How/when does the keyword `this` receive its value?

Context is another concept close to scope. It is important to understand because it has direct impact on how the `this` keyword gets its value. The execution context is the location where JS is currently operating. It has: a `this` value, variables, objects, and functions available for JS to use. 

`this` gets its value from the object that invokes a function. 

When a JS file executes it starts in the Global context where `this` is set to the global object (Window if you’re in a browser). So in a simple function call in a browser, `this` will be set to Window because by default Window is the object that invokes the function and therefore sets the value of `this`.

```
function test(){
  console.log(this);  
} 
window.test();	     //logs Window{…}	
test();			                 //logs Window{…}	

```

By using .call or .apply you can pass an object to a function and set the `this` value to the object.

```
const obj = { a: 10, b: 20, c: 30};

function testThis(){
  console.log(this.a);
}

testThis.call(obj); 	//passes obj to the function and logs 10

```
By making a function a method of the object the `this` value of the function comes from the object.

```
function context(){
  console.log(this.num);
}
let o = {num: 25,func: context}
let o2 = {num: 50,func: context}

console.log(o.func())	           	//logs 25
console.log(o2.func())	      	//logs 50
```






- How is `this` different in an arrow function compared to a function declared with the `function` keyword?

Arrow functions have no `this` value instead they inherit this from the lexical context – that is the context that contains the arrow function.

```
const es5 = {
  id: "ES5",
  counter: function counter() {
    console.log(this.id);			//logs ES5
    setTimeout(function() {
      console.log(this.id);	         //undefined because there is no `this` value in this function. Use .bind(this) to 
    }, 1000);                                   //correct this code.
  }
};

const es6 = {
  id: "ES6",
  counter: function counter() {
    console.log(this.id);			//logs ES6
    setTimeout(() => {
      console.log(this.id);	             //logs ES6 because the function inherits `this` from the counter
    }, 1000);
  }
};
es5.counter();
es6.counter();
```

 
