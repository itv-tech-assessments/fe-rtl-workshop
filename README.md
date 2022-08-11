# Introduction

This workshop will cover how to test React components using React Testing Library. We'll go through a series of tasks and highlight the best practices when using this tool for React testing.

### Before we begin

Within your breakout rooms, choose a member to be the driver and the rest should be participating navigators. 

Make sure the driver has all the following pre-requisites:
 
- Node.js v14.18
- Clone the following repo [fe-rtl-workshop-app](https://github.com/ITV/fe-rtl-workshop-app)
- Follow instructions on the `README` file to setup the app

## What is [React Testing Library](https://testing-library.com/)?

React Testing Library (RTL) is a set of **utilities for DOM manipulation** that focuses on testing React components.

![Quote from Kent C. Dodds](./assets/kent-c-dodds-quote.png)

These utilities encourage testing components the way users interact with them. This makes tests simpler by testing behaviour rather than implementation details.

### RTL common utilities

- `getBy*` and `getAllBy*` - Returns the matching node(s) for a query, and throws an error if no elements match.
- `queryBy*` and `queryAllBy*` - Returns the matching node(s) for a query, and return `null` (or empty array) if no elements match.
- `findBy*` and `findAllBy*` - Returns a `Promise` which resolves when the element(s) for a query is found. The `Promise` is rejected if no element(s) is found.
- `fireEvent` - Methods for firing DOM events (`click`, `focus`, etc.)
- `waitFor` and `waitForElementToBeRemoved` - Methods to wait for an element to appear or disappear in response to an event, user action, timeout, or `Promise`.

#### Next: [1. Basic Component](./1.basic-component/README.md)
