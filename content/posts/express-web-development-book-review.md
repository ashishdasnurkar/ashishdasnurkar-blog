---
author: "Ashish Dasnurkar"
title: "Express Web Development: Book Review"
date: 2013-09-11
draft: false
tags: ["NodeJS", "ExpressJS", "JavaScript", "Book Review"]
categories: ["NodeJS", "ExpressJS", "JavaScript", "Book Review"]
---

[Express Web Development]() by [Hage Yaapa](http://www.hacksparrow.com/) is an in-depth introduction to web development using Express JS web framework. This book is aimed at anyone who is familiar with JavaScript and wants to leverage node.js platform for web development. Usually primary motivation for using NodeJS platform is to leverage same language i.e. JS at the front-end as well as back-end and also the non-blocking/fast nature of node.js platform. 

Sometime early last year I dabbled in [node.js](http://nodejs.org/) platform  however it was still evolving and I found it hard to develop apps with as there was no straight forward way for managing routes and file-uploads. I did come across Express back then but the documentation was lacking. I kept hearing about ExpressJS in some [other](http://addyosmani.github.io/backbone-fundamentals/) [books](http://shop.oreilly.com/product/0636920024231.do) but it was almost always a pretty introductory fare.In its current state ExpressJS documentation has improved a lot but a book that is neatly crafted, error free, easy to understand and follow is always most welcome. Express Web Development by Hage Yaapa is exactly this kind of book.

I liked the fact that Hage starts the book by explaining the concepts used in Express framework. He quickly brought me upto the speed on NodeJS and got me into developing with Express at the same time making sure I understood all the fundamental concepts like node modules and middlewares.

Next couple of chapters Hage introduces important ExpressJS important topics like routing, sending various responses back to client, handling form inputs including file uploads, managing application data with sessions and cookies. Everything essential for web application development. Recurring theme is that ExpressJS minimalistic approach is stressed and all various approaches are discussed for handling certain situations without bias to any specific approach. 

In addition to that specific chapters are dedicated to introduce Jade templating language for rendering response and Stylus CSS Preprocessor for managing application style. I won't debate the choice of these two as the point is not to show you the best available tool to do the job but to introduce you the basics of using each and quickly get your web application development with express off the ground. 

Last but not the least, and this is what lot of framework books miss out on including, is a whole chapter dedicated to tips on managing your Express apps in production. I stress this because it is important. Getting your webapp off the ground is the easiest part but once you have to put it out there then you struggle on finding good resources on actually preparing it for production. Hage introduces you how to simulate production environment and benchmark your webapps, creating app cluster for fail-over and high-availability and finally how to handle few critical events & ensuring uptime of your application.

I'm not a big fan of a book starting with a simple sample code and keeps evolving that same sample into something bigger as the book progress. This makes it harder for me to connect all the dots and ties me down to follow the book front to back. I'm glad this book did not follow this approach. Each concept is explained with its own self-contained sample code without having to backtrack to previous chapters for understanding them. And additionally you can even skip ahead and read the topics that you are interested in.

The only mild gripe I have is that book doesn't explain any strategy or a module for implementing authorization (introduction to [passport.js](http://passportjs.org/) would have been useful) and using any node ORM to interface with databases. If you have any previous experience with web development and coupled with Express middleware know-how you learn in this book you should be able to figure these things out on your own though. 

In conclusion, this book is extremely well written, easy to follow with enough latest and greatest in-depth information about ExpressJS framework at the moment. I highly recommend it. 

Disclaimer: I was approached by Packtpub to review this book and was provided with a free copy.