# ACS 3340 - Electron Build

<!-- > -->

## Review

- Name three reference types in JS? 
- Name three value types in JavaScript?
- What is Electron?

<!-- > -->

## Review the Current Project

You should be using your product/project (remember that dog fooding disucssion?) looking for improvements. 

<!-- > -->

- How did you use your product? 
- What did you observe that was working well?
- What did you observe that needed improvement? Name three things...

<!-- > -->

## Building Electron

<!-- > -->

The instructions here continue from what was covered in class. If you haven't followed the instructions from class 3 be sure to do that now before continuing. 

<!-- > -->

Import Electron forge to build the app binary: 

```
npm install --save-dev @electron-forge/cli
npm exec --package=@electron-forge/cli -c "electron-forge import"
```

This should add some new scripts to your package.json. Check teh scripts section and look for: 

```
"package": "electron-forge package",
"make": "electron-forge make"
```

These instructions follow the guide here: https://www.electronforge.io/import-existing-project

<!-- > -->

## Customizing the App

<!-- > -->

The `electron.js` file has configuration code that is used to by the process that runs the electron app. The HTML/CSS/JS code that was your original CRA project is displayed by electron and is the user interface for your project.

<!-- > -->

You can modify the application in a few says. Try changing the size of the app and adding an icon. 

<!-- > -->

While the icon might not sound important in reality it is. It's the first thing people see when they use your app. So it's a chance to brand your product and set impressions. It's also required to publish your apps to the app store. Without an icon your app will automatically be rejected. 

<!-- > -->

Icons are actually more complex than you might think. You'll need images for all of the different screen resolutions your app might support. 

- Set window size in `electron.js:11`
	- `mainWindow = new BrowserWindow({width: 400, height: 600});`
- App Icon
	- https://dev.to/onmyway133/changing-electron-app-icon
  - https://www.christianengvall.se/electron-app-icons/
	- https://www.electron.build/icons

Here are some alternative instructions for building Electron with Electron Builder: https://reactjsexample.com/electron-integration-with-react/

<!-- > -->
	
## Build an Electron App

<!-- > -->

Your goal is to build a desktop application from your React project. Follow the guide above. Your deliverable is a functioning native app that runs on Mac or Windows. 

- Native App

<!-- > -->

## After Class

Build your Electron app. This will conclude the first assignment. 

<!-- > -->

## Resources

- https://dev.to/mandiwise/electron-apps-made-easy-with-create-react-app-and-electron-forge-560e
