

# ACS 3340 — Theming in React Native

## 🎯 Learning Objectives

By the end of this lesson you will:

- Understand what “theming” means in a mobile application
- Build a reusable theme system using a theme object
- Implement dark mode using React Context
- Connect your theme to React Navigation
- Refactor hardcoded styles into a scalable design system

---

# ⏱ Class Plan (2:45)

| Time      | Activity                                         |
|-----------|--------------------------------------------------|
| 0:00–0:15 | Why Theming Matters (Discussion + Demo)          |
| 0:15–0:45 | Build the Sample App Scaffold (Tabs + UI Elements)|
| 0:45–1:15 | Build a Theme Object                             |
| 1:15–1:55 | Add Theme Context + Toggle                       |
| 1:55–2:20 | Connect Theme to Navigation                      |
| 2:20–2:40 | Refactor Hardcoded Styles (Tokens + Reuse)       |
| 2:40–2:45 | Reflection + Final Project Tie-In                |


# Part 0 — Build a Sample App Scaffold (30 min)

You will build a tiny app first. It is intentionally **ugly and hardcoded** so you can practice theming.

## What you will build

A 2‑tab app:

- **Home tab:** headings, text, and a couple buttons
- **List tab:** a list with a simple “row” layout (title on left, rating on right)

You will refactor the hardcoded colors/spacings into theme tokens.

## Dependencies

If you don’t already have navigation installed:

```bash
npm install @react-navigation/native
npx expo install react-native-screens react-native-safe-area-context
npm install @react-navigation/bottom-tabs
```

## File structure

Create these files:

```
.
├── App.js
└── screens
    ├── HomeScreen.js
    └── ListScreen.js
```

## 1) `App.js` (Tabs)

Paste this:

```js
import * as React from 'react';
import { NavigationContainer } from '@react-navigation/native';
import { createBottomTabNavigator } from '@react-navigation/bottom-tabs';
import { Ionicons } from '@expo/vector-icons';

import HomeScreen from './screens/HomeScreen';
import ListScreen from './screens/ListScreen';

const Tab = createBottomTabNavigator();

export default function App() {
  return (
    <NavigationContainer>
      <Tab.Navigator
        screenOptions={({ route }) => ({
          headerTitleAlign: 'center',
          tabBarIcon: ({ focused, color, size }) => {
            const icons = {
              Home: focused ? 'home' : 'home-outline',
              List: focused ? 'list' : 'list-outline',
            };
            return <Ionicons name={icons[route.name]} size={size} color={color} />;
          },
          tabBarActiveTintColor: 'tomato',
          tabBarInactiveTintColor: 'gray',
        })}
      >
        <Tab.Screen name="Home" component={HomeScreen} />
        <Tab.Screen name="List" component={ListScreen} />
      </Tab.Navigator>
    </NavigationContainer>
  );
}
```

## 2) `screens/HomeScreen.js` (Hardcoded UI)

Paste this:

