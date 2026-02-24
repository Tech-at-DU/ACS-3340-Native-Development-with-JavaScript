# Session 1 — Architecture & Refactor Lab
## From Student App to Professional App

**Duration:** 2h 45m  
**Goal:** Refactor an existing screen to improve structure, separation of concerns, and clarity.

---

## 🎯 Learning Goals

By the end of this session, you will:

- Separate logic from presentation
- Extract reusable UI components
- Create a custom hook
- Remove side-effects from render
- Clean up file structure
- Reduce duplication
- Make your code easier to reason about

---

## Quick Intro — React Hooks

Hooks are functions that let your components “hook into” React features like **state**, **side effects**, and **context**. They exist to help you reuse logic and keep components readable.

You’ll use hooks for:

- **State** (`useState`) — values that change over time
- **Side effects** (`useEffect`) — fetch data, save data, subscribe/unsubscribe
- **Memoization** (`useMemo`, `useCallback`) — avoid expensive rework on re-render
- **Context** (`useContext`) — app-wide values like theme, auth, locale
- **Redux hooks** (`useSelector`, `useDispatch`) — read/store global state

### Rules of Hooks

1. **Only call hooks at the top level**  
   Don’t call hooks inside loops, conditions, or nested functions.

2. **Only call hooks from React functions**  
   Hooks must run inside a React function component or a custom hook.

3. **Name custom hooks with `use`**  
   Examples: `useAnimals`, `useTheme`, `useFavorites`.

### Best Practices

- **Use hooks to separate responsibilities**
  - Screens should compose UI.
  - Hooks should own logic (fetching, toggling, derived state).
  - Components should focus on rendering.

- **Keep effects narrow**
  - One effect = one responsibility (fetch OR persist OR subscribe).
  - Don’t combine unrelated side effects in the same `useEffect`.

- **Dependencies matter**
  - If you reference a variable in an effect, it usually belongs in the dependency array.
  - Prefer stable values and functions when possible.

- **Don’t store derived state**
  - If you can compute something from existing state/props, compute it instead of saving it.

- **Memoize intentionally**
  - `useMemo` / `useCallback` are for preventing real performance problems, not for “because it’s best practice.”
  - Start with readability. Optimize once you see a hotspot.

- **Custom hook test**
  - If a component has a big chunk of logic that could be reused or makes the JSX hard to read, extract it into a custom hook.

---

## Part 1 — What’s Wrong With This? (25 min)

Consider this realistic but messy screen:

```js
export default function AnimalListScreen() {
  const dispatch = useDispatch();
  const { animals, favorites } = useSelector(...);
  const theme = useContext(ThemeContext);

  const styles = createStyles(theme);

  useEffect(() => {
    dispatch(fetchAnimals());
  }, []);

  useEffect(() => {
    AsyncStorage.setItem('favorites', JSON.stringify(favorites));
  }, [favorites]);

  const toggleFavorite = (url) => {
    ...
  };

  return (
    <FlatList
      data={animals}
      renderItem={({ item }) => (
        <View style={{ padding: 16, backgroundColor: theme.colors.card }}>
          ...
        </View>
      )}
    />
  );
}
```

### Discussion Prompts

- What responsibilities does this screen have?
- What does it *know* about?
- What could be reused?
- What makes it hard to test?
- What makes it hard to change?

Write down your answers.

<details>
  <summary><strong>Reveal: What responsibilities does this screen have?</strong></summary>

You should notice this screen is responsible for:

- Data fetching
- Persistence
- Rendering
- Styling
- Business logic
- Async side effects
- Redux wiring

That’s too much.

</details>

---

## Part 2 — Architecture Pattern (20 min)

We will use this structure:

```
/screens
  AnimalListScreen.js

/components
  AnimalCard.js
  FavoriteButton.js

/hooks
  useAnimals.js
```

### Responsibilities

| Layer     | Responsibility       |
|----------|-----------------------|
| Screen   | Layout + composition  |
| Hook     | State + logic         |
| Component| Pure UI               |

---

## Part 3 — Guided Refactor (30 min)

### Step 1 — Extract a Custom Hook

#### What Is a Custom Hook?

A custom hook is a function that:

- Starts with `use`
- Calls other React hooks (like `useState`, `useEffect`, `useContext`, `useSelector`)
- Encapsulates reusable logic so components stay focused on UI

Custom hooks exist to **separate logic from presentation**.

- The **screen** should answer: *“What does this look like?”*
- The **hook** should answer: *“How does this work?”*

#### Why This Matters

As apps grow, screens often accumulate:

- Data fetching
- Derived state
- Business logic
- Redux wiring
- Async side effects

When that logic lives directly inside a component, it becomes harder to:
- Read
- Test
- Reuse
- Refactor

Extracting a custom hook keeps your screen lean and focused.

