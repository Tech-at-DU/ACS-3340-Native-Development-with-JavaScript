# ACS 3340 — In-Class Project 

📱 Device Playground (Native / Device Integration)

## 🎯 Objective

Today you will build a small **Device Playground** app that demonstrates **one native capability** from a mobile device.

By the end of class, you will:

- Integrate at least one device API
- Handle permissions correctly (if required)
- Show working output in your UI
- Explain when and why your feature would be useful

This project prepares you for the **Final Project requirement: Native / Device Integration**.

---

# ⏱ Schedule (2h 45m)

- **0:00–0:15** Setup + Overview
- **0:15–1:30** Build your feature (pair work)
- **1:30–2:15** 2-minute demos per group
- **2:15–2:45** Polish + short write-up

---

# 🚀 Setup

## 1️⃣ Create the project

```bash
npx create-expo-app@latest device-playground --template blank
cd device-playground
npx expo start
```

---

## 2️⃣ Install navigation

```bash
npm install @react-navigation/native @react-navigation/bottom-tabs
npx expo install react-native-screens react-native-safe-area-context
npm install @react-navigation/native-stack
```

## 3️⃣ `app.json` (New Concept)

Some device APIs require iOS permission text (the popup that asks the user for access).
In Expo projects, you often set this up in **`app.json`** using a **plugin**.

### When you need to edit `app.json`

- When an API needs iOS permission text (Photos, Camera, Location, etc.)
- When the library’s docs show a `plugins: [...]` config block

### After you edit `app.json`

1. **Restart Expo** so Metro reloads your config:
   ```bash
   npx expo start -c
   ```
2. Important: some `app.json` changes only fully apply when you build a native app (Development Build / EAS Build). Expo Go may work for quick testing, but don’t assume it’s “done” until you’ve built.

---

# 🏗 App Structure

Your app must have **two tabs** and a stack inside the first tab:

- **Playground (Tab 1):** a **Stack Navigator** with two screens: Explore → Feature
- **Notes (Tab 2):** your write-up (API used, permissions, use case, one challenge)

---

# 🧰 Starter Code (Scaffold)

---

## Suggested folder structure

```
.
├── App.js
├── PlaygroundStack.js
└── screens
    ├── ExploreScreen.js
    ├── FeatureScreen.js
    └── NotesScreen.js
```

## 1) `App.js`

```js
import * as React from 'react';
import { NavigationContainer } from '@react-navigation/native';
import { createBottomTabNavigator } from '@react-navigation/bottom-tabs';
import { Ionicons } from '@expo/vector-icons';

import PlaygroundStack from './PlaygroundStack';
import NotesScreen from './screens/NotesScreen';

const Tab = createBottomTabNavigator();

export default function App() {
  return (
    <NavigationContainer>
      <Tab.Navigator
        screenOptions={({ route }) => ({
          headerShown: false, // Stack will handle headers in the Playground tab
          tabBarIcon: ({ focused, color, size }) => {
            const icons = {
              Playground: focused ? 'compass' : 'compass-outline',
              Notes: focused ? 'document-text' : 'document-text-outline',
            };
            return <Ionicons name={icons[route.name]} size={size} color={color} />;
          },
          tabBarActiveTintColor: 'tomato',
          tabBarInactiveTintColor: 'gray',
        })}
      >
        <Tab.Screen name="Playground" component={PlaygroundStack} />
        <Tab.Screen name="Notes" component={NotesScreen} />
      </Tab.Navigator>
    </NavigationContainer>
  );
}
```

## 1b) `PlaygroundStack.js`

```js
import * as React from 'react';
import { createNativeStackNavigator } from '@react-navigation/native-stack';

import ExploreScreen from './screens/ExploreScreen';
import FeatureScreen from './screens/FeatureScreen';

const Stack = createNativeStackNavigator();

export default function PlaygroundStack() {
  return (
    <Stack.Navigator>
      <Stack.Screen
        name="Explore"
        component={ExploreScreen}
        options={{ title: 'Device Playground' }}
      />
      <Stack.Screen
        name="Feature"
        component={FeatureScreen}
        options={{ title: 'My Feature' }}
      />
    </Stack.Navigator>
  );
}
```

## 2) `screens/ExploreScreen.js`

