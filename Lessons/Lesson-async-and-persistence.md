# 🐶 ACS 3340 – In-Class Build

Animal Explorer (Async + Persistence)

## 🧠 Learning Goals

By the end of class students will:
- Fetch remote data using Redux Toolkit async thunks
- Handle loading + error states
- Implement pull-to-refresh
- Persist data using AsyncStorage
- Hydrate state on app launch
- Understand server state vs local state

## ⏱ Class Timeline Overview

Time	Activity
0:00–0:15	Setup + architecture overview
0:15–0:45	Redux slice + async thunk
0:45–1:15	Connect UI + loading/error states
1:15–1:30	Break
1:30–2:00	Favorites + Redux reducers
2:00–2:20	AsyncStorage persistence
2:20–2:35	Hydration on launch
2:35–2:45	Reflection + extension challenge

## 🏗 Project Overview

You will build: Animal Explorer

Features:
- Fetch 10 dog images from API
- Display in FlatList
- Pull to refresh
- Add/remove favorites
- Favorites tab
- Persist favorites
- Restore favorites on restart

## 🔰 SIGNPOST 1 – Project Setup (15 min)

### Step 1 – Install AsyncStorage

```
npx expo install @react-native-async-storage/async-storage
```

### Step 2 – Create Redux Slice

Create:

```js
features/animals/animalsSlice.js
```

Server state lives in async *thunks*.
UI state lives in reducers.

## 🔰 SIGNPOST 2 – Async Thunk (30 min)

Goal: Fetch 10 dogs from API

```JS
export const fetchAnimals = createAsyncThunk(
  'animals/fetchAnimals',
  async () => {
    const response = await fetch(
      'https://dog.ceo/api/breeds/image/random/10'
    );
    const data = await response.json();
    return data.message;
  }
);
```

Add extraReducers:
- pending → status = loading
- fulfilled → animals = payload
- rejected → status = failed

CHECKPOINT

You should see:
- animals array in Redux DevTools
- status updating correctly

Ask:
- What happens if network fails?

## 🔰 SIGNPOST 3 – Connect UI (30 min)

Inside AnimalList screen:

```js
useEffect(() => {
  dispatch(fetchAnimals());
}, []);

Render states:

if (status === 'loading') {
  return <ActivityIndicator size="large" />;
}

if (status === 'failed') {
  return <Text>Error: {error}</Text>;
}
```

Add FlatList:

```JSX
<FlatList
  data={animals}
  keyExtractor={(item, index) => index.toString()}
  renderItem={({ item }) => (
    <Image
      source={{ uri: item }}
      style={{ height: 200, marginBottom: 10 }}
    />
  )}
/>
```

## 🔰 SIGNPOST 4 – Pull to Refresh (10 min)

Add:

```JS
refreshing={status === 'loading'}
onRefresh={() => dispatch(fetchAnimals())}
```

Explain: This is real mobile UX.

Test it live.

## ☕ BREAK (15 min)

## 🔰 SIGNPOST 5 – Favorites Feature (30 min)

Add reducers:

```JS
addFavorite
removeFavorite
```

Add favorite button under each image.

Challenge students:
- How do we check if item is already favorite?
- How do we toggle?

Add second tab:
FavoritesScreen

Display: const favorites = useSelector(state => state.animals.favorites)

CHECKPOINT:
- Add/remove favorites works
- Favorites tab updates

## 🔰 SIGNPOST 6 – Persistence (20 min)

Add AsyncStorage.

Redux state disappears when app closes.
You persist only important data.

Add effect in root:

```JS
const favorites = useSelector(...);

useEffect(() => {
  AsyncStorage.setItem(
    'favorites',
    JSON.stringify(favorites)
  );
}, [favorites]);
```

Test:
- Add favorite
- Reload app
- It disappears

Ask:

Why did it disappear?

Because we didn’t hydrate.

🔰 SIGNPOST 7 – Hydration (15 min)

In root component:

useEffect(() => {
  const loadFavorites = async () => {
    const stored = await AsyncStorage.getItem('favorites');
    if (stored) {
      dispatch(setFavorites(JSON.parse(stored)));
    }
  };

  loadFavorites();
}, []);

Now test:
- Add favorite
- Restart app
- It persists

✨ This is the “real app” moment.

⸻

🔰 SIGNPOST 8 – Architecture Discussion (10 min)

Draw this on board:

Server → Redux → UI
           ↓
     AsyncStorage
           ↓
       Rehydrate

Discuss:
- Should we persist animals list?
- Should we persist loading state?
- What is server state vs local state?

⸻

🧪 Extension Challenge (Final 10 min)

Students choose one:
- Add empty state for favorites
- Add badge to tab with favorites count
- Add optimistic favorite toggle
- Add error retry button
- Add animation when favorite is added

⸻

🎯 What Students Built

In one class they implemented:
- Async thunk
- Loading + error handling
- Pull-to-refresh
- Redux state management
- AsyncStorage persistence
- Hydration logic
- Multi-tab architecture

That’s serious mobile engineering.

⸻

💡 Homework (Optional)

Refactor:
- Move persistence logic into custom hook
- Extract selectors
- Improve folder structure
- Add dark mode
- Add optimistic update

⸻

🧭 Why This Works

This class session:
- Connects Redux to real-world async
- Introduces mobile persistence
- Demonstrates architecture patterns
- Feels like a complete mini app
- Directly scaffolds the final project

⸻

If you’d like next, I can:
- Write a fully scaffolded starter repo structure
- Create a slide outline for you to present this
- Design a follow-up class focused on device APIs
- Create a debugging checklist students can use

