# Introduction

This guide is to help [JR Academy](https://jiangren.com.au)'s students to better understand commercial javascript projects.

The JavaScript ecosystem evolves at incredible speed: staying current can feel
overwhelming. So, instead of you having to stay on top of every new tool,
feature and technique to hit the headlines, this project aims to understand javascript tools, library and terminology.

You get to start your app with our community's current
ideas on what represents optimal developer experience, best practice, most
efficient tooling and cleanest project structure.

## Table of Contents

  - [Networking](networking.md)
  - [Security](security.md)
  - [**CLI Commands**](commands.md)
  - [Tool Configuration](files.md)
  - [Server Configurations](general/server-configs.md)
  - [Database: SQL vs NoSQL](database.md)
  - [Deployment](deployment.md) _(currently Heroku and AWS S3 specific)_
  - [Debugging](debugging.md)
  - [FAQ](faq.md)
  - [Gotchas](general/gotchas.md)
  - [Extracting components](components.md)


Opening an issue is the fastest way to draw the attention of the team, but please make it a point to read the docs and contribution instructions before you do. The issues section is specifically used for pointing out defects and suggesting enhancements. If you have a question about one of the tools please refer to StackOverflow instead.

## Tech Stack

A curated list of packages that you should be at least familiar with before starting your awesome project/ code test/ legacy project. However, the best way to see a complete list of the dependencies is to check **package.json**. In package.json you could check out scripts for [**CLI Commands**](commands.md)

### Redux

Redux is going to play a huge role in your application. If you're new to Redux, we'd strongly suggest you to complete this checklist and then come back:

- [ ] Understand the motivation behind Redux
- [ ] Understand the three principles of Redux
- [ ] Implement Redux in a small React app of yours

The Redux `store` is the heart of your application. 

The store is created with the `createStore()` factory, which accepts three parameters.

1.  **Root reducer:** A master reducer combining all your reducers.
2.  **Initial state:** The initial state of your app as determined by your reducers.
3.  **Middleware/enhancers:** Middlewares are third party libraries which intercept each redux action dispatched to the redux store and then... do stuff. For example, if you install the [`redux-logger`](https://github.com/evgenyrodionov/redux-logger) middleware, it will listen to all the actions being dispatched to the store and print previous and next state in the browser console. It's helpful to track what happens in your app.

In our application we are using two such middleware.

1.  **Router middleware:** Keeps your routes in sync with the redux `store`.
2.  **Redux saga:** Used for managing _side-effects_ such as dispatching actions asynchronously or accessing browser data.

### Reselect

Reselect is a library used for slicing your redux state and providing only the relevant sub-tree to a react component. It has three key features:

1.  Computational power
2.  Memoization
3.  Composability

Imagine an application that shows a list of users. Its redux state tree stores an array of usernames with signatures:

`{ id: number, username: string, gender: string, age: number }`.

Let's see how the three features of reselect help.

- **Computation:** While performing a search operation, reselect will filter the original array and return only matching usernames. Redux state does not have to store a separate array of filtered usernames.
- **Memoization:** A selector will not compute a new result unless one of its arguments change. That means, if you are repeating the same search once again, reselect will not filter the array over and over. It will just return the previously computed, and subsequently cached, result. Reselect compares the old and the new arguments and then decides whether to compute again or return the cached result.
- **Composability:** You can combine multiple selectors. For example, one selector can filter usernames according to a search key and another selector can filter the already filtered array according to gender. One more selector can further filter according to age. You combine these selectors by using `createSelector()`


### Redux Saga

If your application is going to interact with some back-end application for data, we recommend using redux saga for side effect management. Too much jargon? Let's simplify.

Imagine that your application is fetching data in json format from a back-end. For every API call, ideally you should define at least three kinds of [action creators](http://redux.js.org/docs/basics/Actions.html):

1.  `API_REQUEST`: Upon dispatching this, your application should show a spinner to let the user know that something's happening.
2.  `API_SUCCESS`: Upon dispatching this, your application should show the data to the user.
3.  `API_FAILURE`: Upon dispatching this, your application should show an error message to the user.

And this is only for **_one_** API call. In a real-world scenario, one page of your application could be making tens of API calls. How do we manage all of them effectively? This essentially boils down to controlling the flow of your application. What if there was a background process that handles multiple actions simultaneously, communicates with the Redux store and react containers at the same time? This is where redux-saga comes into the picture.

The mental model is that a saga is like a separate thread in your application that's solely responsible for side effects. `redux-saga` is a redux middleware, which means this thread can be started, paused and cancelled from the main application with normal redux actions, it has access to the full redux application state and it can dispatch redux actions as well.

### Linting

This includes a guide static code analysis setup. It's composed of [ESLint](http://eslint.org/), [stylelint](https://stylelint.io/), and [Prettier](https://prettier.io/).

We recommend that you install the relevant IDE extensions for each one of these tools. Once you do, every time you'll press save, all your code will be formatted and reviewed for quality automatically. (Read more at [editor.md](./editor.md).)

We've also set up a git hook to automatically analyze and fix linting errors before your code is committed. If you'd like to disable it or modify its behavior, take a look at the `lint` section in `package.json`.

## The Jiangren Website.
The best code bootcamp in Australia
Visit [The best code bootcamp in Australia: JR Academy](https://jiangren.com.au).

## Australia IT Professional Community

[Sydney JR Academy | Code bootcamp](https://jiangren.com.au/city/sydney).

[Melbourne JR Academy | Code bootcamp](https://jiangren.com.au/city/melbourne).

[Brisbane JR Academy | Code bootcamp](https://jiangren.com.au/city/brisbane).

[JR Talent](https://jrtalent.com.au)