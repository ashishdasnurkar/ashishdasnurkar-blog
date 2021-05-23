---
layout: post
title: "Using webtask with ifttt to save reddit stories into mongodb"
date: "2016-02-02T13:43:40"
comments: true
tags: ["hacking", "webtask", "ifttt", "reddit", "personal"]
categories: ["hacking", "webtask", "ifttt", "reddit", "personal"]
---

While reddit may not be the frontpage of internet for me, I do visit it often to keep in touch with programming, technology and sports news. Reddit has almost become my default bookmarking system where I often "save" reddit stories and read or refer them later. As the number of saved stories have gone up it has become quite difficult to search specific stories when I need them. There is no way to categorise them by subreddit either. I often thought about writing a node script or a cron job to fetch saved stories myself and dump them in a database or even a plaintext file for easy full-text search.

While there are ton of reddit related recipes on [ifttt](http://ifttt.com/) but none of them fit specifically my purpose. Then I was told about [webtask.io](https://webtask.io/) and I immediately liked this idea of running a webtask, basically a piece of code that can be executed with a simple http call. Webtask combined with ifttt is a perfect combination as I get a ifttt trigger fired whenever I save a post on reddit and call a webtask action to do anything I want. I have complete control over the action part and not limited by the ifttt actions only.

So here is what I did briefly. I created a webtask that is invoked whenever I save a post on reddit. This webtask connects to mongolab mongodb instance and saves the reddit post meta data in "posts" collection. Later on I plan to write a node cli client to fetch/search these posts from terminal.

For now I will just document the webtask creation and ifttt configuration so just in case if you want to setup similar ifttt task you can follow along as well.

# create webtask
A webtask is a simple node module. A simplest webtask could be as simple as a hello.js file containing following code

{{< highlight javascript >}}
module.exports = function (cb) {
  cb(null, 'hello webtasks!');
}
{{< / highlight >}}

and you can create a webtask using webtask cli (install it fist via https://webtask.io/cli)

{{< highlight bash >}}
`wt create hello.js`
{{< / highlight >}}

Executing above command gives you a webtask url of sorts `https://webtask.it.auth0.com/api/run/<some_magic_string>/hello?webtask_no_cache=1`. To execute this webtask you simply have to call this url either via browser or via curl manually. Or with any http client library really. You can also host your webtask source code on a publically accessible url and create a webtask with that url.

For my purpose I created a [github repo](https://github.com/ashishdasnurkar/redditwebtask) and pushed my redditwebtask into it. Now remember that webtask can be executed via http call so you can also pass some data to your webtask via query string. This is exactly what I needed. The idea is to use ifttt trigger to call redditwebtask via url and pass reddit post metadata to it in querystring. Let's first take a look at redditwebtask implementation and see how it saves the metadata to mongolab mongodb instance.

{{< highlight javascript >}}
var MongoClient = require('mongodb').MongoClient;

function savePost(post, db, cb) {
	var col = db.collection('posts');

	col.insertOne(post, function(err, r) {
		if(err) return cb(err);
    	console.log('inserted ', post);
    	cb(null);
	});
};

module.exports = function (ctx, done) { 
	MongoClient.connect(ctx.data.MONGO_URL, function (err, db) {
	    if(err) return done(err);

	    var post = {};
	    post.title = ctx.data.title;
	    post.content = ctx.data.content;
	    post.postURL = ctx.data.postURL;
	    post.subreddit = ctx.data.subreddit;

	    savePost(post, db, function(err) {
	    	if(err) return done(err);
	    	db.close();
	    	done(null, 'Success');
	    })
	});
};
{{< / highlight >}}

As I said earlier webtask is a node module and behind the scenes it is executed inside a [webtask runtime](https://webtask.io/docs/how). This runtime provides generic and uniform execution environment which basically means access to lot of other popular node modules :)

Here I am "require"ing official mongodb node module to connect with mongodb instance.

The webtask module itself is a JavaScript function that takes two parameters; a `ctx` context object that provides access to any http querystring parameters passed to our webtask and a `done` callback function. There are [different forms](https://webtask.io/docs/model) of this JavaScript function but this is one that is perfect for my use case.

Rest of the code is pretty self-explanatory. I connect to mongodb instance via `MongoClinet.connect` call with passed in MONGO_URI and save the post metadata in "posts" collection using mongodb driver api `insertOne`.

Next step is to actaully create our redditwebtask using wt-cli `wt create` command. Remember to create mongolab mongodb database and database user first as you need these to form your MONGO_URI connect parameter.

![wt create command](/images/create_webtask_command.png)


Creating webtask by executing `wt create` command return a url. Later you will use this url to setup an action in ifttt (step 6 below)

# Create IFTTT trigger and action

Next, I added a recipe on ifttt.

Step 1. Choose reddit channel

![ifttt reddit channel](/images/choose_reddit_trigger_step1.png)

Step 2. Choose "New post saved by you" trigger.
This will take you to reddit login and authorize ifttt to certain permissions on your reddit account.

![Choose reddit trigger](/images/choose_post_save_trigger_step2.png)

Step 3. Create trigger

![create trigger](/images/create_trigger_step3.png)

Step 4. Next, choose Maker channel for the "that" action part in ifttt

![Choose maker channel](/images/choose_maker_channel_step4.png)

Step 5. Select the "Make web request" action

![Select make web request](/images/choose_make_web_request_step5.png)

Step 6. Configure ifttt to execute webtask by making a request to webtask url and pass "ingredients" i.e. reddit saved post metadata to the webtask

![configure webtask url](/images/maker_channel_settings_step6.png)

# Troubleshooting

IFTTT takes bit of time to trigger the webtask web request. You can try the "Check recipe" buttom om ifttt once yur recipe is created to force the trigger.

If "Check recipe" throws any error, check the webtask logs by running `wt logs` command. This command streams the webtask logs so you might need to click "Check recipe" while this command is running as it won't show past logs.

You could also test the webtask locally using the `test.js` in the repo (refer to readme in github repo to see how to use it).

