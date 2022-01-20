# ACS 3340 - Redux Review

<!-- > -->

## Review and Dig into Redux

<!-- > -->

## Learning Objectives 

1. Define Actions, Reducers, and Store
1. Use Redux to manage application state 
1. Create container components implement React Redux
1. Use the react controlled component pattern

<!-- > -->

## Why practice the Redux Todo App?

<!-- > -->

The controlled component pattern is a common pattern to know with Redux. You should know it!

You'll need this to handle forms with React.

<!-- > -->

Redux is an industry standard tool for managing application state. You should know it!

<!-- > -->

## Build a todolist ✅

<!-- > -->

Get the [sample code](https://github.com/soggybag/redux-todo). 

<!-- > -->

The starting application will: 

- make new todo items 
- remove todo items. 

<!-- > -->

Items are stored in Redux. 

`TodoList.js` component displays a list of `Todo.js` components. 

<!-- > -->

New Todo uses the [controlled pattern](https://reactjs.org/docs/forms.html). 

Use this with all form elements. 

<!-- > -->

### Organizing code 

<!-- > -->

The project contains several components organized in folders. 

- `App` : The main view of the project. 
- `todo-new` : creates a new todo with a name
- `todo-list` : Displays a list of `todo` items
- `todo` : Displays a single todo

Components that use more than one file are organized in folders. 

<!-- > -->

### App State

<!-- > -->

Here application state is an array of Objects. Each of these is an instance of `./components/TodoItem.js`.

<!-- > -->

Look at `./reducers/todos-reducer.js`. The reducer defines the initial state and how changes are made to state. 

<!-- > -->

Currently, there are two actions that are handled by this reducer: 

- `NEW_TODO`: adds a new todo item to the array
- `DELETE_TODO`: removes a todo item from the array

In both cases a new array is created and returned. 

**Important!** A reducer must always return new state! 

<!-- > -->

### App state only changes by sending actions

Application state can only be changed by sending actions.

<!-- > -->

`./actions/index.js`. 

This app defines two actions:

- `NEW_TODO`: adds a new todo item to the array
- `DELETE_TODO`: removes a todo item from the array

<!-- > -->

In this app you can only create a new todo item or delete an existing one. 

<!-- > -->

Every action string also has a coresponding action creator. This is a function that returns an object with a type and a payload. 

<!-- > -->

These action objects are what is sent to the reducer and are what the reducer uses to handle making changes to application state. 

<!-- > -->

#### Action types

<!-- > -->

The action type is a string. Defining this as a constant is best practice. This prevents errors are makes it easy to edit if needed. 

<!-- > -->

#### Action Payload

<!-- > -->

Some actions might require more data to accomplish its purpose. For example, creating a new todo item requires the name of the todo. 

<!-- > -->

To delete a todo you need the index of the todo item to be deleted. 

<!-- > -->

### Todo List challenges

Try these challenges to test your understanding of Redux. 

<!-- > -->

**Challenge 1** Display completed

Currently todos have the following properties: 

- name
- completed
- date

Only the name is currently displayed. Modify `./component/todo.js` to display the completed state. 

You can do this by adding a check mark or changing the color of text or background of this component.

<!-- > -->

**Challenge 2** Mark todos complete

Add a button to the `Todo` component that will mark a todo as completed. Follow these steps: 

- Add a new action `MARK_COMPLETE`
- Add an action creator function `markComplete`
	- Supply index of the todo as a parameter
	- It should also include the index in the payload
- Modify the `todosReducer.js` to handle this change. 
	- Import the action type
	- Add a new case for this type
	- Set the completed property of a the todo with the matching index

<!-- > -->

**Challenge 3** Mark all complete 

Make a new button that marks all todos complete.

<!-- > -->

## After Class

- Complete your React/Redux tutorial from [Lesson 1](Lesson-01.md)

<!-- > -->

## Additional Resources

- [React](https://reactjs.org)
- [Redux](https://redux.js.org)
- [Password App](https://github.com/Tech-at-DU/React-Redux-passwords-Tutorial)
- [Timers App](https://github.com/Tech-at-DU/React-Redux-Timers-Tutorial)
- [Tetris App](https://github.com/Tech-at-DU/React-Redux-Tetris-Tutorial) 
- [Hiring Trends](https://www.hntrends.com/2018/jun-no-signs-of-slowing-for-react.html?compare1=React&compare2=Redux&compare3=Angular+2&compare4=AngularJS)
- [NPM Trends](https://npm-stat.com/charts.html?package=react&package=vue&package=angular&package=angular%202&package=redux&from=2016-06-01&to=2018-05-31)
