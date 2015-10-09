---
title: "Using Hoisting to improve JS readability"
tags:
  - javascript
  - clean code
---

A few days ago I read this interesting tweet in my streamline.

<blockquote class="twitter-tweet" lang="es">
  <p lang="en" dir="ltr">i&#39;ve started practicing what i preach Re: inline funcs vs func decls, and also func decls at end (and hoisted) instead of at top of scope.</p>&mdash; getify (@getify) <a href="https://twitter.com/getify/status/651473332957638656">octubre 6, 2015</a></blockquote>
<script async src="//platform.twitter.com/widgets.js" charset="utf-8"></script>

There's a thing there that I've been practicing lately, taking advantage of the function hosting in JS to move my function declarations right after the `return` statements. I'll show an example in a moment but first let's clarify what hoisting is:

What is hosting?
--------------

If we have a look at the mozilla dev's doc (really good for JS/HTML reference) about [hoisting](https://developer.mozilla.org/en-US/docs/Glossary/Hoisting):

> In JavaScript, functions and variables are hoisted. Hoisting is JavaScript's behavior of moving declarations to the top of a scope (the global scope or the current function scope).

So no matter were I declare my function it will be moved to the top of the scope:

{% highlight javascript %}
hello(); // logs "hello!"

function hello() {
  console.log("hello!");
}
{% endhighlight %}

Be aware that *only* the declaration is moved so this will result in an error:

{% highlight javascript %}
aFunction(); // error
var aFunction = function() {
  console.log("hello!");
}
/*
Effectively the code above becomes

var aFunction;

aFunction();

aFunction = function() {
  console.log("hello!");
}
*/
{% endhighlight %}
