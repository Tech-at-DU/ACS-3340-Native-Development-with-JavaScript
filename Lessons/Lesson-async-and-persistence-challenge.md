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
