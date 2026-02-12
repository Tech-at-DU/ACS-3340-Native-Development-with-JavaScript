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

# ✅ Project Setup + Dependency Checklist

You should start with the **blank Expo template**. It keeps the project minimal and avoids preloaded navigation or extra boilerplate.

## 1) Create the project

```bash
npx create-expo-app@latest animal-explorer --template blank
cd animal-explorer
npx expo start
```

Make sure the app runs **before** you start adding code.

## 2) Install dependencies

### Navigation (Tabs)

```bash
npm install @react-navigation/native @react-navigation/bottom-tabs
npx expo install react-native-screens react-native-safe-area-context
```

### Redux Toolkit

```bash
npm install @reduxjs/toolkit react-redux
```

### Persistence (AsyncStorage)

```bash
npx expo install @react-native-async-storage/async-storage
```

### Icons

Expo includes `@expo/vector-icons` already. You do **not** need to install `react-native-vector-icons`.

## 3) Quick sanity check

After installing, restart Metro:

```bash
npx expo start -c
```

If you see “Unable to resolve module …” errors, it usually means:

- you missed a dependency install
- Metro cache needs to be cleared (`-c`)
- your import path is wrong

---

# 🧰 Starter Code (Scaffold)

Copy the files below into your project. You will fill in the TODOs as you work through the signposts.

## Suggested folder structure

```
.
├── App.js
├── store.js
├── features
│   └── animals
│       └── animalsSlice.js
└── screens
    ├── AnimalListScreen.js
    └── FavoritesScreen.js
```

## `App.js`

```js
import * as React from 'react';
import { Provider, useSelector } from 'react-redux';
import { NavigationContainer } from '@react-navigation/native';
import { createBottomTabNavigator } from '@react-navigation/bottom-tabs';
import { Ionicons } from '@expo/vector-icons';
import AsyncStorage from '@react-native-async-storage/async-storage';

import store from './store';
import AnimalListScreen from './screens/AnimalListScreen';
import FavoritesScreen from './screens/FavoritesScreen';
import { setFavorites } from './features/animals/animalsSlice';

const Tab = createBottomTabNavigator();

/**
 * RootNav is separated so we can use hooks (dispatch/select) after Provider.
 */
function RootNav() {
  // NOTE: In Signpost 7 you will hydrate favorites here.
  // TODO (Signpost 7): load favorites from AsyncStorage and dispatch(setFavorites(...))
  // Hint:
  // React.useEffect(() => {
  //   const load = async () => {
  //     const stored = await AsyncStorage.getItem('favorites');
  //     if (stored) dispatch(setFavorites(JSON.parse(stored)));
  //   };
  //   load();
  // }, []);

  const favoritesCount = useSelector((state) => state.animals.favorites.length);

  return (
    <NavigationContainer>
      <Tab.Navigator
        screenOptions={({ route }) => ({
          headerTitleAlign: 'center',
          tabBarIcon: ({ focused, color, size }) => {
            const icons = {
              Explore: focused ? 'paw' : 'paw-outline',
              Favorites: focused ? 'heart' : 'heart-outline',
            };
            return <Ionicons name={icons[route.name]} size={size} color={color} />;
          },
          tabBarActiveTintColor: 'tomato',
          tabBarInactiveTintColor: 'gray',
        })}
      >
        <Tab.Screen name="Explore" component={AnimalListScreen} />
        <Tab.Screen
          name="Favorites"
          component={FavoritesScreen}
          options={{
            // Optional badge: show count when > 0
            tabBarBadge: favoritesCount > 0 ? favoritesCount : undefined,
          }}
        />
      </Tab.Navigator>
    </NavigationContainer>
  );
}

export default function App() {
  return (
    <Provider store={store}>
      <RootNav />
    </Provider>
  );
}
```

## `store.js`

```js
import { configureStore } from '@reduxjs/toolkit';
import animalsReducer from './features/animals/animalsSlice';

const store = configureStore({
  reducer: {
    animals: animalsReducer,
  },
});

export default store;
```

## `features/animals/animalsSlice.js`

```js
import { createSlice, createAsyncThunk } from '@reduxjs/toolkit';

export const fetchAnimals = createAsyncThunk(
  'animals/fetchAnimals',
  async () => {
    // TODO (Signpost 2): fetch 10 random dog images and return an array of URLs
    // API: https://dog.ceo/api/breeds/image/random/10
    // Expected: return data.message
    throw new Error('Not implemented');
  }
);

const animalsSlice = createSlice({
  name: 'animals',
  initialState: {
    animals: [],
    favorites: [],
    status: 'idle', // 'idle' | 'loading' | 'succeeded' | 'failed'
    error: null,
  },
  reducers: {
    addFavorite: (state, action) => {
      // TODO (Signpost 5): add the URL in action.payload to favorites (avoid duplicates)
    },
    removeFavorite: (state, action) => {
      // TODO (Signpost 5): remove the URL in action.payload from favorites
    },
    setFavorites: (state, action) => {
      // TODO (Signpost 7): replace favorites with action.payload
    },
  },
  extraReducers: (builder) => {
    builder
      .addCase(fetchAnimals.pending, (state) => {
        // TODO (Signpost 2): set status to 'loading' and clear error
      })
      .addCase(fetchAnimals.fulfilled, (state, action) => {
        // TODO (Signpost 2): set status to 'succeeded' and set animals = action.payload
      })
      .addCase(fetchAnimals.rejected, (state, action) => {
        // TODO (Signpost 2): set status to 'failed' and set error = action.error.message
      });
  },
});

export const { addFavorite, removeFavorite, setFavorites } = animalsSlice.actions;
export default animalsSlice.reducer;
```

