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
