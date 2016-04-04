Callbacks, Callback Hell, and the Async Library
===============================================

## Overview

In this lesson we will learn how to manage a common challenge that one encounters when architecting Node applications: namely, what do you do when you need a part of your program to run *now* and then another part of it to run *later*? This situation, known as "asychronous processing" (or just "async"), is a key Node programming skill. By the end of this lesson you'll be able to:

1. Explain asynchronous processes.
2. Explain how callbacks make handling the asynchronous flow of an application easier in Node.

## What is an asynchronous process?

So what are asynchronous processes? The term sounds intimidating. Above, we learned that we are dealing with async when we want to run part of our program now, and part of it later. But what does this really mean in concrete terms. Let's take a concrete real-life example, far from the world of web applications.

Let's say we want to write a program that makes a sandwhich, a peanbut butter sandwhich. What are the steps of making a sandwhich? Well, we all know that:

```
1) Prepare workspace.  
2) Gather you ingredients:   
      * White Bread
      * Peanut Butter
      * Jelly
3) Put to slices of bread on workspace.
4) Spread peanut butter on one slice.
5) Spread jam on the other slice.
6) Slap those slices together.
8) Boom! Peanut Butter Sandwich.
```

![](http://ezmiller.s3.amazonaws.com/public/flatiron-imgs/peanutbutter.gif)

So there we have an algorithm, which can be performed one-step-after-another, i.e., synchronously. Pretty straight-forward. Now let's complicate things.

Let's say that we want our algorithm to use freshly baked bread, and that we also want the algorithm to be able to the handle scenario in which there's no peanut butter or jelly available. 
Now we have a problem. Because suddenly step #1 above is going to take time. Worse, it may even take an indefinite amount of time because we don't know *when exactly* the visit to the grocery store will be completed or when the bread will be baked. Anything can happen really. The bread could burn. Our regular grocer, remarkably, could be out of peanut butter! 

Suddenly our nice algorithmic recipe above is no longer synchronous; it's **A**sychronous! We cannnot proceed with step #3 until all of the processes involved in step #2 have finished. Using our more general definition of async above, here we have a situation in which we want part of our program (i.e. the preparation of the work space and the intial gathering of ingredients) to run immediately, and then later, at some undetermined moment when we have everything, we want to complete the task (i.e. make the sandwich).

This, then, is an asynchronous procedure. The key is that **we don't know when exactly in the future "later" will be; it's just *sometime* later.**

## Callbacks to the Rescue

Now that we've accomplished our first objective -- learning how to explain what an asychronous process is -- let's examine the most basic and common method for handling asynchrony using a pattern known as a "callback." Since you've probably already used jQuery quite a bit, it is very likely that you've already written a few callback functions, perhaps without knowing it. If so, maybe something of this will feel familiar!

Before we jump into an example of a callback, let's imagine a more real-world async example. Instead of making a sandwich, let's think about fetching search results from a server and displaying them to a user. All we need to  do is the following:

```
1. Send the search terms to the server that will fetch our results.
3. Display the results.
```

This is simple indeed, but the fetching of results from the server -- just as the gathering of ingredients in our peanut butter sandwich example -- will take an indeterminate amount of time. So how do we handle this scenario?

The most basic way to handle this problem is to use a "callback", and a callback is just a function that is called ("back") once an asynchronous process has completed. You can think of it this way: if you call someone and ask them do something for you that might take some time, you might also ask them to give you a "call back" when they are done so that 1) you know they are done, and 2) they can give you any information that you might need so that you can move forward with your part of the task. That's really all a callback is, and when we write them in our code all we are doing is explicitly organizing this process of coordination as part of the logic of the flow of our application.

Enough theory. Here's how we would use callbacks inside a jQuery AJAX request to perform a search on Google's Books API for works related to peanut butter sandwiches:

```
var successCallback = function(data) {
  // Display results on the page.
};

var errorCallback = function() {
  alert('Oh no! Something went wrong while fetching results from the server.');
}

function fetchResults(searchTerm) {
  $.ajax({
    method: 'GET',
    url: 'http://www.googleapis.com/books/v1/volumes?q=' + searchTerm,
    success: successCallback,
    error: errorCallback
  });
}

fetchResults('peanut butter sandwich');
```

So let's look at this code. The first two var declarations create two callback functions: `successCallback` and `errorCallback`. Then we define a function `fetchResults` that contains a jQuery AJAX request. The jquery ajax method takes an object with some options set: the first two setup the request itself by specifying the url and the method. Then there are two additional parameters `success` and `error` and here is where we link in our callback functions. Now, once this request returns back either sucessfully or with an error, the appropriate callback function will be called.

The beauty of this is that the (asynchronous) process that occurs on the server, which actually finds and returns the results, can take as long as it needs. Obviously, we don't want it to take a long time since that would annoy the user who is no doubt impatiently waiting for the results, but our program's logic can now handle the indeterminate time-gap of the async process because the process that is fetching the results now knows how to "call back" the process that will display the results once they are in.

So this is what a callback is. Essentially, callbacks are functions that, as one expert has succinctly put it, "wrap or encapsulate the continuation of [a] program" at some unspecified point later.
