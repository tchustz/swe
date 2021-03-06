# Bootcamp > Week 4 > Day 5

## Overview of the day

----

## Lesson 1 - Tests in the browser

## Learning Objectives

* Recall the purpose of system tests
* Set up cypress testing in your project

## Before we start

* You need your app in a state where you have some UI elements on the page
* Your app needs to run in a test mode

## Materials needed

* [Cypress](https://www.cypress.io/)

## Lesson

Your app will need to run in 3 different environments: Your laptop, the deployment server and when we are running tests. Before adding cypress system (or end-to-end) tests make sure your app can run in a test mode.

Then in your project folder run

```sh
npm install --save-dev cypress mocha chai
```
Mocha and chai are testing dependencies for cypress.

### What does cypress do?

Have you been testing your app in the browser to make sure it works? Cypress can automate this for you. Encoding critical user journeys is incredibly valuable. It is much quicker than us clicking round the site. These journeys are then encoded and repeatable, and continuous integration tools can be set up to run these tests before each deployment.

### Getting started

Writing cypress tests is similar to the tests we have been writing with jest. The first thing we will need to do is have our app running in test mode. Then lets write a first test. In the `/cypress/integration` folder create a test spec file for example `homepage.spec.js`. Then create a simple test like the one below.

```javascript
describe('/', () => {
    it('the home page loads 2 seed tasks', () => {
        cy.visit('http://localhost:3001/')
        cy.contains('Tasks')
        cy.get('ul')
          .find('li')
          .should($li => {
              expect($li).to.have.length(2)
          })
    })
})
```
Run the test by adding the script below to your `package.json` and triggering it with `npm run test:cypress`

```json
"test:cypress": "cypress open"
```

What is new here is the `cy` object this object is our API for driving the browser. You can see we start by visiting a web address (our app must be running). We assert that the page `contains` the text 'Tasks' (thats my `<h1>` title). Then I get an element (you can select elements using .css selectors), find the `<li>` children (my tasks), finally I make an assertion that there should be 2 tasks.

You can peek inside the example folder for the syntax to do all kinds of things you do in your browser. One obvious test to try next would be to add a task.

```javascript
it('adds a new task to the list', () => {
    cy.visit('http://localhost:3000/')
    cy.get('input').type('New test task')
    cy.get('button').click()
    cy.get('ul')
    .find('li')
    .should($li => {
        expect($li).to.have.length(3)
    })
    .get('ul').find('li').last().contains('New test task')
})
```
Above we visit the page, get the `<input>` element, 'type' "New test task" into the input, then target and "click" the Add button, finally we make the assertion that there is a new task in the list. Awesome.

## Assignment

* In your team decide on 2 critical user journeys
* Encode these 2 journeys

[attendance log](https://applied.whitehat.org.uk/mod/questionnaire/complete.php?id=6702)
[main](/swe)|[prev](/swe/bootcamp/wk4/day4.html)|[next](/swe/bootcamp/wk5/day1-5.html)