```js
import * as React from 'react';
import { View, Text, StyleSheet, Pressable } from 'react-native';

const STATIONS = [
  'Image Picker',
  'Camera',
  'Location',
  'Haptics',
  'Share API',
  'Clipboard',
  'Linking',
  'Notifications',
];

export default function ExploreScreen({ navigation }) {
  const choose = (name) => {
    navigation.navigate('Feature', { station: name });
  };

  return (
    <View style={styles.container}>
      <Text style={styles.p}>
        Pick ONE station. You will build it on the next screen.
      </Text>

      <Text style={styles.h}>Stations</Text>

      {STATIONS.map((name) => (
        <Pressable key={name} onPress={() => choose(name)} style={styles.item}>
          <Text style={styles.itemText}>{name}</Text>
        </Pressable>
      ))}

      <Text style={styles.hint}>
        Tip: start with Image Picker, Haptics, Clipboard, or Linking.
      </Text>
    </View>
  );
}

const styles = StyleSheet.create({
  container: { flex: 1, padding: 16 },
  p: { fontSize: 16, marginBottom: 10, lineHeight: 22 },
  h: { marginTop: 10, marginBottom: 8, fontSize: 18, fontWeight: '700' },
  item: {
    borderWidth: 1,
    borderColor: '#ddd',
    borderRadius: 10,
    paddingVertical: 12,
    paddingHorizontal: 12,
    marginBottom: 10,
  },
  itemText: { fontSize: 16, fontWeight: '600' },
  hint: { marginTop: 12, color: '#555', lineHeight: 20 },
});
```

## 3) `screens/FeatureScreen.js`

```js
import * as React from 'react';
import { View, Text, Pressable, StyleSheet } from 'react-native';

export default function FeatureScreen({ route }) {
  const station = route?.params?.station ?? 'Pick a station in Explore';

  return (
    <View style={styles.container}>
      <Text style={styles.title}>My Feature</Text>
      <Text style={styles.p}>Selected station:</Text>
      <Text style={styles.station}>{station}</Text>

      <Text style={styles.p}>
        Implement ONE station on this screen. Don’t try to build all of them.
      </Text>

      <Text style={styles.h}>Starter plan</Text>
      <Text style={styles.p}>
        1) Install the station dependency{'\n'}
        2) Add state for output (image URI, coords, etc.){'\n'}
        3) Request permission (if needed){'\n'}
        4) Trigger the API with a button{'\n'}
        5) Render output or an error message
      </Text>

      <Pressable style={styles.button} onPress={() => {}}>
        <Text style={styles.buttonText}>Placeholder Button</Text>
      </Pressable>

      <Text style={styles.hint}>
        Replace the placeholder with your real feature UI.
      </Text>
    </View>
  );
}

const styles = StyleSheet.create({
  container: { flex: 1, padding: 16 },
  title: { fontSize: 28, fontWeight: '700', marginBottom: 12 },
  station: { fontSize: 20, fontWeight: '800', marginBottom: 12 },
  h: { marginTop: 10, marginBottom: 6, fontSize: 18, fontWeight: '700' },
  p: { fontSize: 16, marginBottom: 10, lineHeight: 22 },
  hint: { marginTop: 12, color: '#555', lineHeight: 20 },
  button: {
    marginTop: 10,
    backgroundColor: '#f4511e',
    paddingVertical: 12,
    paddingHorizontal: 14,
    borderRadius: 10,
    alignSelf: 'flex-start',
  },
  buttonText: { color: 'white', fontWeight: '700', fontSize: 16 },
});
```

## 4) `screens/NotesScreen.js`

```js
import * as React from 'react';
import { View, Text, StyleSheet } from 'react-native';

export default function NotesScreen() {
  return (
    <View style={styles.container}>
      <Text style={styles.title}>Notes</Text>

      <Text style={styles.h}>1) API used</Text>
      <Text style={styles.p}>TODO: (Image Picker / Camera / Location / ...)</Text>

      <Text style={styles.h}>2) Permissions</Text>
      <Text style={styles.p}>TODO: What permission did you request? What happens if denied?</Text>

      <Text style={styles.h}>3) Real-world use case</Text>
      <Text style={styles.p}>TODO: Why would a real app need this?</Text>

      <Text style={styles.h}>4) One challenge</Text>
      <Text style={styles.p}>TODO: What was confusing or tricky?</Text>
    </View>
  );
}

const styles = StyleSheet.create({
  container: { flex: 1, padding: 16 },
  title: { fontSize: 28, fontWeight: '700', marginBottom: 12 },
  h: { fontSize: 18, fontWeight: '700', marginTop: 16, marginBottom: 6 },
  p: { fontSize: 16, lineHeight: 22 },
});
```

