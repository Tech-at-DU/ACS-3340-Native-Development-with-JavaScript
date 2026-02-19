# ACS 3340 — Theming in React Native

## 🎯 Learning Objectives

By the end of this lesson you will:

- Understand what “theming” means in a mobile application
- Build a reusable theme system using theme tokens
- Use a style factory function to keep JSX clean
- Introduce ThemeContext and use `useContext()` to read theme values in any screen
- Connect theme values to React Navigation UI
- (Bonus) Detect system dark mode with `useColorScheme()`

---

# ⏱ Class Plan (2:45)

| Time | Activity |
|------|----------|
| 0:00–0:15 | Why Theming Matters (Discussion + Demo) |
| 0:15–0:45 | Build the Sample App Scaffold (Tabs + UI Elements) |
| 0:45–1:15 | Build a Theme Object (Tokens) |
| 1:15–1:55 | Add Theme Context + useContext |
| 1:55–2:20 | Connect Theme to Navigation |
| 2:20–2:40 | Refactor Hardcoded Styles (Tokens + Reuse) |
| 2:40–2:45 | Reflection + Final Project Tie‑In |

---

# Part 0 — Build a Sample App Scaffold (30 min)

You will build a tiny app first. It is intentionally **ugly and hardcoded** so you can practice theming.

## What you will build

A 2-tab app:

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

If any of these fail, fix them before starting Part 1.

---

# Part 1 — Why Theming Matters (15 min)

Open the sample app you just built. Notice how many values are hardcoded and repeated across screens.

## Discussion Prompt

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

You are now going to refactor the **sample app scaffold** you built earlier (Home + List tabs).

This app is intentionally:
- Hardcoded
- Repetitive
- Not ready for dark mode

Your job is to fix that.

You are not building something new — you are improving what already exists.

---

## Step 1 — Audit the Demo App (5 min)

Open:

- `HomeScreen.js`
- `ListScreen.js`

Scan the styles.

Make a quick list (in comments or on paper):

- What colors repeat across both screens?
- What padding numbers repeat?
- Where do you see the same borderRadius value?
- Where do you see the same background color used multiple times?

Now discuss:

> If you had to support dark mode right now, how many lines would you have to change?

This is the problem theming solves.

---

## Step 2 — Design Your Theme Structure (10 min)

Create a file called `theme.js` at the root of your project.

Before writing values, decide on structure.

What categories does your app need?

Common categories:

- colors
- spacing
- radius
- typography (optional)

### Starter scaffold (you fill this in)

```js
// theme.js

export const lightTheme = {
  colors: {
    // background
    // text
    // primary
    // secondary
    // card
    // border
  },

  spacing: {
    // sm
    // md
    // lg
  },

  radius: {
    // sm
    // md
  }
};

export const darkTheme = {
  // Mirror the exact same structure as lightTheme
};
```

Important:

- Light and dark themes must have the **exact same structure**.
- Every repeated value in your demo app should become a token.
- Do not invent tokens you don’t use.

---

## Step 3 — Refactor the Home Screen (10 min)

### Use a Style Factory Function (Best Practice)

Instead of writing large inline style objects inside JSX, build your styles from the theme using a factory function.

```js
// styles.js (or inside HomeScreen.js)
import { StyleSheet } from 'react-native';

export const createStyles = (theme) =>
  StyleSheet.create({
    container: {
      flex: 1,
      backgroundColor: theme.colors.background,
      padding: theme.spacing.md,
    },
    card: {
      backgroundColor: theme.colors.card,
      borderRadius: theme.radius.md,
      padding: theme.spacing.md,
    },
    text: {
      color: theme.colors.text,
    },
    button: {
      backgroundColor: theme.colors.primary,
      padding: theme.spacing.sm,
      borderRadius: theme.radius.sm,
    }
  });
```

Then in `HomeScreen.js`:

```js
import { lightTheme } from '../theme';
import { createStyles } from './styles';

const theme = lightTheme;
const styles = createStyles(theme);
```

This keeps your JSX clean and keeps all visual decisions centralized.

---

## Step 4 — Refactor the List Screen (5 min)

Now refactor `ListScreen.js`.

Your goal:

- Make list items use the same `card` styling tokens as the Home screen.
- Make text colors consistent with your theme.
- Remove ALL repeated style literals.

You should start noticing that:

> Multiple screens are now sharing design decisions.

---

## Step 5 — Reflection (5 min)

Discuss:

- What improved?
- What still feels repetitive?
- Which tokens were most useful?
- What would happen if we changed one color in `theme.js`?

At this point, your theme file should be controlling the visual identity of your demo app.

That is the foundation of scalable design.

---

# Part 3 — Introduce ThemeContext (45 min)

So far, you imported `lightTheme` directly.

That works — but it doesn’t scale.

What if:
- You want to toggle dark mode?
- You want different themes per user?
- You want to avoid importing the theme in every file?

This is where React Context helps.

---

## What is useContext? (Concept Primer)

`useContext` is a React hook that allows components to access shared values without passing props through multiple levels of the component tree.

Normally in React, data flows like this:

