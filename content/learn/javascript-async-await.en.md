---
title: 'Modern Asynchronous JavaScript with Async and Await'
description: 'Discover the modern approach to asynchronous functions in JavaScript. JavaScript evolved in a very short time from callbacks to Promises, and since ES2017 asynchronous JavaScript is even simpler with the async/await syntax'
authors: flaviocopes, potch, MylesBorins, LaRuaNa, amiller-gh, ahmadawais
category: learn
---

## Introduction

JavaScript evolved in a very short time from callbacks to promises (ES2015), and since ES2017 asynchronous JavaScript is even simpler with the async/await syntax.

Async functions are a combination of promises and generators, and basically, they are a higher level abstraction over promises. Let me repeat: **async/await is built on promises**.

## Why were async/await introduced?

They reduce the boilerplate around promises, and the "don't break the chain" limitation of chaining promises.

When Promises were introduced in ES2015, they were meant to solve a problem with asynchronous code, and they did, but over the 2 years that separated ES2015 and ES2017, it was clear that _promises could not be the ultimate solution_.

Promises were introduced to solve the famous _callback hell_ problem, but they introduced complexity on their own, and syntax complexity.

They were good primitives around which a better syntax could be exposed to the developers, so when the time was right we got **async functions**.

They make the code look like it's synchronous, but it's asynchronous and non-blocking behind the scenes.

## How it works

An async function returns a promise, like in this example:

```js
const doSomethingAsync = () => {
  return new Promise(resolve => {
    setTimeout(() => resolve('I did something'), 3000);
  });
};
```

When you want to **call** this function you prepend `await`, and **the calling code will stop until the promise is resolved or rejected**. One caveat: the client function must be defined as `async`. Here's an example:

```js
const doSomething = async () => {
  console.log(await doSomethingAsync());
};
```

## A quick example

This is a simple example of async/await used to run a function asynchronously:

<iframe
  title="Modern Asynchronous JavaScript with Async and Await"
  src="https://stackblitz.com/edit/nodejs-dev-0007-01?index.js&zenmode=1&view=editor"
  alt="nodejs-dev-0007-01 on StackBlitz"
  style="height: 400px; width: 100%; border: 0;">
</iframe>

## Promise all the things

Prepending the `async` keyword to any function means that the function will return a promise.

Even if it's not doing so explicitly, it will internally make it return a promise.

This is why this code is valid:

```js
const aFunction = async () => {
  return 'test';
};

aFunction().then(alert); // This will alert 'test'
```

and it's the same as:

```js
const aFunction = () => {
  return Promise.resolve('test');
};

aFunction().then(alert); // This will alert 'test'
```

## The code is much simpler to read

As you can see in the example above, our code looks very simple. Compare it to code using plain promises, with chaining and callback functions.

And this is a very simple example, the major benefits will arise when the code is much more complex.

For example here's how you would get a JSON resource, and parse it, using promises:

```js
const getFirstUserData = () => {
  return fetch('/users.json') // get users list
    .then(response => response.json()) // parse JSON
    .then(users => users[0]) // pick first user
    .then(user => fetch(`/users/${user.name}`)) // get user data
    .then(userResponse => userResponse.json()); // parse JSON
};

getFirstUserData();
```

And here is the same functionality provided using await/async:

```js
const getFirstUserData = async () => {
  const response = await fetch('/users.json'); // get users list
  const users = await response.json(); // parse JSON
  const user = users[0]; // pick first user
  const userResponse = await fetch(`/users/${user.name}`); // get user data
  const userData = await userResponse.json(); // parse JSON
  return userData;
};

getFirstUserData();
```

## Multiple async functions in series

Async functions can be chained very easily, and the syntax is much more readable than with plain promises:

<iframe
  title="Multiple async functions in series"
  src="https://stackblitz.com/edit/nodejs-dev-0008-01?index.js&zenmode=1&view=editor"
  alt="nodejs-dev-0008-01 on StackBlitz"
  style="height: 400px; width: 100%; border: 0;">
</iframe>

## Easier debugging

Debugging promises is hard because the debugger will not step over asynchronous code.

Async/await makes this very easy because to the compiler it's just like synchronous code.
