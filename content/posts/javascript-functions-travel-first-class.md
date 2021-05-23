---
layout: post
title: "JavaScript functions travel first class"
date: 2014-11-21
comments: true
tags: ["functions", "jsfoundations", "jsbasics"]
categories: ["functions", "jsfoundations", "jsbasics"]
---


Functions in JavaScript are first class citizens of the language just like Objects.

In another words Functions enjoy all the same privileges (in fact more) as do the Objects in JavaScript.

Privileges such as 

### 1. Functions can be created via a literal.

{{< highlight js >}}
var obj = {a : 2, b: 2}; // object literal

var add = function(a, b) { // function literal
	return a + b;
};
{{< / highlight >}}

### 2. Functions can be assigned to variables, array entries and object properties.

{{< highlight js >}}

var add = function(a, b) { // function assigned to a variable
	return a + b;
};

var arr = [];
arr[0] = add; // function assigned to array entry at index 0

var obj = {};
obj.addMethod = add; // function assigned to Object obj as a addMethod property

{{< / highlight >}}

### 3. Functions can be passed as arguments to other functions.

{{< highlight js >}}

var values = [34, 56, 12, 11 , 5, 123];

var descendingComparator = function(value1, value2) {
	return value2 - value1;
}
values.sort(descendingComparator); // function descendingComparator passed as argument

console.log(values); // this should print [123, 56, 34, 12, 11, 5] 

{{< / highlight >}}

### 4. Functions can be returned as values from other functions

{{< highlight js >}}

function multiplier(x) {
	return function(y) { return x * y;};
}

var doubleIt = multiplier(2);
console.log(doubleIt(2)); // prints 4

var tripleIt = multiplier(3);
console.log(tripleIt(3)); // prints 9

{{< / highlight >}}

### 5. Functions can also have their own properties and these properties can be added or removed dynamically
{{< highlight javascript >}}

function myFunc() {}

myFunc.prop1 = 'prop1';

console.log(myFunc.prop1); // prints prop1

{{< /highlight >}}

In addition to all of above, Functions also have special superpower in that they can be _invoked_