Parent → Child → Grandchild → Great‑grandchild

If many components need the same value (like a theme), you would have to “prop drill” that value through every level.

Context solves this by creating a shared channel.

When should you use Context?

Use Context for:

- Theme or design system values
- Authentication state
- Global user preferences
- Locale / language
- App‑wide configuration

These are values that:
- Change rarely
- Are needed in many places
- Represent global app state

When should you NOT use Context?

Do NOT use Context for:

- Frequently changing UI state (like text input values)
- Local component state
- Highly dynamic data lists
- Large, complex async state (Redux is better for that)

Context is best for stable, global values — not as a replacement for Redux.

In this lesson, we are using Context specifically for theming, which is exactly the kind of problem Context was designed to solve.

---

## Step 1 — Create a ThemeContext

Create a new file: `ThemeContext.js`

```js
import React from 'react';
import { lightTheme } from './theme';

export const ThemeContext = React.createContext(lightTheme);
```

---

## Step 2 — Wrap Your App with the Provider

In `App.js`:

```js
import { ThemeContext } from './ThemeContext';
import { lightTheme } from './theme';

<ThemeContext.Provider value={lightTheme}>
  <YourNavigation />
</ThemeContext.Provider>
```

---

## Step 3 — Use useContext in a Screen

Earlier in this lesson, you did this:

```js
import { lightTheme } from '../theme';
const theme = lightTheme;
const styles = createStyles(theme);
```

That tightly couples your screen to a specific theme file.

Now we replace that pattern with Context.

```js
import React from 'react';
import { View, Text } from 'react-native';
import { ThemeContext } from '../ThemeContext';
import { createStyles } from './styles';

export default function HomeScreen() {
  const theme = React.useContext(ThemeContext);
  const styles = createStyles(theme);

  return (
    <View style={styles.container}>
      <Text style={styles.text}>Home Screen</Text>
    </View>
  );
}
```

Now the screen does NOT import lightTheme or darkTheme.
It simply reads whatever theme the Provider supplies.

When the Provider changes, the entire UI updates automatically.

---

## Step 4 — Why This Matters

With Context:

- You remove direct imports of theme files.
- You centralize theme decisions.
- You prepare for dark mode toggling.
- You make your app scalable.

This is how professional apps implement theming systems.

---

## Stretch Goal (Optional)

Add a state toggle in `App.js`:

```js
const [mode, setMode] = useState('light');
const theme = mode === 'light' ? lightTheme : darkTheme;
```

Pass `theme` into the Provider and add a button to toggle modes.

Watch your entire app change.

That is the power of a theme system.

---

# Part 4 — Connect Theme to Navigation (25 min)

React Navigation has its own theme shape. You can map your theme tokens into NavigationContainer so headers and tab bars match.

In your navigation root (where NavigationContainer lives):

```js
import { NavigationContainer } from '@react-navigation/native';

<NavigationContainer
  theme={{
    dark: false, // update later
    colors: {
      primary: theme.colors.primary,
      background: theme.colors.background,
      card: theme.colors.card,
      text: theme.colors.text,
      border: theme.colors.border ?? '#ccc',
      notification: theme.colors.primary,
    },
  }}
>
  <TabNavigator />
</NavigationContainer>
```

Activity:
- Make the header and tab bar match your theme.
- Confirm text remains readable in both modes.

---

# Part 5 — Refactor Challenge (20 min)

Refactor to reduce duplication:

- Remove ALL hardcoded colors.
- Replace padding values with spacing tokens.
- Add one extra token group (typography OR shadow OR border widths).
- Make the List rows reuse the same “card” styles as the Home screen (same border radius, border color, and background).

---

# Reflection (5 min)

- What got easier after tokens?
- What got easier after Context?
- What UI broke in dark mode, and why?
- Where should theme state live?

---

# Bonus — Detect System Dark Mode with `useColorScheme()`

Manual toggles are useful for learning, but real apps often follow the system setting.

React Native includes a built‑in hook:

```js
import { useColorScheme } from 'react-native';
```

It returns `'light'`, `'dark'`, or `null`, and updates automatically when the system theme changes.

## Use system theme in `App.js`

Instead of keeping a `mode` state, you can do:

```js
import React from 'react';
import { useColorScheme } from 'react-native';
import { ThemeContext } from './ThemeContext';
import { lightTheme, darkTheme } from './theme';

export default function App() {
  const scheme = useColorScheme();
  const theme = scheme === 'dark' ? darkTheme : lightTheme;

  return (
    <ThemeContext.Provider value={theme}>
      <YourNavigation />
    </ThemeContext.Provider>
  );
}
```

Now your entire app automatically respects system dark mode.

## Turn on dark mode in the iOS Simulator

1. Open the Simulator
2. Top menu: **Features → Appearance → Dark**
3. Your app should update immediately

If it doesn’t:
- reload the app, or
- restart Expo with `npx expo start -c`

## Optional Challenge: system default + manual override

Support BOTH:
- system theme (default)
- manual override stored in state (and optionally persisted)

Logic:
- if the user chose a theme, use it
- otherwise fall back to `useColorScheme()`
