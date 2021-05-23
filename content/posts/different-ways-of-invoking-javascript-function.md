---
layout: post
title: "Different ways of invoking JavaScript functions"
date: 2014-11-25
comments: true
tags: ["functions", "jsfoundations", "jsbasics"]
categories: ["functions", "jsfoundations", "jsbasics"]
---

In JavaScript the way a function is invoked has a significant impact on how the code within it executes, especially regarding `this` parameter.

If you have followed couple of my earlier posts I have demonstrated two ways of invoking functions already and they are
#### 1. Invoking functions as, surprise, functions
#### 2. Invoking functions as object methods

Now in addition to these two there are two more ways of invocations
#### 3. Invoking functions as constructors
#### 4. Invoking functions using `apply()` and `call()` methods

Before I dig into details of each type of function invocation, I must explain two important concepts i.e. two implicit parameters `arguments` and `this` that are passed to every invoked function.

### `arguments` *collection*
Whenever a function is invoked in any of the four ways mentioned above, it can also be supplied with list of arguments which in turn are assigned to the parameters in the same order as defined in function declaration.

When the number of arguments match number of parameters then first argument is assigned to the first parameter, second argument is assigned to the second parameter and so on.

When the number of arguments are less than number of parameters then the parameters that have no corresponding arguments are set to undefined.

When the number of arguments are more than number of parameters then these extra arguments are simply not assigned to any parameters. Important thing to note though is that there is a way to access these extra arguments via implicit parameter called `arguments`

This implicit `arguments` parameter is quite deceptive. As in it acts like an array but it isn't really an array. It has `length` property, its elements can be accessed using array notation and it can also be iterated just like an array can be. But it doesn't have all array methods available on it.

### `this` parameter

This notion of `this` implicit reference comes from object-oriented languages where methods are usually invoked in the context of an object and `this` generally points to this *function context* object. In JavaScript also if you invoke a function as an object's method then its *function context* is the object and it means `this` implicitly references to the object. However invoking function as object method is just one of the four ways of invoking a function.

Let's take a look at each of these four ways of invoking functions and what effect they have on the implicit `this` parameter they have in detail with sample codes

#### 1. Invoking functions as _functions_
A function can be invoked simply using the () operator on any expression that resolves to a function reference.

{{< highlight javascript >}}
function a() {
	console.log(this); // prints [object Window]
}
a();
{{< / highlight >}}

When function is invoked in this manner the function context is `Window` i.e. the global context. This is true even when a function is deeply nested within other functions.

{{< highlight javascript >}}
function a() {
    function b() {
       function c() {
          console.log(this); // prints [object Window]
       }
       c();
    }
    b();
}
a();
{{< / highlight >}}

In above code, for function `c` function context is still `Window`

#### 2. Invoking functions as object methods
As I described in [earlier post]({% post_url 2014-11-21-javascript-functions-travel-first-class %}), functions can be assigned to object properties and can be invoked using parentheses `()`.

Similar to object-oriented languages, if a function is invoked as an object's method then the object becomes the function context.

{{< highlight javascript >}}
var user = {'name' : 'Bon Jovi'};
user.sing = function() {
	console.log(this.name + ' is singing');
}
user.sing(); // prints Bon Jovi is singing
{{< / highlight >}}

#### 3. Invoking functions as constructors
Functions that can be invoked as constructors are just like any other functions. What makes a function a constructor function is the way it is invoked which is using `new` keyword.

When a function is invoked as a constructor function i.e. using `new` keyword,

1. A new empty Object is created and passed on to the function as `this` implicit parameter. `this` new Object becomes <em>function context</em> for the constructor function.

2. If nothing is returned from function, by default this newly created Object is returned from it.

{{< highlight javascript >}}
function Singer() {
	this.sing = function() {
		console.log(this + ' is singing');
        return this;
    };
}
var user = new Singer();
console.log(user.sing() === user); // prints true
{{< / highlight >}}

*Note:* Nothing is stopping from you to invoke a constructor function like a normal function i.e. without `new` keyword. In above example calling `singer();` will get *window* object passed to it as its function context and get a *sing* method attached on it. Not exactly how you would want a constructor function to work.

#### 4. Invoking function with `apply()` and `call()` methods
All the above ways of function invocation dictate what will be the _function context_ But what if you want to force what will be the _function context_ ? Well, function themselves provides you with the means to do so with `apply()` and `call()` methods. Wait a min though. Methods? methods of functions? Yes, you heard it right. Remember [functions are first class objects]({% post_url 2014-11-21-javascript-functions-travel-first-class %}) and what do objects have? Objects have methods, and data of course. Every function is an object. Specifically, each function is an object of [Function](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Function).

Ok, let's see how to use `apply()` and `call()` methods to explicitely set `this` then.

{{< highlight javascript >}}

var randomWeights = [10,20,30];

function addToBox() {
	var weight = 0;
	for (var i = 0; i < arguments.length; i++) {
		weight += arguments[i];
	}
	this.weight = weight;
}

var redBox = {};
var blueBox = {};

addToBox.apply(redBox, randomWeights);
addToBox.call(blueBox, 1,2,3,4,5);
console.log(redBox.weight); // prints '60'
console.log(blueBox.weight);// prints '15'

{{< / highlight >}}

Both, `apply()` and `call()` methods takes first argument as the object that can be used as _function context_ i.e. `this`. The difference between these two methods is how the rest of the arguments are supplied to the function. `apply()` takes an array of arguments whereas `call()` takes any number of arguments.