## Quick reminder

You will still need to install the dependency for your chosen station (for example `expo-image-picker`).

---

# 🧪 Choose ONE Station

Pick **one** native capability below. Do not attempt multiple unless you finish early.

Recommended: **Image Picker, Haptics, Clipboard, Linking**  
Challenge: **Camera, Location, Share (file), Notifications**

If a station tells you to update `app.json`, do it **before** writing code for the station, then restart Expo with `npx expo start -c`.

---

## 🖼 Station A — Image Picker (Recommended)

### Reference docs

- Expo ImagePicker reference: https://docs.expo.dev/versions/latest/sdk/imagepicker/
- Expo tutorial: Use an image picker: https://docs.expo.dev/tutorial/image-picker/

### Step 1 — Install

```bash
npx expo install expo-image-picker
```

### Step 2 — Add iOS permission text (app.json plugin)

Add this to your `app.json`:

```json
{
  "expo": {
    "plugins": [
      [
        "expo-image-picker",
        {
          "photosPermission": "Allow access to your photos so you can choose an image."
        }
      ]
    ]
  }
}
```

Then restart Expo:

```bash
npx expo start -c
```

> If your permission popup text still looks wrong, that’s usually because you need a **development build** for the config plugin to apply fully (Expo Go can be inconsistent here).

### Step 3 — Implement in FeatureScreen

1. Import ImagePicker.
2. Add state for `imageUri`.
3. Request permission.
4. Launch the picker.
5. Render the image.

Here is a complete working example you can paste into `screens/FeatureScreen.js` while your selected station is **Image Picker**:

```js
import * as React from 'react';
import { View, Text, Pressable, StyleSheet, Image, Alert } from 'react-native';
import * as ImagePicker from 'expo-image-picker';

export default function FeatureScreen({ route }) {
  const station = route?.params?.station ?? 'Image Picker';
  const [imageUri, setImageUri] = React.useState(null);

  const pickImage = async () => {
    const perm = await ImagePicker.requestMediaLibraryPermissionsAsync();
    if (!perm.granted) {
      Alert.alert('Permission required', 'Please allow photo access to pick an image.');
      return;
    }

    const result = await ImagePicker.launchImageLibraryAsync({
      mediaTypes: ['images'],
      allowsEditing: true,
      quality: 0.8,
    });

    if (result.canceled) return;

    setImageUri(result.assets[0].uri);
  };

  return (
    <View style={styles.container}>
      <Text style={styles.title}>My Feature</Text>
      <Text style={styles.station}>{station}</Text>

      <Pressable style={styles.button} onPress={pickImage}>
        <Text style={styles.buttonText}>Pick Image</Text>
      </Pressable>

      {!imageUri ? (
        <Text style={styles.hint}>No image selected yet.</Text>
      ) : (
        <Image source={{ uri: imageUri }} style={styles.image} />
      )}
    </View>
  );
}

const styles = StyleSheet.create({
  container: { flex: 1, padding: 16 },
  title: { fontSize: 28, fontWeight: '700', marginBottom: 6 },
  station: { fontSize: 18, fontWeight: '700', marginBottom: 12 },
  hint: { marginTop: 12, color: '#555' },
  button: {
    backgroundColor: '#f4511e',
    paddingVertical: 12,
    paddingHorizontal: 14,
    borderRadius: 10,
    alignSelf: 'flex-start',
  },
  buttonText: { color: 'white', fontWeight: '700', fontSize: 16 },
  image: { marginTop: 12, width: '100%', height: 280, borderRadius: 12 },
});
```

### Checklist (if it doesn’t work)

- You installed the dependency with `npx expo install expo-image-picker`.
- You restarted Expo with `npx expo start -c` after editing `app.json`.
- You requested permission before launching the picker.
- You handled the “canceled” result without crashing.

---

## 📷 Station B — Camera

**Build:** Take a photo and display it.

### Reference docs

- Expo Camera reference: https://docs.expo.dev/versions/latest/sdk/camera/
- Expo Camera tutorial: https://docs.expo.dev/tutorial/camera/

### Step-by-step

1. Install `expo-camera`
2. Request camera permission
3. Render a camera preview
4. Capture a photo and store its `uri`
5. Display the captured image

### Install

```bash
npx expo install expo-camera
```

