# ACS 3340 - Electron Main and Rendering Process

Debugging Electron. 

Communication between the Main Process and rendering Processes. 

## Learning Objectives

1. Identify problems 
1. Create solutions
1. Use CSS to create user interfaces 
1. Implement communication between main and rendering processes 

## Initial Exercise

- CSS and UI issues
	- forms elements
- Electron Options 
	- Size of Window
	- Saving data

## Main Process vs Rendering Process

Electron apps are built on Chrome. This is similar to building web apps. The difference is that web apps run in the browser. With an electron app you also are in control of the browser itself. 

With web apps you as a web developer are in control of a single process that is your web page. As the developer of an Electron app you can run one process in each browser tab or window and another called the Main process that runs underneath all of the others. 

Read this page from the Electron docs: https://www.electronjs.org/docs/latest/tutorial/process-model It explains the process model used by Electron. 

Answer these questions when you're done:

- What is the main process? 
- What is the rendering process? 
- How do you communicated between rendering and main processes? 

## After Class

- Wrap up the Electron project. You should have a functional Electron application that runs on the desktop. 

## Additional Resources

- [Electron Passing Data Demo](https://github.com/soggybag/electron-passing-data-demo)
- [ipcRenderer](https://electronjs.org/docs/api/ipc-renderer)
- [ipcMain](https://electronjs.org/docs/api/ipc-main)
