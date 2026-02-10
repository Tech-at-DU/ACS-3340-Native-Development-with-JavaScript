# ACS 3340 - Navigation Part 2

<!-- > -->

A second look at navigation with Tab Navigator and Drawer Navigator. We will also look at how to display icons in your app. 

<!-- > -->

## Learning Objectives/Competencies

- Describe use case for Tab navigation
- Implement Tab navigation
- Combine navigation schemes

<!-- > -->

## Tabs and Drawers

<!-- > -->

Tabbed navigation appears is common on iOS and also appears on Android. Drawer navigation appears on Android and rarely on iOS. On iOS, the tab always appears at the bottom. 

<!-- > -->

There is a Tabbed starter project option when using `expo init`, I found this project code to be a little more complex than needed. I suspect most people would end up undoing more than they would use with this project. 

<!-- > -->

[Drawer Navigator Starter](https://reactnavigation.org/docs/en/drawer-based-navigation.html) (Note! See my notes below tl;dr this did not work for me)

I tested these both on my iOS device.

<!-- > -->

### Tab Navigator 

<!-- > -->

To get started with tabbed navigation you'll need to spin up a react native app along with the required dependancies. 

```
npx create-expo-app tabbed-example --template blank
```

<!-- > -->

Install dependancies:

```
npm install @react-navigation/native
```

```
npx expo install react-native-screens react-native-safe-area-context
```

```
npm install @react-navigation/bottom-tabs
```

```
npm install @react-navigation/native-stack
```

Test your app with `npm start`. 

If everything is working you should see the default Expo app and you will create some tabbed navigation in the next steps! 

<!-- > -->

The code below comes from the React Native Navigation docs [here](https://reactnavigation.org/docs/tab-based-navigation#minimal-example-of-tab-based-navigation).

<!-- > -->

The code below creates a bare bones application with two tabs. Notice this is very similar to the stack navigation code! 

Replace the code in `App.js` with the code below:

```JS
import * as React from 'react';
import { Text, View } from 'react-native';
import { NavigationContainer } from '@react-navigation/native';
import { createBottomTabNavigator } from '@react-navigation/bottom-tabs';

function HomeScreen() {
  return (
    <View style={{ flex: 1, justifyContent: 'center', alignItems: 'center' }}>
      <Text>Home!</Text>
    </View>
  );
}

function SettingsScreen() {
  return (
    <View style={{ flex: 1, justifyContent: 'center', alignItems: 'center' }}>
      <Text>Settings!</Text>
    </View>
  );
}

const Tab = createBottomTabNavigator();

export default function App() {
  return (
    <NavigationContainer>
      <Tab.Navigator>
        <Tab.Screen name="Home" component={HomeScreen} />
        <Tab.Screen name="Settings" component={SettingsScreen} />
      </Tab.Navigator>
    </NavigationContainer>
  );
}
```

**Tabbed apps** should use tabs to organize funcitonality with each tab acting as a smaller app inside the app as a whole. For example, one tab might be used for taking pictures, and another used for displaying a list of images. 

You'll never create navigation that moves between the tabs. That would defeat the purpose of tabs and create confusion. 

<!-- > -->

The code above uses three components: `App`, `HomeScreen`, and `SettingsScreen` in one file. Best practice is to keep all components in their own files. 

**Challenge:** 

Move `HomeScreen` and `SettingsScreen` into their own files. Be sure to import the required dependencies, export the component, and import these components for use in `App.js`. 

<!-- > -->

**Challenge:** 

Customize `HomeScreen` and `SettingsScreen`. This is an open ended challenge you can do as much as you like here. The goal is to modify the contents and styles of these screens. 

- Change the content
  - Modify the existing text
  - Add new text and or a view
- Change the style
  - Style the text
    - Set the `fontSize`, `color` etc.
  - Style any new content
  - Style the containing view

<!-- > -->

**Challenge:**

- Add another tab to the tab bar. Follow these steps: 
  - Make a new component screen. This can be a function that returns a `<View>` with some `<Text>`
  - Add a new `<Tab.Screen>` as a child of `<Tab.Navigator>`. You can put this below the existing tab screens. Assign it your new component. 

<!-- > -->

## Icons 

<!-- > -->

Icons are used in many places in mobile apps. With the small screen size a picture communicates a lot. Think about how emojis do the same in text messages. 

<!-- > -->

`react-native-vector-icons` is a native module Expo wraps and preloads it. 

In your React Native project import the icon with: 

```JS
import { Ionicons } from '@expo/vector-icons';
```

Use the icon with: 

```JS
<Ionicons name="add-circle-outline" size={32} color="black" />
```

This should display a 32pt icon that looks like a circle with a plus in the center.

You can put this component almost anywhere! You can place it inside of a `View` or inside of a `Text` component. 

<!-- > -->

**Explore Icons here:**

Look at the bundled icon sets: https://github.com/oblador/react-native-vector-icons#bundled-icon-sets.

Spend a few minutes exploring the iconsets and icons at the link above. 

**Note!** Above uses the name: `ios-add-circle-outline` which identifies and displays a specific icon. Finding the name to use for a specific icon is not as easy as you might think. The names shown with the icon bundles are not always what you need to set as the name in the component using React Native!

If you get a warning that the name isn't working for an icon open the warning and read the names! It will list all of the names that are possible values. 

<!-- > -->

**Challenges:**

- Make an icon that appears on the Home Screen
- Make an icon that appears on the Settings Screen 
- Set the and color of the icons
  - Use the `style` prop and set the `color`. The same as you would with a `Text` component.
- Make some icons in a Screen. You'll need to:
	- Import the icon set you want to use
	- Figure out the name of the icon you want to see (this might take some experimentation)
	- Add and configure an Icon component in your Screen
	
<!-- > -->

**Stretch Challenge:**

Add a button to the Home or Settings screens and include an icon with that button. 

- Make an Icon button. 
	- Follow the guide [here](https://github.com/oblador/react-native-vector-icons#iconbutton-component)

<!-- > -->

## Tab Bar Icons 

<!-- > -->

The docs show a solution in the [customizing the appearance](https://reactnavigation.org/docs/tab-based-navigation/#customizing-the-appearance) section of the React Navigation Tab Bar Example. This example looks a little complex as it uses the React-Native-Vector-Icons and includes a badge on the icon.

<!-- > -->

Notice that big block of code inside:

```
<Tab.Navigator screenOptions={...lots of code here...} >
```

This function returns an object that includes a property: `tabBarIcon`. This property is a function that receives an object with three properties: `focused`, `color`, `size`. You'll use these values to generate an icon and return it. 

<!-- > -->

The `route` identifies which tab the icon is for. We have an if else that defines the correct icon name for each route. 

<!-- > -->

The three properties: `focussed`, `color`, and `size` describe how the icon should be displayed. The `focussed` property is true when that tab is currently active. 

```JS
...
import { Ionicons } from 'react-native-vector-icons'
...
<Tab.Navigator
  screenOptions={({ route }) => ({
    tabBarIcon: ({ focused, color, size }) => {
      let iconName;

      if (route.name === 'Home') {
        iconName = focused
          ? 'ios-information-circle' // Set focused icon
          : 'ios-information-circle-outline'; // Set the not focused icon
      } else if (route.name === 'Settings') {
        iconName = focused ? 'ios-list' : 'ios-list';
      }

      // You can return any component that you like here!
      return <Ionicons name={iconName} size={size} color={color} />;
    },
    tabBarActiveTintColor: 'tomato', // Active/focussed color
    tabBarInactiveTintColor: 'gray' // Inactive color
  })}
>
...
</Tab.Navigator>
```

<!-- > -->

**Challenge:**

Add a custom icon for your new tab bar view. Find an icon you like. In the `tabBarIcon` function add a new else if block. This case should look for `route.name` to identify the icon. Look up an icon in the icon set. 

<!-- > -->

Note: Many icons have a solid and outline version. You can use the solid for the focussed state and the outline when the tab is not focussed. 

```JS
if (route.name === 'Home') {
  iconName = focused ? 'ios-information-circle' : 'ios-information-circle-outline';
} else if (route.name === 'Settings') {
  iconName = focused ? 'ios-list-box' : 'ios-list';
} else if (route.name === 'Other') {
  iconName = focused ? 'ios-star' : 'ios-star-outline';
}
```

<!-- > -->

Here I used the icons 'ios-star' and 'ios-star-outline' for the icon used in the 'Other' route.

<!-- > -->

### Stretch Challenges

<!-- > -->

- Import the breed data from the previous assignments. You can use the components from theose assignments also! 
	- Make a cats tab, display all the cat breeds in the cats tab, use FlatList.
		- Give this a cat icon
	- Make a dogs tab and display dog breeds in a list here.
		- Give this tab a Dog icon. 

<!-- > -->

### Tab Navigator Recap

<!-- > -->

Discussion: 

- Quickly describe how tabnavigator is constructed
- How are icons added? 
- What other options can be applied? 

Stretch challenges: 

- Add a stack navigator to one of the tabs screens

<!-- > -->

### Drawer Navigator

<!-- > -->

I had a hard time getting this to work. I suspect this was because I was working on iOS. 

The default sample code [here](https://reactnavigation.org/docs/en/drawer-based-navigation.html) did NOT work for me on iOS. 

What DID work for me was this example: 

- https://snack.expo.io/@aboutreact/navigationdrawer-example?session_id=snack-session-5vdX_nqe_

To get this working I did the following: 

- Download the sample code. This was incomplete and possible not using the latest version of React Native and React Navigator. 
- Make a new blank project with `expo init`. 
- Copy the following files from [example](https://snack.expo.io/@aboutreact/navigationdrawer-example?session_id=snack-session-5vdX_nqe_) and paste them into a new project you created in the previous step. 
    - App.js
    - app.json
    - assets
    - image
    - pages
    
The package.json from the example code is missing a few things. Don't copy this! You will need to add react-navigation

`npm install react-navigation`

This worked for me and generated a simple app with a drawer and three subpages. 

Please let me know if missed a step here. It was hard to backtrack over my experiments and note them here. 

## Apply navigation concepts

Apply the navigation concepts from class to your final project. Choose a navigation scheme for your project and create a minimal implementation to get started. Mockup any Screens you may need with a Text label naming the content that will eventually be there. 

Do this now in class! 

## After Class

- Continue working on the final project. Focus on building navigation and passing data to screens. 

## Additional Resources

- React Navigation
	- Tabs
		- https://reactnavigation.org/docs/en/tab-based-navigation.html
		- https://reactnavigation.org/docs/en/tab-based-navigation.html#minimal-example-of-tab-based-navigation
		- https://reactnavigation.org/docs/en/tab-based-navigation.html#customizing-the-appearance
		- https://reactnavigation.org/docs/en/bottom-tab-navigator.html#bottomtabnavigatorconfig
	- Vector Icons 
		- https://github.com/oblador/react-native-vector-icons
		- https://github.com/oblador/react-native-vector-icons#iconbutton-component
	- Drawers 
		- https://reactnavigation.org/docs/en/drawer-based-navigation.html
		- https://snack.expo.io/@aboutreact/navigationdrawer-example?session_id=snack-session-5vdX_nqe_

