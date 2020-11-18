# Bootcamp > Week 1 > Day 4

## Overview of the day

# Lesson 1 - Airports Async

## Learning Objectives
*Explain the differences between the way synchronous and asynchronous functions work.
*Demonstrate the methods to test async code in Jest

## Pre-work or Assumed knowledge

## Materials
You need [this file](https://raw.githubusercontent.com/WhiteHatLearningProducts/airports/master/airportsData.json) saved in your airports project folder
## Notes

## Assignment
*In pairs can you explain to each other the differences between synchronous and asynchronous functions, and how you can tell the difference in your code.
*Use the three different ways of forming async functions in the Airport class
*Write async tests for each version in Jest


<hr/>

# Lesson 2 - The Event Loop

## Learning Objectives
*Demonstrate a deeper understanding of async code in javascript by naming key parts of the event loop
*Recall the meaning of 'stack overflow'

## Pre-work or Assumed knowledge

## Materials

## Notes

## Assignment
Make an animation or slide show that illustrates the event loop for the following piece of code. app.post is a route handler from an 'express' server it takes a string that is a path, and a route handler function. The route handler function is called when the server receives a POST request to the /users route. Start your stack with a call to the createUser function.

The route handlers in express are all on a timer, so if you don't call response.render or response.send within a time limit express will return a timeout error. Don't worry about this for now.

Your assignment code example uses promises, the behavior of the 'stack' and the 'pending callbacks' works the same way as the example above. Functions without a name are referred to as 'anonymous' functions. Be ready to present your slides or animation back to the group.

## Additional Reading
