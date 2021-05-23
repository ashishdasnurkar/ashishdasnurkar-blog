---
layout: post
title: "JavaScript scopes"
date: 2014-11-24
comments: true
tags: ["functions", "jsfoundations", "jsbasics"]
categories: ["functions", "jsfoundations", "jsbasics"]
---

I started my programming career with C,C++/VC++ and then Java. All are known as C family languages and they all share *block level scope* when it comes to variables and function declarations. In other words each block, typically represented by `{` and `}` braces, creates a new scope. For e.g.



{{< highlight c >}}
#include <studio.h>

int main() {
	int a = 1;
	printf("%d ", a); //1

	if(1) {
		int a = 2;
		printf("%d ", a); //2
	}

	printf("%d ", a); //1
}

{{< / highlight >}}

Scope of <code>int a</code> variable declared inside <code>if</code> block exists only till the end of <code>if</code> block.

However when I started working with JavaScript I tripped real hard understanding scopes in JavaScript. JavaScript's C family like syntax had me fooled. The thing is JavaScript scopes are fundamentally different as they are defined by *functions*, not by blocks. For eg.

{{< highlight javascript >}}
function a() {
	console.log(typeof b); // prints 'function'
	if(1) { 
		function b() {};
	}
	console.log(typeof b); // prints 'function'
}
a(); // prints 'undefined' as expected
{{< / highlight >}}

Here function <code>b</code> is accessible i.e. prints 'function' even after the <code>if</code> block ends. In fact, function <code>b</code> can also be forward referenced before the <code>if</code> block. This is known as *function hoisting*.

Here comes the tricky part though. Variables declarations in JavaScript are in scope from their point of declaration inside a function till the end of function, irrespective of block nesting. For e.g.

{{< highlight javascript >}}
function a() {
	console.log(typeof b); // prints 'undefined' because var b is not in scope yet
	if(1) { 
		var b = 1; // var b will be in scope till the end of function a
	}
	console.log(typeof b); // prints 'number'
}
a(); // prints 'undefined' as expected
{{< / highlight >}}

Notice that unlike <code>function b</code> in previous sample code, <code>var b</code> here can not be forward referenced.

