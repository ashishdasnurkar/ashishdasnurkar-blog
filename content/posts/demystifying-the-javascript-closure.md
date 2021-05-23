---
layout: post
title: "demystifying the javascript closure"
date: 2021-05-22
comments: true
tags: ["functions", "jsfoundations", "jsbasics"]
categories: ["functions", "jsfoundations", "jsbasics"]
draft: false
---

Imagine you are a mexican cook in the middle of an imaginary mexican desert. Your job is to cook whenever you are called upon. For you to be able to cook something, you need access to some usual stuff like ingredients, utensils and fire. Of course if you are in the middle of an imaginary desert you won't have anything on you or have access to anything. So let's also give you some super power, maybe flexible, stretchable, unbreakable arms that can reach to any <s>corners</s> part of the earth to retrieve whatever you need. This means you have access to anything on this planet and thus the planet is in your "scope". As long as you are alive and can be called upon, you will have access to everything in your _scope_ i.e the entire planet.

Now let's say I have come to know some magic spells from the e-learning course I took at Hogwarts online school of magic. If I were to turn you into a JavaScript function and stuff you into an html page via script tag, this is how you will look like


{{< highlight html >}}
<script>
var frying_pan = 'magic';
var oil = 'olive';
var tortilla = 'plain tortilla';

function mexican_cook() {
	console.log('add ' + oil + ' oil to ' + frying_pan
		+ ' frying pan and do something with ' + tortilla);
	// I'm not a mexican cook so not sure
	// what and how the hell you are going to cook something so
	// TODO: get a real mexican cook to fill in the details
}

mexican_cook();
</script>
{{< / highlight >}}

Assuming this is the only script tag and hence only piece of JavaScript code in this html page, everything inside the script tag is accessible to you i.e. <code>mexican_cook</code> function here. Hence the outout is as you would expect

Well this is all pretty understandable to you because everything declared in above code is in *global* scope hence is accessible to <code>mexican_cook</code> function. So if you go by the [MDN way of defining closure](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Closures) which I quote below, global scope is the *closure* in which you, the <code>mexican_cook</code> function lives in above example.

> Closures are functions that refer to independent (free) variables. In other words, the function defined in the closure 'remembers' the environment in which it was created.


But you probably are still confused, aren't you? Because everything is global. Everything is accessible. If closure is global then why not just call it global. Fair enough. Let me take the mexican cook analogy a bit more further to drive the point of _closure_ home, but before that I want you to remember what [JavaScript scopes]({% post_url 2014-11-22-javascript-scopes%}) are.

JavaScript scopes are function based. Every function defines its own scope and a function defined within another function has access to all the other functions and variables defined in outer function.

Ok now imagine you are re-born again as a mexican cook (you didn't think you would survive after I stuffed you in an html page now, did you?) but this time you were born inside a taco truck. You have access to everything inside the truck and can cook whenever called upon.

All right, time for some magic again. I'm going to stuff you inside an html page via a script tag again but this time along with your taco truck. This is how you would look like then


{{< highlight html >}}
<script>

function taco_truck() {
	var frying_pan = 'magic';
	var oil = 'olive';
	var tortilla = 'plain tortilla';

	function mexican_cook() {
		console.log('add ' + oil + ' oil to ' + frying_pan
			+ ' frying pan and do something with ' + tortilla);
		// I'm not a mexican cook so not sure
		// what and how the hell you are going to cook something so
		// TODO: get a real mexican cook to fill in the details
	}

	mexican_cook(); // calling you to cook something
}
// imagine I'm tapping on the outside of your truck here
// to get you to cook something for me
taco_truck();

</script>
{{< / highlight >}}

Okay, I hear you. You say this isn't all that different than the previous standing-in-the-middle-of-desert situation. Back then the _closure_ was _global_ scope and here the closure is <code>taco_truck</code> function.

Now, at this point I want to remind you another important concept about JavaScript functions. They are first class citizens of the JavaScript language. So just like _objects_ they can be assigned to variables and passed to other functions or returned as values from functions.

Considering this fact, what if the taco truck in our story has the phone number of you i.e. you the <code>mexican_cook</code> function written on the outside for everyone to see. So instead of tapping on the outside of taco truck, someone could call your number any time later. Even when the taco truck is not in sight. Equivalent of this situation would look something like below in actual JavaScript code


{{< highlight html >}}
<script>

function taco_truck() {
	var frying_pan = 'magic';
	var oil = 'olive';
	var tortilla = 'plain tortilla';

	function mexican_cook() {
		console.log('add ' + oil + ' oil to ' + frying_pan
			+ ' frying pan and do something with ' + tortilla);
		// I'm not a mexican cook so not sure
		// what and how the hell you are going to cook something so
		// TODO: get a real mexican cook to fill in the details
	}

	return mexican_cook; // taco truck is handing your number to anyone seeing it
}
// imagine someone just drove by your taco_truck and wrote down your number
var cook_number = taco_truck();

taco_truck = null; // taco_truck is out of sight

setTimeout(function() {
		cook_number(); // calling the mexican_cook after 3 looooooong seconds
	}, 3000);

</script>
{{< / highlight >}}