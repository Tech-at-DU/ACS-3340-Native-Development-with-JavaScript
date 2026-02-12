# ACS 3340 – Final Project

Build a Production-Ready Mobile App

## Overview

Your final project is an opportunity to demonstrate your understanding of React Native, navigation, state management, and mobile application architecture.

This project should feel like a real mobile app — something you could reasonably ship to the App Store or Play Store.

You will design and build a mobile application of your own choosing that integrates multiple concepts from this course.

## Project Requirements

Your app must include the following components:

### 1️⃣ Navigation Architecture

Your app must include:
	•	A Bottom Tab Navigator with at least three tabs
	•	At least one nested Stack Navigator
	•	At least one screen pushed onto a stack
	•	A clearly organized navigation structure

Navigation should feel intentional and usable.

### 2️⃣ State Management (Redux Toolkit)

Your app must:
	•	Use Redux Toolkit
	•	Include at least one slice of meaningful global state
	•	Include at least one async thunk
	•	Avoid unnecessary state in navigation params

Redux should manage real application state — not just trivial examples.

### 3️⃣ Data & Async Behavior

Your app must include one of the following:
	•	Fetch data from a public API
	•	Connect to a backend service
	•	Simulate a backend with async logic

Your app must also include:
	•	A loading state
	•	An error state
	•	A refresh mechanism (e.g., pull-to-refresh or manual refresh)

### 4️⃣ Persistence

Your app must:
	•	Use AsyncStorage (or equivalent)
	•	Persist user-generated data (e.g., favorites, settings, notes)
	•	Restore persisted state on app launch

The app should feel stateful across sessions.

### 5️⃣ Native / Device Integration

Your app must integrate at least one native capability:

Choose one:
	•	Camera
	•	Image picker
	•	Location
	•	Haptics
	•	Share API
	•	Notifications
	•	File system
	•	Clipboard
	•	Linking (deep links or external URLs)

You must handle permissions properly.

### 6️⃣ UI / Design System

Your app must demonstrate thoughtful UI design:

Choose one:
	•	Use a component library (React Native Elements, NativeBase, etc.) with theming
	•	OR implement your own theme system

Your app must include:
	•	Consistent spacing and typography
	•	At least one animated interaction
	•	Dark mode toggle OR a themed color system

### 7️⃣ Animation

Your app must include at least one animation:

Examples:
	•	Animated list insertion
	•	Modal transition
	•	Button press microinteraction
	•	Layout animation
	•	Tab icon animation
	•	Screen transition customization

## Project Scope

You have approximately three weeks.

Your project should:
	•	Be ambitious but realistic
	•	Be feature-complete
	•	Demonstrate polish
	•	Avoid unnecessary complexity

You are encouraged to:
	•	Start simple
	•	Build incrementally
	•	Refactor as needed

Suggested Project Ideas
	•	Habit Tracker
	•	Fitness Log
	•	Mood Journal
	•	Travel Journal
	•	Recipe Manager
	•	Book Tracker
	•	Pet Adoption Browser
	•	Personal Finance Tracker
	•	Event Planner
	•	Movie Discovery App

You may propose your own idea.

## Deliverables

You must submit:
	•	GitHub repository
	•	Working Expo project
	•	README including:
	•	App description
	•	Architecture overview
	•	Features implemented
	•	Known limitations
	•	Screenshots or screen recording

## Assessment Rubric

| Category              | Does Not Meet                         | Meets                           | Exceeds                                 |
|-----------------------|---------------------------------------|---------------------------------|-----------------------------------------|
| Completion          | Missing required features          | All required features implemented    | Includes additional thoughtful features    |
| Navigation          | Incomplete or broken navigation    | Functional tab + stack structure     | Clean, intuitive navigation architecture   |
| State Management    | Minimal Redux usage                | Meaningful slices + async logic      | Clean selectors, structure, separation of concerns |
| Async + Persistence | No loading/error handling          | Correct async + persistence behavior | Robust error handling + clean hydration    |
| Native Integration  | Not implemented                    | One native API integrated            | Well-integrated + polished UX              |
| UI / UX             | Inconsistent design                | Consistent layout and styling        | Thoughtful UX + animations                 |
| Code Quality        | Errors or warnings                 | Clean, functional code               | Organized structure + professional formatting |
| Development Process | Few commits                        | Regular commits                      | Clear commit history showing iteration     |

## Proposal Requirement

Before beginning full implementation, submit a brief proposal including:
	•	App name
	•	Core idea
	•	Target user
	•	List of required features
	•	Which native API you will integrate
	•	Rough navigation structure
	•	Wireframe sketches (hand-drawn is fine)

Instructor approval is required before proceeding.

## What This Project Demonstrates

By completing this project, you will demonstrate:
	•	Mobile application architecture
	•	State management in React Native
	•	Async data handling
	•	Device integration
	•	UX design
	•	Real-world development practices
