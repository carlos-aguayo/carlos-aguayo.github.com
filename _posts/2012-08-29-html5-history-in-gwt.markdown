---
layout: post
title:  "HTML5 History in GWT"
date:   2012-08-29 19:35:00
categories: html5 gwt history
---

One of the best features in HTML5 is the [History API](http://diveintohtml5.info/history.html) which lets you update the browser url without forcing a page refresh.

GWT provides a built-in framework for browser history management. It comes up with the concept of "Activities" and "Places" which make a lot of sense and facilitate development. As Ray Ryan said, ["get browser history right, and get it right early"](http://www.youtube.com/watch?v=PDuhR18-EdM#t=1m26s).

A small shortcoming is that the default implementation is based in updating the fragment. Lucikly the GWT team made it easy to extend GWT and provide alternative implementations.

When looking for ways to make GWT work with the new HTML5 history, I stumbled upon this f[orum post](https://groups.google.com/d/msg/google-web-toolkit/xsf_GCnH36Q/iU8c80zsl5cJ) where a good fella called [Thomas Broyer](https://plus.google.com/113945685385052458154/posts)&nbsp;helped with the following skeleton implementation.

{% gist 1883821 %}

In here I provide a working example of GWT and HTML5 History:

**[http://gwthistoryhtml5.appspot.com/](http://gwthistoryhtml5.appspot.com/)**

And you can find the code here:

**[https://github.com/carlos-aguayo/html5gwthistory](https://github.com/carlos-aguayo/html5gwthistory)**

{% youtube Hw_BnmmM1RA %}

Notice how when you move across the tabs, search or links, the URL updates without forcing a page refresh.

The default implementation of Places in GWT uses a token/value pair separated by a colon, example:

[http://gwthistoryhtml5.appspot.com/mail:starred](http://gwthistoryhtml5.appspot.com/mail:starred)

So what this implementation does is to just replace the colon for a slash:

[http://gwthistoryhtml5.appspot.com/mail/starred](http://gwthistoryhtml5.appspot.com/mail/starred)

Firefox, Chrome and Safari support HTML5 History, however IE9 doesn't have support for it. So instead I provide a "hashbang" implementation, so the URL above becomes:

[http://gwthistoryhtml5.appspot.com/#!/mail/starred](http://gwthistoryhtml5.appspot.com/#!/mail/starred)

All starts in:

{% gist 3521926 %}

Using deferred binding, we provide two implementations, when IE, we use the "HashBangHistorian", and for any other browser, we can use the "Html5Historian". Notice that it's better to just hardcode the browser versions so that way we don't have a "[deferred binding explosion](http://code.google.com/p/google-web-toolkit/wiki/ConditionalProperties)".

They are designed using the Template Method design pattern. Both of them extend from the "CustomHistorian" which does the heavy-lifting:

[https://github.com/carlos-aguayo/html5gwthistory/blob/master/src/com/dreamskiale/client/history/CustomHistorian.java](https://github.com/carlos-aguayo/html5gwthistory/blob/master/src/com/dreamskiale/client/history/CustomHistorian.java  )

Both of them use JSNI to be able to access JavaScript directly. HTML5Historian then can directly invoke the HTML5 History API.

{% gist 3522213 %}

The implementation is quite simple and as you can see does not support deeper URL levels as it relies in the key/value pair of GWT places.