# FEW 2.4 Class 2 - Redux Review 

## Review and Dig into Redux

## Learning Objectives 

1. Define Actions, Reducers, and Store
1. Use Redux to manage application state 
1. Create container components implement React Redux
1. Use the react controlled component pattern

## Why learn this?

The controlled component pattern is a common pattern to know with Redux. 

Redux is a great system from managing state. If you've ever struggled with complex application state in the past you know how much trouble you can have, amnd how much work it can be to reinvent the wheel for each new project. 

Using Redux will solve many common problems and give you a system that you apply to almost any project. 

## Build a todolist

Build a Todo list. Look at the [sample code](https://github.com/soggybag/redux-todo). 

Currently the application allows you to add items to the list. Each item shows a name and a "completed" state as an X on the right. 

Items are stored in Redux. `TodoList` displays a list of `Todo`. 

New Todo uses the [controlled pattern](https://reactjs.org/docs/forms.html). Use this with all form elements. 

### Organizing code

The project contains several components organized in folders. 

- `App` : The main view of the project. You can think of this as the entry point for you code. 
- `todo-new` : An input field and button that creates a new todo with a name
- `todo-list` : Displays a list of `todo` items
- `todo` : Displays a single todo item with a name and completed status

Components that use more than one file are organized in folders. 

### App State 

Here application state is an array of Objects. Each of these is an instance of `./components/TodoItem.js`.

Look at `./reducers/todos-reducer.js`. The reducer defines the initial state and how changes are made to state. 

Currently, there are two actions that are handled by this reducer: 

- `NEW_TODO`: adds a new todo item to the array
- `DELETE_TODO`: removes a todo item from the array

In both cases a new array is created and returned. 

**Important!** A reducer must always return new state! 

### App state only changes by sending actions

Application state can only be changed by sending actions.

Take a look at `./actions/index.js`. Here is where all of the actions are defined. This app defines two actions: `NEW_TODO` and `DELETE_TODO`.

- `NEW_TODO`: adds a new todo item to the array
- `DELETE_TODO`: removes a todo item from the array

In this app you can only create a new todo item or delete an existing one. 

Every action string also has a coresponding action creator. This is a function that returns an object with a type and a payload. 

These action objects are what is sent to the reducer and are what the reducer uses to handle making changes to application state. 

#### Action types

The action type is a string. Defining this as a constant is best practice. This prevents errors are makes it easy to edit if needed. 

#### Action Payload

Some actions might require more data to accomplish its purpose. For example, creating a new todo item requires the name of the todo. 

To delete a todo you need the index of the todo item to be deleted. 

### Todo List challenges

Try these challenges to test your understanding of Redux. 

**Challenge 1** Display completed

Currently todos have the following properties: 

- name
- completed
- date

Only the name is currently displayed. Modify `./component/todo.js` to display the completed state. 

You can do this by adding a check mark or changing the color of text or background of this component.

**Challenge 2** Mark todos complete

Add a button to the `Todo` component that will mark a todo as completed. Follow these steps: 

- Add a new action `MARK_COMPLETE`
- Add an action creator function `markComplete`
	- This function should take the index of the todo as a parameter
	- It should also include the index in the payload
- Modify the `todosReducer.js` to handle this change. 
	- Import the action type
	- Add a new case for this type
	- You need to set the completed property of a the todo with the matching index to true, copy and return the array of todos

**Challenge 3** Mark all complete 

Make a new button that marks all todos complete. 

### Using Stylesheets

Components have been styled using stylesheets. By convention  by making a name of the stylesheet match the component. 

Use class names within the stylesheet to assign styles to elements. 

Import the stylesheet into a component using: 

`import './component-name.css`

### Connecting Components 

Components can connect themselves to the store using 'react-redux'. This library provides the connect method for this purpose. 

`connect` takes two parameters. 

- `mapStateToProps(state)` takes state as a parameter and maps values to props on a components. This means you get valeus from state from props! The return value should be an object with the properties that you want to map to props. 
- `mapDispatchToProps()` this function returns an object with properties whose values are 

## Local Storage 

Local storage allows your apps to save data in the browser. This gives your apps the abilioty to save information without and internet connection, a login, database, or other tooling. 

Local storage saves data in key value pairs. Values must be simple either strings or numbers. 

This sounds pretty limited. Using JSON allows you to convert collections, like objects and arrays, to a string which can be saved. 

Save data with: 

`localStorage.setItem(key, data)`

Fetch data with: 

`localStorage.getItem(key)`

Convert complex values with `JSON.stringify()`

NOTE! Objects and Arrays can contain functions which can not be converted to JSON!

Use this with 

## Adding features 

You need to implement some features. How would you implement the following features? 

- Mark todos when completed
- Sort the todo list by completed
- Filter for completed or not completed
- Delete a todo from the list
- Edit the name of a todo in the list

## After Class

- Complete your tutorial from [Lesson 1](Lesson-01.md)

## Additional Resources

- [Incremental (Clicker) Games](https://en.wikipedia.org/wiki/Incremental_game)
- [Universal Paperclips]
- [React](https://reactjs.org)
- [Redux](https://redux.js.org)
- [Password App](https://github.com/MakeSchool-Tutorials/web-7-react-redux-passwords-app)
- [Timers App](https://github.com/MakeSchool-Tutorials/web-7-react-redux-timers-app)
- [Tetris App](https://github.com/MakeSchool-Tutorials/web-7-react-redux-tetris-app) 
- [Hiring Trends](https://www.hntrends.com/2018/jun-no-signs-of-slowing-for-react.html?compare1=React&compare2=Redux&compare3=Angular+2&compare4=AngularJS)
- [NPM Trends](https://npm-stat.com/charts.html?package=react&package=vue&package=angular&package=angular%202&package=redux&from=2016-06-01&to=2018-05-31)
