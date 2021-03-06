# Bootcamp > Week 1 > Day 4

## Overview of the day

<hr/>

## Lesson 1 - Airports Async

## Learning Objectives

* Explain the differences between the way synchronous and asynchronous functions work.
* Demonstrate the methods to test async code in Jest

## Before we start

* You need to have an Airport class
* You should be familiar with Object.keys as a way to access the keys of an object
* You should have iterated over an object in javascript before
* You should recognise deconstructing assignment

## Materials needed

* You need [this file](https://raw.githubusercontent.com/WhiteHatLearningProducts/airports/master/airportsData.json) saved in your airports project folder

## Lesson

So far our code executes synchronously. That means the code in the line above has been evaluated and any values are available for us to use on our current line. Async functions, they do not return straight away. For example if we want to read something from disc, that is an async function. It will not return immediately.

There are 3 ways to write and get values from async functions and in this session we are going to look at each of them. They are:

1. Use a callback function
1. Use Promises
1. Use 'async await'

Above is a link to a file with 28,000 airports in it. The file is in JSON so we can read it into our javascript programme and use that data to augment our Airport instances.

To start with lets write a test like this.

```javascript
test('airports have a city', () => {
    const CDG = new Airport('CDG')
    CDG.getInfo((err, info) => {
        console.log(info)
        expect(err).toBeNull()
        expect(info.country).toEqual('FR')
    })
})
```
Here is the way we are going to start doing this. In our test you can see I'm using a callback function. In Node.js callbacks follow this signature and `err` followed by your async value being returned. If there are no errors the `err` object is `null`. Run your tests.

Lets turn to our `Airport` class and write the `getInfo` function (that will take a callback). You will have to require the fs or 'file system' module from Node.js.

```javascript
const fs = require('fs')
// add this function to your Airport class definition
getInfo(callback) {
    fs.readFile('./airports.json', (err, data) => {
        callback(err, JSON.parse(String(data)))
    })
}
```
This is async code. We read the file from disk. The file contents comes out as a Buffer - you can console.log it to have a look at it. We need to turn the Buffer into a String, then that string we turn into a javascript object using JSON.parse. Finally we call the callback with an error if there is one or our file content nicely parsed into JSON.

But what? in our test we should see it logged out. But we don't. Why do we not see the contents of the file?

The test is called synchronously, it does not wait for the result of calling `CDG.getInfo`. To test an async function in jest pass in a "done" function and then call it when you are done.
```javascript
test('airports have a city', (done) => {
    const CDG = new Airport('CDG')
    CDG.getInfo((err, info) => {
        console.log(info)
        expect(err).toBeNull()
        expect(info.country).toEqual('FR')
        done()
    })
})
```
Can you see how that is working. You should now see your logging. Look at one of the entries in the airport data. We want to filter out an airport using the "iata" code. Can your `getInfo` function filter out the right airport and return just that data point?

### Promises

Another way to write and organise async code is using Promises. Lets refactor our getInfo function to <u>return</u> a promise.
```javascript
getInfo() {
    return new Promise((resolve, reject) => {
        fs.readFile('./airports.json', (err, data) => {
            if (err) return reject(err)
            
            const airports = JSON.parse(String(data))
            const [airport] = Object.keys(airports)
                .filter(airportCode => airports[airportCode].iata === this.name)
                .map(airportCode => airports[airportCode])
            
            resolve(airport)
        })
    })
}
```
Can you see the `new` keyword? What does that tell you about a Promise? What do you initialise a Promise with? Our callback style structure that we use with the `fs` module is wrapped in a promise. Now when resolve is finally called it will trigger the `.then` part of a promise object.

So to use our code now it will look different in our test:

```javascript
test('airports have a city', () => {
    const CDG = new Airport('CDG')
    return CDG.getInfo()
        .then(info => {
            expect(info.city).toEqual('Paris')
        })
        .catch(err => {
            expect(err).toBeNull()
        })
})
```
Notice now we don't need the `done` callback in the test, this is because we are returning a promise from our test, and Jest will figure this is an async test and will wait for the promise to resolve or reject.

The promise object is "thenable" you can chain a series of promises together using 'then' like this:

```javascript
return doSomeThing()
    .then(thing => {
        return theNextPromise(thing)
    })
    .then(next => {
        return anotherPromise(next)
    })
    .catch(err => {
        console.error('this catch block will catch any reject(err) in the chain.')
    })
```
Take note you must return a promise from the `then` block if you want to keep chaining. This avoids the pattern of deeply nesting callbacks, which some people find hard to read.

### Async await

Finally we can use the `async` and `await` keywords to make our Asynchronous code read more synchronously. Lets update our test:
```javascript
test('can get information like the city from an airport instance', async () => {
    const CDG = new Airport('CDG')
    const airport = await CDG.getInfo()
    expect(airport.city).toEqual('Paris')
})
```
Notice how we use the 2 keywords. First of all we need to declare an `async` function. We use the `async` keyword before our function definition. Then <u>inside</u> the async function we can use the `await` keyword to pause, and wait for our async value to resolve. That means we don't need the `done` callback, we don't need to use a promise with `then`, we can just write it nice and simple, line by line. Jest knows that this is an async test because we used the `async` keyword before the function definition.

Can you refactor your `getInfo` function to use async await?

It's a bit tricky because `fs.readFile` takes a callback. It's not really designed to work with async await. However from Node.js 11.0 onwards you can require a version of the `fs` functions that are wrapped in a promise object. Add this to the top of your Airport class:

```javascript
const { readFile } = require('fs/promises')
```
Now you can use the `readFile` function with the `await` keyword because this `readFile` function is wrapped in a promise.

That is a lot to get your head around! Async functions are a key characteristic of javascript. Objects, functions and async are the building blocks of the language. Spending time now learning how to work with them will enable you to start writing more complex code more quickly.

## Assignment

* In pairs can you explain to each other the differences between synchronous and asynchronous functions, and how you can tell the difference in your code.
* Use the three different ways of forming async functions in the Airport class
* write async tests for each version in Jest

----

## Lesson 2 - The Event Loop

## Learning Objectives

* Demonstrate a deeper understanding of async code in javascript by naming key parts of the event loop 
* Recall the meaning of 'stack overflow'

## Before we start

* You should have written an async function in the 3 different styles: callbacks, promises and async await

## Materials needed

* [reference video](https://youtu.be/8aGhZQkoFbQ)

## Lesson

Javascript is a single threaded runtime. Single threaded means one thing at a time. In reality that means one stack.

### What is the stack?

The stack is a data structure in the runtime of javascript that functions to organise the running or 'execution' of your code.

![an animation of a stack](https://miro.medium.com/max/1280/0*SESFJYWU5a-3XM9m.gif)

Last on first off. Imagine a stick over which you can place hoops. To get to the bottom hoop, all the other hoops have to come off first.

![stack of hoops](https://ws-na.amazon-adsystem.com/widgets/q?_encoding=UTF8&ASIN=B000U02LXY&Format=_SL250_&ID=AsinImage&MarketPlace=US&ServiceVersion=20070822&WS=1&tag=toy-ideas-20&language=en_US).

When your code is executed, javascript will run it in 2 passes. The first pass optimises your code for execution. The second pass actually runs your code, and it is in this second pass that javascript uses the stack.

### How does javascript use the stack?

Lets take the following code example:

```javascript
function multiply(a, b) {
    return a * b
}
function square(n) {
    return multiply(n, n)
}
function printSquare(n) {
    const result = square(n)
    console.log(result)
}

printSquare(4)
```
Read the code above. First of all there are 3 function definitions, then one of those functions is called. When `printSquare` is called it is put onto the stack. `printSquare` is evaluated and calls `square` which is added to the stack, `square` calls `multiply` which is added to the stack. `multiply` does not call any other function so it returns the value 16. the `return` keyword means that function pops off the stack, now inside `square` that function is evaluated to 16, and returns so `square` is then popped off the stack. Now back in `printSquare` the called to `square` is evaluated and assigned in memory to the variable `result`. Next line console.log is called with 16 and the function implicitly returns (without a value) as there is nothing more to execute. See below:

![call stack](https://user-images.githubusercontent.com/4499581/93218919-af697080-f762-11ea-8a8b-f2ab1b39b4fb.gif)

### What is a stack trace?

The stack is very help to know about. When your code errors, you often get a 'stack trace' as part of the error message. Being able to read the 'stack trace' can help us follow the executing code, and that can lead us to our piece of code that is causing the error.

Try running the code below in your browser.
```javascript
function multiply(a, b) {
    throw new Error(`can't multiply ${a} and ${b}`)
}
function square(n) {
    return multiply(n, n)
}
function printSquare(n) {
    const result = square(n)
    console.log(result)
}

printSquare(4)
```
This is the error. Read the stack trace from the bottom up.
![stack trace](https://user-images.githubusercontent.com/4499581/93219628-78e02580-f763-11ea-9948-81d558dbf65d.png)
What do you think the numbers like (<anonymous:5:12>) refer to?

### Stack overflow

```javascript
function hello() {
    hello()
}
hello()
```
This will break.
![stack overflow error message](https://user-images.githubusercontent.com/4499581/93220411-65818a00-f764-11ea-9a64-b5a92881ecaa.png)
Can you explain what is going on here? What other code might cause a max call stack size exceeded (stack overflow)?

### Async and the task que

In addition to the stack lets imagine another data structure a 'pending callback' que. We have seen that async functions get called, but don't return their values straight away. So you can imagine those async functions on the stack get put on, then popped off. We saw this in the example of the async Jest test before we used the `done` callback. However these async functions are recognized by Node.js so it places the callback function into another stack structure called the 'pending callback phase' and then polls (regularly checks) the computer for the completion of the reading from disc operation.

When the 'poll phase' receives an event indicating that the content of the file has been read into memory, it moves the pending callback back onto the stack the next time that the stack is emptied. It waits for the stack to be empty, because otherwise it would randomly interrupt the execution of whatever sequence of function calls were queued up on the stack.

Below is an example.

![async call stack](https://user-images.githubusercontent.com/4499581/93320644-3a04ab00-f809-11ea-9ab9-4770ec86b177.gif)

## Assignment

Make an animation or slide show that illustrates the event loop for the following piece of code. `app.post` is a route handler from an 'express' server it takes a string that is a path, and a route handler function. The route handler function is called when the server receives a POST request to the `/users` route. Start your stack with a call to the `createUser` function.

The route handlers in express are all on a timer, so if you don't call `response.render` or `response.send` within a time limit express will return a timeout error. Don't worry about this for now.

Your assignment code example uses promises, the behavior of the 'stack' and the 'pending callbacks' works the same way as the example above. Functions without a name are referred to as 'anonymous' functions. Be ready to present your slides or animation back to the group.

```javascript
app.post('/users', function createUser(request, response) {
    User.findOrCreate({ where: request.body })
        .then(function (user) {
            user.getContacts()
                .then(contacts => {
                    request.session.userId = user.id
                    response.render('profile', {user, contacts})
                })
        })
    logging(`/users route called with ${request.body}`)
})
```


[attendance log](https://platform.whitehat.org.uk/apprentice/attendance-log/156)

[main](/swe)|[prev](/swe/bootcamp/wk1/day3.html)|[next](/swe/bootcamp/wk1/day5.html)

    