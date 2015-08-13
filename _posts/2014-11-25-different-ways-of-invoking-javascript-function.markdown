---
layout: post
title: "Different ways of invoking JavaScript functions"
date: 2014-11-25 00:01
comments: true
categories: functions, jsfoundations, jsbasics
---

In JavaScript the way a function is invoked has a significant impact on how the code within it executes, especially regarding <code>this</code> parameter.

If you have followed couple of my earlier posts I have demonstrated two ways of invoking functions already and they are
#### 1. Invoking functions as, surprise, functions
#### 2. Invoking functions as object methods

Now in addition to these two there are two more ways of invocations
#### 3. Invoking functions as constructors
#### 4. Invoking functions using <code>apply()</code> and <code>call()</code> methods

Before I dig into details of each type of function invocation, I must explain two important concepts i.e. two implicit parameters <code>arguments</code> and <code>this</code> that are passed to every invoked function.

### <code>arguments</code> *collection*
Whenever a function is invoked in any of the four ways mentioned above, it can also be supplied with list of arguments which in turn are assigned to the parameters in the same order as defined in function declaration.

When the number of arguments match number of parameters then first argument is assigned to the first parameter, second argument is assigned to the second parameter and so on.

When the number of arguments are less than number of parameters then the parameters that have no corresponding arguments are set to undefined.

When the number of arguments are more than number of parameters then these extra arguments are simply not assigned to any parameters. Important thing to note though is that there is a way to access these extra arguments via implicit parameter called <code>arguments</code>

This implicit <code>arguments</code> parameter is quite deceptive. As in it acts like an array but it isn't really an array. It has <code>length</code> property, its elements can be accessed using array notation and it can also be iterated just like an array can be. But it doesn't have all array methods available on it.

### <code>this</code> parameter

This notion of <code>this</code> implicit reference comes from object-oriented languages where methods are usually invoked in the context of an object and <code>this</code> generally points to this *function context* object. In JavaScript also if you invoke a function as an object's method then its *function context* is the object and it means <code>this</code> implicitly references to the object. However invoking function as object method is just one of the four ways of invoking a function.

Let's take a look at each of these four ways of invoking functions and what effect they have on the implicit <code>this</code> parameter they have in detail with sample codes

#### 1. Invoking functions as _functions_
A function can be invoked simply using the () operator on any expression that resolves to a function reference.

{% highlight javascript %}
function a() {
	console.log(this); // prints [object Window]
}
a();
{% endhighlight %}

When function is invoked in this manner the function context is <code>Window</code> i.e. the global context. This is true even when a function is deeply nested within other functions.

{% highlight javascript %}
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
{% endhighlight %}

In above code, for function <code>c</code> function context is still <code>Window</code>

#### 2. Invoking functions as object methods
As I described in [earlier post]({% post_url 2014-11-21-javascript-functions-travel-first-class %}), functions can be assigned to object properties and can be invoked using parentheses <code>()</code>.

Similar to object-oriented languages, if a function is invoked as an object's method then the object becomes the function context.

{% highlight javascript %}
var user = {'name' : 'Bon Jovi'};
user.sing = function() {
	console.log(this.name + ' is singing');
}
user.sing(); // prints Bon Jovi is singing
{% endhighlight %}

#### 3. Invoking functions as constructors
Functions that can be invoked as constructors are just like any other functions. What makes a function a constructor function is the way it is invoked which is using <code>new</code> keyword.

When a function is invoked as a constructor function i.e. using <code>new</code> keyword,

1. A new empty Object is created and passed on to the function as <code>this</code> implicit parameter. <code>this</code> new Object becomes <em>function context</em> for the constructor function.

2. If nothing is returned from function, by default this newly created Object is returned from it.

{% highlight javascript %}
function Singer() {
	this.sing = function() {
		console.log(this + ' is singing');
        return this;
    };
}
var user = new Singer();
console.log(user.sing() === user); // prints true
{% endhighlight %}

*Note:* Nothing is stopping from you to invoke a constructor function like a normal function i.e. without <code>new</code> keyword. In above example calling <code>singer();</code> will get *window* object passed to it as its function context and get a *sing* method attached on it. Not exactly how you would want a constructor function to work.

#### 4. Invoking function with <code>apply()</code> and <code>call()</code> methods
All the above ways of function invocation dictate what will be the _function context_ But what if you want to force what will be the _function context_ ? Well, function themselves provides you with the means to do so with <code>apply()</code> and <code>call()</code> methods. Wait a min though. Methods? methods of functions? Yes, you heard it right. Remember [functions are first class objects]({% post_url 2014-11-21-javascript-functions-travel-first-class %}) and what do objects have? Objects have methods, and data of course. Every function is an object. Specifically, each function is an object of [Function](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Function).

Ok, let's see how to use <code>apply()</code> and <code>call()</code> methods to explicitely set <code>this</code> then.

{% highlight javascript %}

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

{% endhighlight %}

Both, <code>apply()</code> and <code>call()</code> methods takes first argument as the object that can be used as _function context_ i.e. <code>this</code>. The difference between these two methods is how the rest of the arguments are supplied to the function. <code>apply()</code> takes an array of arguments whereas <code>call()</code> takes any number of arguments.