```js
import * as React from 'react';
import { View, Text, StyleSheet, Pressable, Alert } from 'react-native';

export default function HomeScreen() {
  return (
    <View style={styles.container}>
      <Text style={styles.h1}>Theme Playground</Text>
      <Text style={styles.p}>
        This screen is intentionally hardcoded. Your job is to remove the hardcoded values
        and replace them with theme tokens.
      </Text>

      <View style={styles.card}>
        <Text style={styles.h2}>Card Title</Text>
        <Text style={styles.p}>
          A “card” is a common UI pattern. In a real app you will reuse this style many times.
        </Text>

        <View style={styles.row}>
          <Pressable style={styles.primaryButton} onPress={() => Alert.alert('Primary')}>
            <Text style={styles.primaryButtonText}>Primary</Text>
          </Pressable>

          <Pressable style={styles.secondaryButton} onPress={() => Alert.alert('Secondary')}>
            <Text style={styles.secondaryButtonText}>Secondary</Text>
          </Pressable>
        </View>
      </View>

      <Text style={styles.small}>
        Goal: When your theme toggles, this entire screen should change correctly.
      </Text>
    </View>
  );
}

const styles = StyleSheet.create({
  container: {
    flex: 1,
    padding: 16,
    backgroundColor: '#ffffff',
  },
  h1: { fontSize: 30, fontWeight: '800', color: '#111111', marginBottom: 10 },
  h2: { fontSize: 20, fontWeight: '700', color: '#111111', marginBottom: 6 },
  p: { fontSize: 16, color: '#333333', lineHeight: 22, marginBottom: 10 },
  small: { marginTop: 16, color: '#555555' },

  card: {
    padding: 16,
    borderRadius: 12,
    backgroundColor: '#f4f4f4',
    borderWidth: 1,
    borderColor: '#dddddd',
  },
  row: { flexDirection: 'row', gap: 10 },

  primaryButton: {
    backgroundColor: '#f4511e',
    paddingVertical: 12,
    paddingHorizontal: 14,
    borderRadius: 10,
  },
  primaryButtonText: { color: 'white', fontWeight: '700' },

  secondaryButton: {
    backgroundColor: '#222222',
    paddingVertical: 12,
    paddingHorizontal: 14,
    borderRadius: 10,
  },
  secondaryButtonText: { color: 'white', fontWeight: '700' },
});
```

## 3) `screens/ListScreen.js` (List + Rows)

Paste this:

```js
import * as React from 'react';
import { View, Text, FlatList, StyleSheet } from 'react-native';

const DATA = [
  { name: 'Malamute', rating: 5 },
  { name: 'Shiba', rating: 4 },
  { name: 'Corgi', rating: 5 },
  { name: 'Greyhound', rating: 3 },
  { name: 'Pug', rating: 2 },
  { name: 'Husky', rating: 4 },
];

export default function ListScreen() {
  return (
    <View style={styles.container}>
      <Text style={styles.h1}>Breed Ratings</Text>

      <FlatList
        data={DATA}
        keyExtractor={(item) => item.name}
        renderItem={({ item }) => (
          <View style={styles.item}>
            <Text style={styles.name}>{item.name}</Text>
            <Text style={styles.rating}>{'★'.repeat(item.rating)}</Text>
          </View>
        )}
        ItemSeparatorComponent={() => <View style={styles.sep} />}
      />
    </View>
  );
}

const styles = StyleSheet.create({
  container: { flex: 1, padding: 16, backgroundColor: '#ffffff' },
  h1: { fontSize: 26, fontWeight: '800', color: '#111111', marginBottom: 12 },

  item: {
    flexDirection: 'row',
    justifyContent: 'space-between',
    alignItems: 'center',
    paddingVertical: 14,
    paddingHorizontal: 12,
    borderRadius: 10,
    backgroundColor: '#f4f4f4',
    borderWidth: 1,
    borderColor: '#dddddd',
  },
  name: { fontSize: 18, fontWeight: '600', color: '#111111' },
  rating: { fontSize: 18, color: '#f4511e' },

  sep: { height: 10 },
});
```

## Quick Check (before theming)

- App runs
- Tabs work
- Home shows a card and buttons
- List shows rows with rating aligned right

If any of these fail, fix them before starting Part 2.

# Part 1 — Why Theming Matters (15 min)

## Discussion Prompt

Open the sample app you just built. Notice how many values are hardcoded and repeated across screens.

Ask yourself:

- What happens if we want dark mode?
- What happens if the brand color changes?
- What happens if 20 components use the same hardcoded value?

### Key Idea

Theming is about:

- Centralizing design decisions
- Making dark mode possible
- Making apps scalable
- Separating design from components

---

# Part 2 — Build a Theme Object (30 min)

## Step 1 — Create `theme.js`

Create a file:

```js
// theme.js

export const lightTheme = {
  colors: {
    background: '#ffffff',
    text: '#111111',
    primary: '#f4511e',
    card: '#f9f9f9',
  },
  spacing: {
    sm: 8,
    md: 16,
    lg: 24,
  }
};

export const darkTheme = {
  colors: {
    background: '#111111',
    text: '#ffffff',
    primary: '#ff784e',
    card: '#1e1e1e',
  },
  spacing: {
    sm: 8,
    md: 16,
    lg: 24,
  }
};
```

