---
layout: post
title:  "Cross Window Messaging with HTML5 LocalStorage Events"
date:   2012-04-12 07:16:00
categories: html5 gwt localstorage
---

Before HTML5, there wasn't a reliable and straightforward way to have 2 or more browser windows talk to each other using purely client side code.

Use case:

You have your web application open in two or more browser windows, say A, B and C. You want A, B and C to be able to talk to each other without having to go through a server.

The way to achieve that, was having B and C being opened by A. Then B or C could invoke [window.opener](https://developer.mozilla.org/en/DOM/window.opener "window.opener") and sent a message to the parent window. However this communication is unidirectional and not very reliable.

HTML5 introduced a concept called "[postMessage](https://developer.mozilla.org/en/DOM/window.postMessage)" where you can send a message to a target window. But this feature falls short from the desired behavior, as you still need to have a reference to the target window which you want to communicate to. Adding you the burden of keeping track of window references.

HTML5 introduced the [LocalStorage](http://dev.w3.org/html5/webstorage/#the-localstorage-attribute) feature and a very nifty events feature along with it. In short words, any window can listen to localStorage changes.

So, A, B and C can be listening to localStorage changes, and if A wants to send a message, all it has to do is store the message into localStorage, B and C will get notified of the change along with the stored value. It's safe, reliable, bidirectional and there's no need to keep track of window references.&nbsp;

Take a look at the attached video referencing the jsffiddle at: [http://jsfiddle.net/carlosaguayo/xgGZk/](http://jsfiddle.net/carlosaguayo/xgGZk/)

{% youtube CEkVsT4H8yc %}

As you can see, any window can store a message in LocalStorage and all the other windows listening to it can receive the message.

It's a safe feature because you only listen to LocalStorage changes from the same origin.