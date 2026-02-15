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

---

## 🖼 Station A — Image Picker (Recommended)

### Reference docs

- Expo ImagePicker reference: https://docs.expo.dev/versions/latest/sdk/imagepicker/
- Expo tutorial: Use an image picker: https://docs.expo.dev/tutorial/image-picker/

### Step 1 — Install

```bash
npx expo install expo-image-picker
```

### Step 2 — Add iOS permission text (app.json)

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

### Code hints

```js
import { CameraView, useCameraPermissions } from 'expo-camera';

// Permission pattern
const [permission, requestPermission] = useCameraPermissions();
if (!permission?.granted) {
  return <Button title="Allow Camera" onPress={requestPermission} />;
}

// Capture (pseudo)
const photo = await cameraRef.current.takePictureAsync();
setUri(photo.uri);
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

### Code hints

```js
import * as Location from 'expo-location';

const { status } = await Location.requestForegroundPermissionsAsync();
if (status !== 'granted') {
  setError('Permission denied');
  return;
}

const pos = await Location.getCurrentPositionAsync({});
setCoords(pos.coords); // { latitude, longitude, ... }
```

Optional enhancement:

```js
const place = await Location.reverseGeocodeAsync({
  latitude: pos.coords.latitude,
  longitude: pos.coords.longitude,
});
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

### Code hints

```js
import * as Haptics from 'expo-haptics';

await Haptics.impactAsync(Haptics.ImpactFeedbackStyle.Medium);

await Haptics.notificationAsync(
  Haptics.NotificationFeedbackType.Success
);
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

### Code hints

**Share text**

```js
import { Share } from 'react-native';

await Share.share({ message: 'Hello from my app!' });
```

**Share a file**

```js
import * as FileSystem from 'expo-file-system';
import * as Sharing from 'expo-sharing';

const uri = FileSystem.documentDirectory + 'notes.txt';
await FileSystem.writeAsStringAsync(uri, 'Hello file!');
await Sharing.shareAsync(uri);
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

### Code hints

```js
import * as Clipboard from 'expo-clipboard';

await Clipboard.setStringAsync(text);

const pasted = await Clipboard.getStringAsync();
setText(pasted);
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

### Code hints

```js
import { Linking } from 'react-native';

const url = 'https://www.dominican.edu';

if (await Linking.canOpenURL(url)) {
  await Linking.openURL(url);
} else {
  setError('Cannot open URL');
}
```

Maps example (iOS):

```js
await Linking.openURL('maps://?q=coffee');
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

### Code hints

```js
import * as Notifications from 'expo-notifications';

const perm = await Notifications.requestPermissionsAsync();
if (!perm.granted) {
  setError('Permission denied');
  return;
}

await Notifications.scheduleNotificationAsync({
  content: { title: 'Hello', body: 'This is a local notification' },
  trigger: { seconds: 5 },
});
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
