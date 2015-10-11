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

So no matter were I declare my function, it will be moved to the top of the scope:

{% highlight javascript %}
hello(); // logs "hello!"

function hello() {
  console.log("hello!");
}
{% endhighlight %}

Be aware that *only* the declaration is moved so this will result in an error:

{% highlight javascript %}
aFunction(); // undefined is not a function
var aFunction = function() {
  console.log("hello!");
}
/*
Effectively the code above is "seen" as:
*/
var aFunction;

aFunction();

aFunction = function() {
  console.log("hello!");
}

{% endhighlight %}

Now how can we use this to improve how our code looks like?

Moving function declarations at the end
--------------------------------------

How many times have you opened a JS module to understand what is going on? What's the first thing you're interested?
A well written module should expose some methods for others to use, isn't?

Usually you have to scroll all the way down till the return statement, and probably it's returning a function result, some object, etc so I have to scroll again all the way up to find what is returning exactly:

{% highlight javascript %}
function doSomething() {
  function returnOneObject() {
    ...
  }
  function returnAnotherObject() {
    ...
  }
  return {
    oneObject: returnOneObject,
    anotherObject: returnAnotherObject
  }
}
{% endhighlight %}

Now imagine the `doSomething` function spans across dozens of lines, is there anyway you can say what is returning? you might rely solely on its name to figure it out unless you start looking for the return statement.

This is how I would rewrite it:

{% highlight javascript %}
function doSomething {
  return {
    oneObject: returnOneObject,
    anotherObject: returnAnotherObject
  }
  function returnOneObject() {
    ...
  }
  function returnAnotherObject() {
    ...
  }
}
{% endhighlight %}

By abusing of the hoisting we moved all the declarations right after the return statement. Now the first thing you see is what it's expected from this function. No matter how complex `returnOneObject` or `returnAnotherObject` might be, with a good naming some other developer can picture what this function is doing and returning.
