# ACS 3340 - Using Flex

<!-- > -->

From here until the end of the term you will be working on your final project. Your goal is to define what the project is and plan how you will complete it between now and the end of the term. 

<!-- > -->

## Learning Objectives

- Apply Styles to components 
- Explore Native Components and read documentation apply what you find
- Define project goals
- Identify the platform
- Map out milestones

<!-- > -->

## Styling Components

Components in React Native are styled using inline styles. 

```JavaScript
...
return <View style={{width: 100}}>...</View>
...
```

<!-- > -->

Use `StyleSheet.create()` for some reason not clearly explained in the docs. The `StyleSheet` also has some helper functions. 

```JavaScript
...
return <View style={styles.container}></View>
...
import { StyleSheet }
const styles = StyleSheet.create({
 container: {
 width: 100
 }
})
```

<!-- > -->

**Important!** React Native uses CSS styles but there are a few differences between React Native and the Web. 

- Does not support all styles 
- Not all components support all styles 
- All units are pixels/points (with a few expectations)

<!-- > -->

### Flex Box

With React Native you will be using Flex to manage the layout of elements. Here is what the React-Native docs say: 

> Flexbox works the same way in React Native as it does in CSS on the web, with a few exceptions. The defaults are different, with flexDirection defaulting to column instead of row, alignContent defaulting to flex-start instead of stretch, flexShrink defaulting to 0 instead of 1, and the flex parameter only supporting a single number.

Take a look at this page and dsicuss: 

https://reactnative.dev/docs/flexbox

<!-- > -->

Everything is styled with Flex. The following properties will take your layouts far. 

- flex
- justifyContent
- alignItems 

Keep in mind that Flexbox applies to children. While Flexbox applies to a single axis you can mix axis by nesting elements in a view. 

**Flex** Is a property in CSS that breaks down into three properties grow, shrink, and basis. Only flex growth is supported. In a flex container, flex items take up a fraction of the space defined by their flex value. 

For example, imagine three flex items with flex 1, 2, and 3. The total is 6 so the first element fills 1/6, the second fill 1/3, and the last fills 1/2 of the available space. 

A pattern in React Native is to give an element a flex of 1 to make it fill the space. This works because a single element with a flex of 1 will take up 1/1 of the available space. 

<!-- > -->

Most of the standard flex properties work mostly the way that you expect them to work! Check out the list of style props here: 

https://reactnative.dev/docs/layout-props

You can create a grid-based layout with `flexWrap`. 

Divide space proportionally using `flex`. 

Create horizontal or vertical layouts with `flexDirection`. 

Sometimes you may need to know the size of the window. This can be useful when you want to fill the available space. Use `useWindowDimensions`: https://reactnative.dev/docs/usewindowdimensions

<!-- > -->

Arrange a group of "boxes" with flex. 

```JS
import { View, Text, StyleSheet, ScrollView } from 'react-native'

function Colors() {
  return (
    <View style={styles.container}> // flex container
      <View style={styles.box}></View> // Some boxes 100 x 100
      <View style={styles.box}></View>
      <View style={styles.box}></View>
      <View style={styles.box}></View>
      <View style={styles.box}></View>
      <View style={styles.box}></View>
      <View style={styles.box}></View>
    </View>
  )
}

export default Colors

const styles = StyleSheet.create({
  container: {
    display: 'flex', // make this a flex container
    flexDirection: 'row', // set direction to row
    flexWrap: 'wrap', // wrap boxes
    gap: 5, // space between rows and columns
    padding: 5, // add some space around the outside
    backgroundColor: 'lightgray'
  },
  box: {
    backgroundColor: 'red',
    width: 100, // each box is 100
    height: 100 // by 100
  }
})
```

This gives us something like this: 

<img src="./images/flex-example-1.png" style="width: 200px; border: 1px solid"/>

You can use other properties of Flex to make objects take up more of less space. 

Use `flexBasis` like width. Replacing `width: 100` with `flexBasis: 100`. This looks the same!

`flexBasis` determines the size of the flex item on the main axis. In this case it applies to width becuase the direction is row.

Flex items can grow and shrink. This determines what the flex container does with the left over space. In example image you can see there is extra space on the right, the boxes wrap when there isn't enough space left on the right. 

Add `flexGrow: 1` now the boxes fill out each row. The flex container takes the extra space and divides it between each of the boxes in a row! The app might look like this now: 

<img src="./images/flex-example-2.png" style="width: 200px; border: 1px solid"/>

Read about this in the docs: 

https://reactnative.dev/docs/flexbox#flex-basis-grow-and-shrink

<!-- > --> 

Use `ScrollView` to scroll content. Change the height of boxes to `300` and change `flexBasis: 180`. This should give you boxes that run off the bottom of the screen in two columns. Imagine the size of the screen is 393px, two boxes is 360. Three boxes would be 540, so the boxes wrap at 2 and the extra space is shared. The height is 852, four boxes at 300 height is 1200 which runs off the bottom of the screen. 

Wrap everything in a `ScrollView`:

