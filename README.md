Handling Asynchronous Processes with Callbacks
===============================================

## Overview

What do we do when we need a part of our program to run *now* and then another part of it to run *later*? This situation, known as "asychronous processing" (or just "async"), is a key Node programming skill. This is what we will focus on in this lesson. By the nd of it you will be able to:

1. Explain asynchronous processes.
2. Explain how callbacks make handling the asynchronous flow of an application easier in Node.

## What is an asynchronous process?

So what are asynchronous processes? The term sounds intimidating. Above, we learned that we are dealing with async when we want to run part of our program now, and part of it later. But what does this really mean in concrete terms. Let's take a real-life example, far from the world of web applications.

Let's say we want to write a program that makes a sandwich, a peanut butter sandwich. What are the steps? Well, we all know that:

```
1) Prepare workspace.  
2) Gather your ingredients:   
      * White Bread
      * Peanut Butter
      * Jelly
3) Put two slices of bread on workspace.
4) Spread peanut butter on one slice.
5) Spread jam on the other slice.
6) Slap those slices together.
8) Boom! Peanut Butter Sandwich.
```

![](https://curriculum-content.s3.amazonaws.com/node-js/peanutbutter.gif)

So there we have an algorithm, which can be performed one-step-after-another, i.e., synchronously. Pretty straight-forward. Now let's complicate things.

Let's say that we want our algorithm to use freshly baked bread, and that we also want the algorithm to be able to the handle the scenario in which there's no peanut butter or jelly available.
Now we have a problem. Because suddenly step #1 above is going to take time. Worse, it may even take an indefinite amount of time because we don't know *when exactly* the visit to the grocery store will be completed or when the bread will be baked. Anything can happen really. The bread could burn. Our regular grocer, remarkably, could be out of peanut butter!

Suddenly our nice algorithmic recipe above is no longer synchronous. It's **A**sychronous! We cannnot proceed with step #3 until all of the processes involved in step #2 have finished. Using our more general definition of async above, here we have a situation in which we want part of our program (i.e. the preparation of the work space and the intial gathering of ingredients) to run immediately; then later, at some undetermined moment when we have everything, we want to complete the task (i.e. make the sandwich).

This, then, is an asynchronous procedure. The key is that **we don't know when exactly in the future "later" will be; it's just *sometime* later.**

## Callbacks to the Rescue

Now that we've accomplished our first objective -- learning how to explain what an asychronous process is -- let's examine the most basic and common method for handling asynchrony using a pattern known as a "callback." Since you've probably already used jQuery quite a bit, it is very likely that you've already written a few callback functions, perhaps without knowing it. If so, maybe something of this will feel familiar!

Before we jump into an example of a callback, let's imagine a more real-world async example. Instead of making a sandwich, let's think about fetching search results from a server and displaying them to a user. All we need to do is the following:

```
1. Send the search terms to the server that will fetch our results.
2. Display the results.
```

This is simple indeed, but the fetching of results from the server -- just as the gathering of ingredients in our peanut butter sandwich example -- will take an indeterminate amount of time. So how do we handle this scenario?

The most basic way to handle this problem is to use a "callback", and a callback is just a function that is called ("back") once an asynchronous process has completed. You can think of it this way: if you call someone and ask them to do something for you that might take some time, you might also ask them to give you a "call back" when they are done. You'd do that so that 1) you know they are done, and 2) they can give you any information that you might need so that you can move forward with your part of the task. That really is all a callback is, and when we write them in our code all we are doing is explicitly organizing this process of coordination as part of the logic of the flow of our application.

Enough theory. Here's how we would use callbacks inside a jQuery AJAX request to perform a search on Google's Books API for works related to peanut butter sandwiches:

```
var successCallback = function(data) {
  // Display results on the page.
};

function fetchResults(searchTerm) {
  $.ajax({
    method: 'GET',
    url: 'http://www.googleapis.com/books/v1/volumes?q=' + searchTerm,
    success: successCallback,
  });
}

fetchResults('peanut butter sandwich');
```

So let's look at this code. The first var declaration creates a callback function: `successCallback`. Then we define a function `fetchResults` that contains a `jQuery.ajax()` request. The jquery ajax method takes an object with some options set: the first two set up the request itself by specifying the url and the method. Then there is an additional parameter `success`. Here is where we link in our callback functions.

Just to show you an alternate syntax for writing callbacks, we coould have achieved the same effect by using the higher order `jQuery.get()` method. As you may have guessed, this function performs an ajax GET request, so you don't have to set the request method manually. The other difference, however, is that the `$.get()` method takes a callback function as its second argument. You'll actually see this syntax much more frequently. Here's how that code would look:

```
function fetchResults(searchTerm) {
  var url = 'http://www.googleapis.com/books/v1/volumes?q=' + searchTerm;
  $.get(url, function(data) {
     // Display results on the page.
  });
}

fetchResults('peanut butter sandwich');
```

Regardless of style, both code snippets achieve the same result by assigning the callback: now, once the ajax request returns back either sucessfully or with an error, the appropriate callback function will be called. The beauty of this is that the (asynchronous) process that occurs on the server can take as long as it needs to. Obviously, we don't want it to take a long time since that would annoy the user who is no doubt impatiently waiting for the results, but our program can now handle the indeterminate time-gap of this common asynchronous process because the process that is fetching the results now knows how to "call back" the process that will display the results once they are ready.

So now you know how to explain callbacks. Essentially, callbacks are functions that, as one programner succinctly put it, "wrap or encapsulate the continuation of [our] program" after an asychronous process completes at some indeterminate point in the future.[^1]

## Resources

* Excellent longer analysis of async in JS: Kyle Simpson,*You Don't Know JS: Async & Performance*, Chs. [1](https://github.com/getify/You-Dont-Know-JS/blob/master/async%20&%20performance/ch1.md) and [2](https://github.com/getify/You-Dont-Know-JS/blob/master/async%20&%20performance/ch2.md)

[^1]: Kyle Simpson, Kyle Simpson, ["Chapter 2: Callbacks"](https://github.com/getify/You-Dont-Know-JS/blob/master/async%20&%20performance/ch1.md) in *You Don't Know JS: Async & Performance*.

<p class='util--hide'>View <a href='https://learn.co/lessons/node-js-callbacks-readme'>Callbacks and the Async Library</a> on Learn.co and start learning to code for free.</p>
