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

That's a thing there that I've been practicing lately, taking advantage of the function hosting in JS to move my function declarations right after the `return` statements. I'll show you an example in a moment but first let's see what hoisting is:

What is hosting?
--------------

If we have a look at the mozilla's dev docs (really good for JS/HTML reference) about [hoisting](https://developer.mozilla.org/en-US/docs/Glossary/Hoisting):

> In JavaScript, functions and variables are hoisted. Hoisting is JavaScript's behavior of moving declarations to the top of a scope (the global scope or the current function scope).

No matter were I declare my functions, they will be moved to the top of the scope of where they were declared:

{% highlight javascript %}
hello(); // logs "hello!"

function hello() {
  console.log("hello!");
}
{% endhighlight %}

Be aware that *only* the declarations are moved, not assignations.
Executing the next code will result in an error:

{% highlight javascript %}
aFunction(); // undefined is not a function
var aFunction = function() {
  console.log("hello!");
}
{% endhighlight %}

The code above is equivalent to this one:

{% highlight javascript %}
var aFunction;

aFunction();

aFunction = function() {
  console.log("hello!");
}

{% endhighlight %}

Now how can we use this to improve how our code looks like?

Moving functions declarations at the end
--------------------------------------

A well written code should express clearly what its doing.
When you write your modules you should be thinking about how the users are going to interact with it, no matter if the users are other modules in your application.

One way, I do believe, it helps is by moving your return statement as high as possible in your code, by doing this you're pushing the readers attention towards what your module is exposing.

Let's take the next code as an example of what you usually see in a JS codebase:

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

Now let's imagine the `doSomething` function spans across dozens of lines, is there any way you can say what's the output? you'll to rely solely on its name to figure it out unless you start scrolling down till you find the return statement.

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

By abusing of the hoisting we moved all the declarations right after the return ending with a more understandable code.