### `app.json` note (Camera permission text)

Add an iOS permission message using the `expo-camera` plugin. In `app.json`:

```json
{
  "expo": {
    "plugins": [
      [
        "expo-camera",
        {
          "cameraPermission": "Allow access to your camera so you can take a photo."
        }
      ]
    ]
  }
}
```

Then restart Expo:

```bash
npx expo start -c
```

### Implementation Plan (Pseudo Code)

```
STATE:
  photoUri = null
  permissionGranted = false

ON SCREEN LOAD:
  ask for camera permission
  if granted → permissionGranted = true
  else → show error + "Try Again" button

RENDER:
  if permissionGranted is false:
    show permission request UI

  else if photoUri is null:
    show camera preview
    show "Take Photo" button

    on button press:
      call takePictureAsync()
      store returned uri in photoUri

  else:
    show the captured image using photoUri
    show "Retake" button (optional)
```

> ⚠️ Works best on a physical device.

---

## 📍 Station C — Location

**Build:** Display current latitude and longitude.

### Reference docs

- Expo Location reference: https://docs.expo.dev/versions/latest/sdk/location/
- Expo Permissions guide: https://docs.expo.dev/guides/permissions/

### Step-by-step

1. Install `expo-location`
2. Request foreground location permission
3. Get current position
4. Render coordinates
5. Handle denied permission

### Install

```bash
npx expo install expo-location
```

### `app.json` note (Location permission text)

Add an iOS permission message using the `expo-location` plugin. In `app.json`:

```json
{
  "expo": {
    "plugins": [
      [
        "expo-location",
        {
          "locationAlwaysAndWhenInUsePermission": "Allow location access so we can show your current position."
        }
      ]
    ]
  }
}
```

Then restart Expo:

```bash
npx expo start -c
```

### Implementation Plan (Pseudo Code)

```
STATE:
  coords = null
  error = null

ON SCREEN LOAD:
  request foreground location permission
  if denied → set error and stop

  if granted:
    call getCurrentPositionAsync()
    store { latitude, longitude } in coords

RENDER:
  if error exists:
    show error message + "Try Again" button

  else if coords is null:
    show loading spinner + "Getting location..."

  else:
    display latitude and longitude
    optional: "Refresh Location" button
```

Optional enhancement (still pseudo-code):

```
call reverseGeocodeAsync(latitude, longitude)
display city/region
```

---

## 📳 Station D — Haptics

**Build:** Buttons that trigger vibration feedback.

### Reference docs

- Expo Haptics reference: https://docs.expo.dev/versions/latest/sdk/haptics/

### Step-by-step

1. Install `expo-haptics`
2. Create at least 3 buttons (success / warning / error)
3. Trigger haptic feedback on press
4. Add a sentence explaining when haptics improve UX

### Install

```bash
npx expo install expo-haptics
```

### `app.json` note

No `app.json` changes are needed for Haptics.

### Implementation Plan (Pseudo Code)

```
UI:
  Button: "Success"
  Button: "Warning"
  Button: "Error"

ON PRESS:
  call a haptics function for that type
  show a short message like "Haptic triggered"
```

---

## 🔗 Station E — Share API

**Build:** Share text (easy) or share a file (advanced).

### Reference docs

- React Native Share API: https://reactnative.dev/docs/share
- Expo Sharing (share a file): https://docs.expo.dev/versions/latest/sdk/sharing/
- Expo FileSystem (create a file): https://docs.expo.dev/versions/latest/sdk/filesystem/

### Step-by-step (choose one)

**Option A: Share text (simplest)**  
1. Call `Share.share({ message })` from a button press

**Option B: Share a file (more realistic)**  
1. Write a file to `FileSystem.documentDirectory`  
2. Call `Sharing.shareAsync(uri)`

### Install (for file sharing)

```bash
npx expo install expo-sharing expo-file-system
```

### `app.json` note

No `app.json` changes are needed to share text.  
If you share **files**, you still usually don’t need `app.json` changes, but you must create the file and share its URI.

### Implementation Plan (Pseudo Code)

```
OPTION A: SHARE TEXT
STATE:
  message = "Hello from my app!"

UI:
  TextInput for message
  Button: "Share"

ON PRESS:
  call Share.share({ message })

RENDER:
  show success/canceled message if available
```