#### When Should You Extract a hook?

Extract a custom hook when:

- The top of your component feels crowded before the `return`
- You have multiple `useEffect` calls doing different things
- Logic could be reused in another screen
- You want to isolate and test behavior separately

A simple rule of thumb:

> If your component feels “busy” above the JSX, it probably wants a custom hook.

Create `/hooks/useAnimals.js`:

```js
import { useEffect } from 'react';
import { useDispatch, useSelector } from 'react-redux';
import { fetchAnimals, addFavorite, removeFavorite } from '../features/animals/animalsSlice';

export function useAnimals() {
  const dispatch = useDispatch();
  const { animals, favorites, status, error } = useSelector(
    (state) => state.animals
  );

  useEffect(() => {
    dispatch(fetchAnimals());
  }, [dispatch]);

  const toggleFavorite = (url) => {
    const isFav = favorites.includes(url);
    dispatch(isFav ? removeFavorite(url) : addFavorite(url));
  };

  const refresh = () => dispatch(fetchAnimals());

  return {
    animals,
    favorites,
    status,
    error,
    toggleFavorite,
    refresh,
  };
}
```

---

### Step 2 — Extract a Presentational Component

Create `/components/AnimalCard.js`:

```js
import { View, Text, Image, Pressable } from 'react-native';

export default function AnimalCard({
  imageUrl,
  isFavorite,
  onToggle,
  styles,
}) {
  return (
    <View style={styles.card}>
      <Image source={{ uri: imageUrl }} style={styles.image} />
      <View style={styles.row}>
        <Text style={styles.label}>
          {isFavorite ? '★ Favorite' : '☆ Not favorite'}
        </Text>
        <Pressable style={styles.button} onPress={() => onToggle(imageUrl)}>
          <Text style={styles.buttonText}>
            {isFavorite ? 'Remove' : 'Favorite'}
          </Text>
        </Pressable>
      </View>
    </View>
  );
}
```

Important:

- No Redux
- No AsyncStorage
- No network calls
- Pure UI only

---

### Step 3 — Simplify the Screen

```js
import * as React from 'react';
import { View, FlatList } from 'react-native';
import AnimalCard from '../components/AnimalCard';
import { ThemeContext } from '../ThemeContext';
import { createStyles } from '../styles/createStyles';
import { useAnimals } from '../hooks/useAnimals';

export default function AnimalListScreen() {
  const theme = React.useContext(ThemeContext);
  const styles = React.useMemo(() => createStyles(theme), [theme]);
  const { animals, favorites, status, refresh, toggleFavorite } = useAnimals();

  return (
    <View style={styles.container}>
      <FlatList
        data={animals}
        keyExtractor={(item) => item}
        contentContainerStyle={styles.listContent}
        refreshing={status === 'loading'}
        onRefresh={refresh}
        renderItem={({ item }) => (
          <AnimalCard
            imageUrl={item}
            isFavorite={favorites.includes(item)}
            onToggle={toggleFavorite}
            styles={styles}
          />
        )}
      />
    </View>
  );
}
```

Now the screen:

- Orchestrates
- Composes
- Stays readable

---

## Part 4 — Refactor Lab (50 min)

Now you will refactor one screen from your own project.

### Requirements

You must:

1. Extract at least one custom hook
2. Extract at least one presentational component
3. Remove inline styles from JSX
4. Reduce duplicated logic
5. Make your screen easier to read (less “busy” above `return`)

Create these folders if you don’t have them:

```
/hooks
/components
```

---

## Partner Review (20 min)

Pair up and review each other’s refactor.

You must explain:

- What logic you extracted
- Why it belongs in a hook
- What the screen is responsible for now
- What architectural smells remain

If you can’t explain it clearly, rethink your structure.

---

## Architectural Smells Checklist (15 min)

Does your screen:

- Exceed 250 lines?
- Mix `useEffect` and rendering logic heavily?
- Include lots of derived values computed inline inside JSX?
- Create big inline style objects in `style={...}`?
- Duplicate selectors or utility logic across files?
- Have multiple unrelated responsibilities?
- Have conditionals scattered everywhere in JSX?

If yes — refactor more.

---

## Deliverable (Due Next Class)

Submit:

- Refactored screen
- Extracted custom hook
- Extracted presentational component
- A short paragraph explaining what changed and why

---

## Grading Criteria

| Category                              | Points |
|--------------------------------------|--------|
| Hook extracted correctly             | 30     |
| Presentational component extracted   | 30     |
| Clear separation of concerns         | 20     |
| Reduced duplication                  | 10     |
| Code readability improved            | 10     |

---

## Why This Matters

This session moves you from:

> “Making it work”

to

> “Building it professionally.”

Architecture determines how easily your app grows, changes, and scales.

You are no longer writing demos — you are building real software.