Tell me what you want to build next.
# 🐶 ACS 3340 – In-Class Project  
# Animal Explorer (Async + Persistence)

## Overview

In this project you will build a small but realistic mobile application that:

- Fetches remote data using Redux Toolkit
- Handles loading and error states
- Supports pull-to-refresh
- Allows users to favorite items
- Persists favorites using AsyncStorage
- Restores favorites when the app restarts

You may work individually or in pairs.

By the end of this session, you should have a working mini app that demonstrates real-world mobile app architecture.

---

# 🎯 Learning Objectives

By completing this project, you will be able to:

- Use `createAsyncThunk` for async data fetching
- Manage loading and error state in Redux
- Implement pull-to-refresh in a FlatList
- Persist application data with AsyncStorage
- Hydrate Redux state on app launch
- Explain the difference between server state and local state

---

# 🏗 Project Requirements

You will build **Animal Explorer** with the following features:

- Fetch 10 dog images from a public API
- Display images in a FlatList
- Pull-to-refresh to fetch new images
- Add and remove favorites
- Display favorites in a second tab
- Persist favorites between app launches
- Restore favorites when the app loads

---

# 🔰 SIGNPOST 1 – Project Setup

### Step 1 – Install AsyncStorage

Run:

```bash
npx expo install @react-native-async-storage/async-storage
```

### Step 2 – Create a Redux Slice

Create the file:

```
features/animals/animalsSlice.js
```

Your slice should include:

Initial state:

```js
{
  animals: [],
  favorites: [],
  status: 'idle',
  error: null
}
```

Think about this:

- Server state will come from async thunks.
- UI state and favorites will live in reducers.

---

# 🔰 SIGNPOST 2 – Create an Async Thunk

Your goal: fetch 10 random dog images.

Use this API:

```
https://dog.ceo/api/breeds/image/random/10
```

Create an async thunk called `fetchAnimals`.

When finished, your slice should handle:

- pending → status = "loading"
- fulfilled → status = "succeeded" and update animals
- rejected → status = "failed" and store error

### ✅ Checkpoint

You should be able to:

- Dispatch `fetchAnimals`
- See `status` change in Redux DevTools
- See `animals` update in state

### Think

What should happen if the network request fails?

---

# 🔰 SIGNPOST 3 – Connect Redux to the UI

In your AnimalList screen:

1. Dispatch `fetchAnimals()` inside `useEffect`
2. Use `useSelector` to access:
   - animals
   - status
   - error

Render different UI based on state:

```js
if (status === 'loading') {
  return <ActivityIndicator size="large" />;
}

if (status === 'failed') {
  return <Text>Error: {error}</Text>;
}
```

Render a FlatList of images.

### ✅ Checkpoint

You should see 10 dog images displayed.

---

# 🔰 SIGNPOST 4 – Add Pull to Refresh

Enhance your FlatList with:

```js
refreshing={status === 'loading'}
onRefresh={() => dispatch(fetchAnimals())}
```

### Test

Pull down on the list to refresh.

This is standard mobile UX.

---

# 🔰 SIGNPOST 5 – Add Favorites

Add reducers:

- `addFavorite`
- `removeFavorite`

Add a button under each image that:

- Adds the image to favorites
- Or removes it if already favorited

### Create a Second Tab

Add a Favorites screen that:

- Uses `useSelector`
- Displays all favorite images

### ✅ Checkpoint

You should be able to:

- Add favorites
- Remove favorites
- See favorites update in real time
- See favorites listed in the second tab

---

# 🔰 SIGNPOST 6 – Persistence with AsyncStorage

Right now, if you reload the app, favorites disappear.

That’s because Redux state is temporary.

You will now persist favorites.

In your root component:

```js
const favorites = useSelector(state => state.animals.favorites);

useEffect(() => {
  AsyncStorage.setItem(
    'favorites',
    JSON.stringify(favorites)
  );
}, [favorites]);
```

### Test

1. Add a favorite
2. Reload the app

What happens?

---

# ❓ Reflection Question

Why did the favorite disappear after reload?

Write your answer before continuing.

---

# 🔰 SIGNPOST 7 – Hydration on App Launch

Now you will restore persisted data when the app starts.

In your root component:

```js
useEffect(() => {
  const loadFavorites = async () => {
    const stored = await AsyncStorage.getItem('favorites');
    if (stored) {
      dispatch(setFavorites(JSON.parse(stored)));
    }
  };

  loadFavorites();
}, []);
```

### ✅ Checkpoint

1. Add a favorite
2. Close the app
3. Restart it

Your favorites should now persist.

---

# 🧠 Architecture Discussion

Draw this diagram in your notes:

```
Server → Redux → UI
           ↓
     AsyncStorage
           ↓
        Rehydrate
```

Answer these questions:

- Should we persist the animals list? Why or why not?
- Should we persist loading state?
- What is the difference between server state and local user state?

---

# 🧪 Extension Challenge (Optional)

Choose one:

- Add an empty state message for Favorites
- Add a badge to the Favorites tab with count
- Add a retry button when network fails
- Add animation when a favorite is added
- Prevent duplicate favorites

---

# ✅ Final Checklist

Your app should now include:

- Async thunk
- Loading and error handling
- Pull-to-refresh
- Redux state management
- AsyncStorage persistence
- Hydration logic
- Multi-tab navigation

---

# 💡 Reflection

Before leaving class, write a short paragraph:

- What part of this project felt most confusing?
- What concept feels most important for your final project?

---

This project introduces core mobile architecture patterns you will use in your final project.