---

## Step 2 — Use the Theme Object (No Context Yet)

In a screen:

```js
import { lightTheme } from '../theme';

<View style={{ backgroundColor: lightTheme.colors.background }}>
```

### Activity (10 min)

Refactor the Home screen:
- Replace hardcoded colors
- Replace padding values with spacing tokens

Discuss:
- What improved?
- What still feels repetitive?

---

# Part 3 — Add Theme Context + Toggle (40 min)

Now we make it dynamic.

## Step 1 — Create ThemeContext

Create `ThemeProvider.js`

```js
import React from 'react';
import { lightTheme, darkTheme } from './theme';

export const ThemeContext = React.createContext();

export function ThemeProvider({ children }) {
  const [theme, setTheme] = React.useState(lightTheme);

  const toggleTheme = () => {
    setTheme(prev =>
      prev === lightTheme ? darkTheme : lightTheme
    );
  };

  return (
    <ThemeContext.Provider value={{ theme, toggleTheme }}>
      {children}
    </ThemeContext.Provider>
  );
}
```

---

## Step 2 — Wrap Your App

In `App.js`:

```js
import { ThemeProvider } from './ThemeProvider';

export default function App() {
  return (
    <ThemeProvider>
      <RootNav />
    </ThemeProvider>
  );
}
```

---

## Step 3 — Consume the Theme

```js
import { ThemeContext } from '../ThemeProvider';

const { theme, toggleTheme } = React.useContext(ThemeContext);

<View style={{ backgroundColor: theme.colors.background }}>
```

Add a button:

```js
<Button title="Toggle Theme" onPress={toggleTheme} />
```

---

## Active Learning (15 min)

In pairs:

- Add theme support to 2 screens
- Ensure text, background, and buttons switch correctly
- No hardcoded colors allowed

---

# Part 4 — Connect Theme to Navigation (30 min)

React Navigation supports theming.

In your navigation container:

```js
<NavigationContainer
  theme={{
    dark: theme === darkTheme,
    colors: {
      primary: theme.colors.primary,
      background: theme.colors.background,
      card: theme.colors.card,
      text: theme.colors.text,
      border: '#ccc',
      notification: theme.colors.primary,
    }
  }}
>
```

Now your header and tab bar will respond to theme changes.

---

## Activity (10 min)

- Style the tab bar
- Style the header
- Confirm dark mode works across navigation

---

# Part 5 — System Dark Mode (Optional Extension)

React Native includes:

```js
import { useColorScheme } from 'react-native';
```

```js
const scheme = useColorScheme();
```

It returns:

- `"light"`
- `"dark"`

### Challenge

Initialize theme based on system preference.

---

# Part 6 — Refactor Challenge (25 min)

You now have a working theme system.

## Your Task

Refactor:

- Remove ALL hardcoded colors
- Replace padding values with spacing tokens
- Add at least one additional design token (ex: borderRadius or typography)
- Make the List rows reuse the same “card” styles as the Home screen (same border radius, border color, and background).

---

# Reflection Questions (10 min)

Discuss:

- Why is theming better than inline styling?
- Where should theme state live?
- Should dark mode respect system or user choice?
- What happens in large teams?

---

# Final Project Requirement (New)

Your final project must:

- Use a centralized theme file
- Have no hardcoded colors in components
- Support either:
  - Dark mode toggle
  - OR system-based dark mode

Bonus:
- Persist theme preference using AsyncStorage

---

# Instructor Notes

This lesson reinforces:

- Context architecture
- State-driven UI
- Design systems thinking
- Separation of concerns
- Scalable app structure

Encourage students to think of theme as:

> A design API for their application.

---

# Optional Advanced Topics

If time allows:

- Dynamic style functions
- Typography tokens
- Styled-components pattern
- Design systems in production apps
- Theming with React Native Elements or NativeBase

---

# End of Lesson