```
OPTION B: SHARE A FILE (ADVANCED)
STATE:
  fileUri = null

UI:
  Button: "Create File"
  Button: "Share File"

ON "Create File":
  write a text file into the app document directory
  set fileUri

ON "Share File":
  if fileUri missing → show error
  else → call shareAsync(fileUri)
```

---

## 📋 Station F — Clipboard

**Build:** Copy and paste text.

### Reference docs

- Expo Clipboard reference: https://docs.expo.dev/versions/latest/sdk/clipboard/

### Step-by-step

1. Install `expo-clipboard`
2. Add a `TextInput` controlled by state
3. Copy: `Clipboard.setStringAsync(text)`
4. Paste: `Clipboard.getStringAsync()` → set into state
5. Show “Copied!” or “Pasted!” feedback

### Install

```bash
npx expo install expo-clipboard
```

### `app.json` note

No `app.json` changes are needed for Clipboard.

### Implementation Plan (Pseudo Code)

```
STATE:
  text = ""
  statusMessage = ""

UI:
  TextInput bound to text
  Button: "Copy"
  Button: "Paste"
  Text: statusMessage

ON "Copy":
  set clipboard to text
  statusMessage = "Copied!"

ON "Paste":
  read clipboard text
  set text to clipboard value
  statusMessage = "Pasted!"
```

---

## 🌐 Station G — Linking

**Build:** Open an external website or app.

### Reference docs

- React Native Linking: https://reactnative.dev/docs/linking

### Step-by-step

1. Choose a URL
2. Check `Linking.canOpenURL(url)`
3. Call `Linking.openURL(url)`
4. Render an error message if it fails

### `app.json` note

No `app.json` changes are needed for Linking.

### Implementation Plan (Pseudo Code)

```
STATE:
  error = null

UI:
  Button: "Open Website"
  Button: "Open Maps"
  Text: error (if any)

ON "Open Website":
  url = "https://www.dominican.edu"
  if canOpenURL(url) is false → set error
  else → openURL(url)

ON "Open Maps" (iOS):
  url = "maps://?q=coffee"
  openURL(url)
```

---

## 🔔 Station H — Notifications (Advanced)

**Build:** Schedule a local notification 5 seconds in the future.

### Reference docs

- Expo Notifications reference: https://docs.expo.dev/versions/latest/sdk/notifications/
- Notifications overview: https://docs.expo.dev/push-notifications/overview/

### Step-by-step

1. Install `expo-notifications`
2. Request permission
3. Schedule a notification with a 5-second trigger
4. Show “Scheduled!” state in the UI
5. Handle denied permission

### Install

```bash
npx expo install expo-notifications
```

### `app.json` note

Local notifications usually work without `app.json` changes.  
If you go further into **push notifications**, you will need additional configuration (out of scope today).

After installing the dependency, restart Expo:

```bash
npx expo start -c
```

### Implementation Plan (Pseudo Code)

```
STATE:
  permissionGranted = false
  statusMessage = ""

ON SCREEN LOAD:
  request notifications permission
  if granted → permissionGranted = true
  else → statusMessage = "Permission denied"

UI:
  Button: "Schedule Notification (5s)"
  Text: statusMessage

ON PRESS:
  if permissionGranted is false:
    statusMessage = "Enable notifications first"
    stop
  else:
    scheduleNotificationAsync(trigger in 5 seconds)
    statusMessage = "Scheduled!"
```

---

# 📦 Deliverables

At the end of class, you must have:

## ✅ 1. Working Feature Screen
- Feature works
- Permissions handled
- Error states handled

## ✅ 2. Notes Screen
Include:

- Which API you used
- What permissions were required
- One real-world use case
- One challenge you encountered

## ✅ 3. GitHub Repo
- At least **3 meaningful commits**
- Clean project structure

---

# 🧠 Reflection

Answer these in your Notes screen:

1. Why is this better as a mobile app feature than a web app feature?
2. What breaks if permission is denied?
3. How could this become part of your Final Project?

---

# 🏁 Evaluation Checklist

| Requirement | Complete? |
|-------------|-----------|
| Navigation with 2 tabs + stack | ☐ |
| Native API integrated | ☐ |
| Permission handled | ☐ |
| Error state present | ☐ |
| Working UI | ☐ |
| Notes screen complete | ☐ |

---

# 💡 Why This Matters

Your Final Project must integrate a native feature.

Today’s goal is to:

- Experiment safely
- Understand permissions
- Understand failure states
- Understand UX implications

This is where your final project idea should begin to take shape.

---