```JS
<ScrollView>
  <View style={styles.container}>
    <View style={styles.box}></View>
    <View style={styles.box}></View>
    <View style={styles.box}></View>
    <View style={styles.box}></View>
    <View style={styles.box}></View>
    <View style={styles.box}></View>
    <View style={styles.box}></View>
  </View>
</ScrollView>
```

Everything should now scroll allowing access to the boxes that fall off the bottom of the screen. 

Read more about Flex here: https://www.joshwcomeau.com/css/interactive-guide-to-flexbox/

<!-- > -->

## Handling Input 

<!-- > -->

Touchscreen devices have their input paradigms. Touch screen interaction is a very different experience from mouse driven interaction. Screens are small and the pointing device, your finger, is really large. This is the opposite of desktop computers, where screens are large and the pointing device is very small and much more accurate. 

<!-- > -->

Take a quick look at the HIG and study the best practices for handling inputs on touchscreen devices: 

https://developer.apple.com/design/human-interface-guidelines/inputs/touchscreen-gestures

<!-- > -->

React Native provides a few interactive components. 

- Button - Good for basic button
- Pressable
- TextInput

<!-- > -->

Use the 'Pressable' components to create custom buttons and things you can tap to handle input. 

Imagine the boxes in the previous example needed to be "pressable". That is, something would happen when they are tapped. To do this, it woudl be best to have another component. 

Replace the box `View`s with a `Box` component: 

```JS
import { View, Text, Pressable, StyleSheet } from 'react-native'

function Box() {
  return (
    <Pressable 
      style={styles.container}
      onPress={() => console.log("Tap!")}  
    >
      <Text>Tap Me</Text>
    </Pressable>
  )
}

export default Box

const styles = StyleSheet.create({
  container: {
    backgroundColor: 'red',
    flexBasis: 180,
    flexGrow: 1,
    height: 300
  }
})
```

In the `Colors` component add replace the `View`s with `Box`es:

```JS
function Colors() {
  return (
    <ScrollView>
      <View style={styles.container}>
        {/* <View style={styles.box}></View>
        <View style={styles.box}></View>
        <View style={styles.box}></View>
        <View style={styles.box}></View>
        <View style={styles.box}></View>
        <View style={styles.box}></View>
        <View style={styles.box}></View> */}
        <Box />
        <Box />
        <Box />
        <Box />
        <Box />
        <Box />
        <Box />
      </View>
    </ScrollView>
  )
}
```

**Challenge!** use flex to center the `Text` in each `Box` inside the `Pressable`!

-
-
-
-
-
-
-
-
-
-
-
-
-
-
-
-
-
-
-
-
-

In the Box styles, in the `container` property add: 

```JS
justifyContent: 'center',
alignItems: 'center'
```

This centers the children on the main axis, vertical, and cross axis, horizontal. 

You can pass props to these boxes to get more functionality, here the example is kept simple. 

Providing user feedback can be handled like this: 

```JS
<Pressable 
  style={({pressed}) => [
    styles.container, 
    {
      backgroundColor: pressed ? 'green' : styles.container.backgroundColor
    }
  ]}
  onPress={() => console.log("Tap!")}  
>
  <Text>Tap Me</Text>
</Pressable>
```

Here, `style` takes a function that recieves an object with a `pressed` property. This true, if the pressable is pressed. This function returns an array of style objects. The second object in this array is overriding the `backgroundColor` to change the color when pressed is true! 

https://reactnative.dev/docs/pressable

If you plan on using more complex gestures you should read more about gestures and how they work: 

- https://developer.apple.com/design/human-interface-guidelines/inputs/touchscreen-gestures
- https://reactnative.dev/docs/gesture-responder-system

<!-- > -->

## Images 

React Native provides two ways to work with images. 

The Image component can be used to display images. Use this when an image is a content element. 

https://reactnative.dev/docs/image

The ImageBackground component can be used when an image is a background element. The ImageBackground allows other elements as children. 

https://reactnative.dev/docs/imagebackground

<!-- > -->

## After Class

Define your final project. This must be a native app of some kind. 

## Defining the final 

What are you going to make? 

What platform will it use? 

- Mobile
- Desktop

Define milestones for the project. A milestone is a step in the construction of your project and should have a deliverable.

## Additional Resources

- [Controlled Component pattern](https://reactjs.org/docs/forms.html) 
- [InputAccessoryView]( https://facebook.github.io/react-native/docs/inputaccessoryview) - Customizes keyboard input view
- [Picker](https://facebook.github.io/react-native/docs/picker) - Handles multi-choice input with a scrolling list of choices. Good for many choices.
- [PickerIOS](https://facebook.github.io/react-native/docs/pickerios) - iOS Picker View
- [SegmentedConreolIOS](https://facebook.github.io/react-native/docs/segmentedcontrolios) - Multi-choice input, iOS only, good for a few choices. 
- [Slider](https://facebook.github.io/react-native/docs/slider)
- [Switch](https://facebook.github.io/react-native/docs/switch) - Like a checkbox
- [TextInput](https://facebook.github.io/react-native/docs/textinput) - Use for Single line and multi-line text input 