## `screens/AnimalListScreen.js`

```js
import * as React from 'react';
import { View, Text, FlatList, Image, Pressable, ActivityIndicator, StyleSheet } from 'react-native';
import { useDispatch, useSelector } from 'react-redux';
import AsyncStorage from '@react-native-async-storage/async-storage';

import { fetchAnimals, addFavorite, removeFavorite } from '../features/animals/animalsSlice';

export default function AnimalListScreen() {
  const dispatch = useDispatch();
  const { animals, favorites, status, error } = useSelector((state) => state.animals);

  React.useEffect(() => {
    // TODO (Signpost 3): dispatch(fetchAnimals())
  }, [dispatch]);

  React.useEffect(() => {
    // TODO (Signpost 6): persist favorites to AsyncStorage whenever favorites changes
    // Hint:
    // AsyncStorage.setItem('favorites', JSON.stringify(favorites));
  }, [favorites]);

  const toggleFavorite = (url) => {
    const isFav = favorites.includes(url);
    dispatch(isFav ? removeFavorite(url) : addFavorite(url));
  };

  if (status === 'loading' && animals.length === 0) {
    return (
      <View style={styles.center}>
        <ActivityIndicator size="large" />
        <Text style={styles.help}>Loading animals…</Text>
      </View>
    );
  }

  if (status === 'failed' && animals.length === 0) {
    return (
      <View style={styles.center}>
        <Text style={styles.error}>Error: {error}</Text>
        <Pressable style={styles.button} onPress={() => dispatch(fetchAnimals())}>
          <Text style={styles.buttonText}>Retry</Text>
        </Pressable>
      </View>
    );
  }

  return (
    <View style={styles.container}>
      <FlatList
        data={animals}
        keyExtractor={(item, index) => `${item}-${index}`}
        contentContainerStyle={styles.listContent}
        // TODO (Signpost 4): add pull-to-refresh using refreshing + onRefresh
        renderItem={({ item }) => {
          const isFav = favorites.includes(item);
          return (
            <View style={styles.card}>
              <Image source={{ uri: item }} style={styles.image} />
              <View style={styles.row}>
                <Text style={styles.label}>{isFav ? '★ Favorite' : '☆ Not favorite'}</Text>
                <Pressable style={styles.button} onPress={() => toggleFavorite(item)}>
                  <Text style={styles.buttonText}>{isFav ? 'Remove' : 'Favorite'}</Text>
                </Pressable>
              </View>
            </View>
          );
        }}
      />
    </View>
  );
}

const styles = StyleSheet.create({
  container: { flex: 1 },
  listContent: { padding: 12 },
  card: { marginBottom: 12, borderWidth: 1, borderColor: '#ddd', borderRadius: 10, overflow: 'hidden' },
  image: { height: 220, width: '100%' },
  row: { flexDirection: 'row', justifyContent: 'space-between', alignItems: 'center', padding: 12 },
  label: { fontSize: 16 },
  button: { paddingVertical: 10, paddingHorizontal: 14, borderRadius: 8, backgroundColor: '#f4511e' },
  buttonText: { color: 'white', fontWeight: '600' },
  center: { flex: 1, justifyContent: 'center', alignItems: 'center', padding: 16 },
  error: { color: '#b00020', fontSize: 16, marginBottom: 10, textAlign: 'center' },
  help: { marginTop: 10, color: '#444' },
});
```

## `screens/FavoritesScreen.js`

```js
import * as React from 'react';
import { View, Text, FlatList, Image, StyleSheet } from 'react-native';
import { useSelector } from 'react-redux';

export default function FavoritesScreen() {
  const favorites = useSelector((state) => state.animals.favorites);

  if (favorites.length === 0) {
    return (
      <View style={styles.center}>
        <Text style={styles.title}>No favorites yet</Text>
        <Text style={styles.subtitle}>Go to Explore and tap Favorite on an image.</Text>
      </View>
    );
  }

  return (
    <View style={styles.container}>
      <FlatList
        data={favorites}
        keyExtractor={(item, index) => `${item}-${index}`}
        contentContainerStyle={styles.listContent}
        renderItem={({ item }) => (
          <View style={styles.card}>
            <Image source={{ uri: item }} style={styles.image} />
          </View>
        )}
      />
    </View>
  );
}

const styles = StyleSheet.create({
  container: { flex: 1 },
  listContent: { padding: 12 },
  card: { marginBottom: 12, borderWidth: 1, borderColor: '#ddd', borderRadius: 10, overflow: 'hidden' },
  image: { height: 220, width: '100%' },
  center: { flex: 1, justifyContent: 'center', alignItems: 'center', padding: 16 },
  title: { fontSize: 22, fontWeight: '700', marginBottom: 8 },
  subtitle: { fontSize: 16, color: '#555', textAlign: 'center' },
});
```

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
