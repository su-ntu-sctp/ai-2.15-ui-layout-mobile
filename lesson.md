# Lesson 2.15: UI Composition and Layout Management in Mobile Applications

## Overview

- **Duration:** ~2 hours (hands-on lab)
- **Prerequisites:** Lesson 2.14 (Expo environment set up, Expo Go working on your device or emulator, basic familiarity with `View`, `Text`, and `StyleSheet`)

## Learning Objectives

By the end of this lesson, you will be able to:

1. **Use** the core React Native components (`View`, `Text`, `Image`, `TextInput`, `ScrollView`, and `KeyboardAvoidingView`) to compose a functional mobile screen
2. **Apply** React Native styling with `StyleSheet.create()`, understanding where it differs from CSS
3. **Design** mobile layouts using Flexbox, controlling axis direction, alignment, and proportional sizing

---

## Setup: Create the Project

This lesson uses a fresh Expo app. Open a terminal and run:

```bash
npx create-expo-app --template blank CompAndLayoutApp
cd CompAndLayoutApp
npx expo start
```

Open the emulator or scan the QR code with Expo Go. Confirm the default screen loads.

> This app is separate from your Lesson 2.14 counter app. It is a sandbox for exploring components and layout, so keeping it simple and focused is intentional.

Open `App.js` and replace its contents with the following starting point:

```jsx
import { StyleSheet, Text, View } from 'react-native';

export default function App() {
  return (
    <View style={styles.container}>
      <Text style={styles.header}>Components and Layout App</Text>
    </View>
  );
}

const styles = StyleSheet.create({
  container: {
    flex: 1,
    backgroundColor: '#fff',
    alignItems: 'center',
    justifyContent: 'center',
  },
  header: {
    fontSize: 20,
    fontWeight: '700',
    color: 'darkblue',
  },
});
```

**Device check:** you should see "Components and Layout App" centred on screen.

---

## Part 1: Core Components and Styling

### Why no HTML?

In Lesson 2.14 you learned that React Native does not use a browser, so there is no DOM and no HTML elements. Instead, React Native provides its own set of primitive components that compile down to native UI on each platform:

| React Native | iOS native | Android native | Web equivalent |
|---|---|---|---|
| `View` | `UIView` | `ViewGroup` | `<div>` |
| `Text` | `UITextView` | `TextView` | `<p>`, `<span>` |
| `Image` | `UIImageView` | `ImageView` | `<img>` |

Two rules that will save you from common errors:

1. **All text must be inside a `Text` component.** Placing a raw string directly inside a `View` throws an error.
2. **There are no heading tags.** There is no `<h1>` or `<h2>` in React Native. Typography hierarchy is achieved entirely through styling.

### Styling in React Native

React Native does not use CSS files or the `className` prop. All styling is written in JavaScript. Three things to remember:

- Property names are **camelCase**: `backgroundColor`, not `background-color`
- Numbers have **no units**: `fontSize: 20` means 20 density-independent pixels
- Some CSS properties **do not exist**: `border` shorthand and `boxShadow` are not supported. Use `borderWidth`/`borderColor` and the platform-specific shadow properties instead

**Inline style objects** work for quick, one-off styles:

```jsx
<Text style={{ fontSize: 24, fontWeight: '700' }}>Hello</Text>
```

**`StyleSheet.create()`** is preferred for most cases because it validates your style properties during development and keeps styles separated from JSX. There is no visual difference between the two approaches.

> **Common mistake:** Writing `border: '1px solid #ccc'` as you would in CSS. React Native requires separate properties: `borderWidth: 1` and `borderColor: '#ccc'`. To style only one side, use `borderBottomWidth` and `borderBottomColor`.

### Step 1: Add a sub-header style

Update `App.js` to add a sub-header text and a new style:

```jsx
import { StyleSheet, Text, View } from 'react-native';

export default function App() {
  return (
    <View style={styles.container}>
      <Text style={styles.header}>Components and Layout App</Text>
      <Text style={styles.subHeader}>Hello World!</Text>
    </View>
  );
}

const styles = StyleSheet.create({
  container: {
    flex: 1,
    backgroundColor: '#fff',
    alignItems: 'center',
    justifyContent: 'center',
  },
  header: {
    fontSize: 20,
    fontWeight: '700',
    color: 'darkblue',
    borderBottomWidth: 1,
    borderBottomColor: 'darkblue',
    marginBottom: 20,
    paddingBottom: 5,
  },
  subHeader: {
    fontSize: 18,
    fontWeight: '600',
    color: 'darkblue',
    marginBottom: 10,
  },
});
```

**Device check:** the header now has an underline and the sub-header appears below it.

---

## Part 2: The `Image` Component

The `Image` component displays images from two sources: local files bundled with the app, and remote URLs.

### Step 1: Add a local image

Download or copy an image file (your instructor will provide one) and place it in `assets/images/`. Then import and display it:

```jsx
import { StyleSheet, Text, View, Image } from 'react-native';
import monkeyImg from './assets/images/monkey.png';

export default function App() {
  return (
    <View style={styles.container}>
      <Text style={styles.header}>Components and Layout App</Text>
      <Text style={styles.subHeader}>Local Image</Text>
      <Image source={monkeyImg} style={styles.localImage} />
    </View>
  );
}

const styles = StyleSheet.create({
  container: {
    flex: 1,
    backgroundColor: '#fff',
    alignItems: 'center',
    justifyContent: 'center',
  },
  header: {
    fontSize: 20,
    fontWeight: '700',
    color: 'darkblue',
    borderBottomWidth: 1,
    borderBottomColor: 'darkblue',
    marginBottom: 20,
    paddingBottom: 5,
  },
  subHeader: {
    fontSize: 18,
    fontWeight: '600',
    color: 'darkblue',
    marginBottom: 10,
  },
  localImage: {
    width: 200,
    height: 200,
    borderWidth: 2,
    borderColor: '#eee',
    borderRadius: 10,
    marginBottom: 20,
  },
});
```

**Device check:** the image appears with a rounded border.

### Step 2: Add a network image

Network images require an explicit `width` and `height` because React Native cannot determine their dimensions ahead of time.

Add the image URL and a second `Image` component below the local one:

```jsx
const imageUrl =
  'https://upload.wikimedia.org/wikipedia/commons/thumb/1/18/Dog_Breeds.jpg/320px-Dog_Breeds.jpg';
```

```jsx
<Text style={styles.subHeader}>Remote Image</Text>
<Image
  source={{ uri: imageUrl }}
  style={styles.networkImage}
/>
```

And the style:

```jsx
networkImage: {
  width: 200,
  height: 200,
  borderWidth: 2,
  borderColor: '#eee',
  borderRadius: 10,
  marginBottom: 20,
},
```

**Device check:** both images appear on screen.

> **Common mistake:** Using `source={imageUrl}` instead of `source={{ uri: imageUrl }}`. Network images require the object form with a `uri` key.

> **Why does nothing appear without a size?** React Native needs to know the rendered dimensions before it can fetch and display a network image. Without `width` and `height` (or a `flex` value that gives the component a size), the image occupies zero space and is invisible.

---

## Part 3: `TextInput`, `ScrollView`, and `KeyboardAvoidingView`

### Step 1: Add a `TextInput`

`TextInput` is React Native's equivalent of `<input type="text">`. It is a controlled component: you manage its value with `useState`.

Update your imports and add state and a `TextInput` to the screen:

```jsx
import { StyleSheet, Text, View, Image, TextInput } from 'react-native';
import { useState } from 'react';
import monkeyImg from './assets/images/monkey.png';

const imageUrl =
  'https://upload.wikimedia.org/wikipedia/commons/thumb/1/18/Dog_Breeds.jpg/320px-Dog_Breeds.jpg';

export default function App() {
  const [text, setText] = useState('');

  return (
    <View style={styles.container}>
      <Text style={styles.header}>Components and Layout App</Text>
      <Text style={styles.subHeader}>Local Image</Text>
      <Image source={monkeyImg} style={styles.localImage} />
      <Text style={styles.subHeader}>Remote Image</Text>
      <Image source={{ uri: imageUrl }} style={styles.networkImage} />
      <TextInput
        style={styles.textInput}
        value={text}
        onChangeText={setText}
        placeholder="Type here..."
        autoCorrect={false}
        autoComplete="off"
      />
    </View>
  );
}
```

Add the `textInput` style:

```jsx
textInput: {
  height: 40,
  width: '70%',
  borderWidth: 1,
  borderRadius: 10,
  borderColor: '#333',
  padding: 10,
  margin: 12,
},
```

**Device check:** a text input appears below the images.

You will now notice two problems:

1. The content at the top of the screen is hidden behind the device notch and status bar
2. On iOS, the software keyboard covers the input field when you tap on it

You will fix both of these in the next steps.

### Step 2: Fix the notch with `SafeAreaView`

A hardcoded `marginTop` is not reliable because different devices have notches and status bars of different sizes. The correct solution is `react-native-safe-area-context`, which measures the safe area on any device automatically.

Install it:

```bash
npx expo install react-native-safe-area-context
```

> **Why `npx expo install` and not `npm install`?** Expo manages native library versions to match the installed Expo SDK. Using `npx expo install` picks the correct version for your project automatically.

Update your imports and wrap the component tree in `SafeAreaProvider` and `SafeAreaView`:

```jsx
import { StyleSheet, Text, View, Image, TextInput } from 'react-native';
import { useState } from 'react';
import { SafeAreaProvider, SafeAreaView } from 'react-native-safe-area-context';
import monkeyImg from './assets/images/monkey.png';

const imageUrl =
  'https://upload.wikimedia.org/wikipedia/commons/thumb/1/18/Dog_Breeds.jpg/320px-Dog_Breeds.jpg';

export default function App() {
  const [text, setText] = useState('');

  return (
    <SafeAreaProvider>
      <SafeAreaView style={{ flex: 1, backgroundColor: '#fff' }}>
        <View style={styles.container}>
          <Text style={styles.header}>Components and Layout App</Text>
          <Text style={styles.subHeader}>Local Image</Text>
          <Image source={monkeyImg} style={styles.localImage} />
          <Text style={styles.subHeader}>Remote Image</Text>
          <Image source={{ uri: imageUrl }} style={styles.networkImage} />
          <TextInput
            style={styles.textInput}
            value={text}
            onChangeText={setText}
            placeholder="Type here..."
            autoCorrect={false}
            autoComplete="off"
          />
        </View>
      </SafeAreaView>
    </SafeAreaProvider>
  );
}
```

Also remove `justifyContent: 'center'` from the `container` style. With `SafeAreaView` in place, you want content to flow from the top of the safe area, not be centred vertically.

```jsx
container: {
  flex: 1,
  backgroundColor: '#fff',
  alignItems: 'center',
},
```

**Device check:** content is no longer hidden behind the notch.

> `SafeAreaProvider` sits at the root of the tree and calculates the safe-area insets for the device. `SafeAreaView` is a `View` that automatically applies those insets as padding. The two components must always be used together.

### Step 3: Fix the keyboard and enable scrolling

Wrapping content in `ScrollView` makes the page scrollable when content overflows the screen. `KeyboardAvoidingView` shifts or resizes the layout when the software keyboard appears, preventing it from covering the input.

The correct nesting order is:

```
SafeAreaProvider
  SafeAreaView
    KeyboardAvoidingView
      ScrollView
        View  (your content)
```

Update your imports and component tree:

```jsx
import {
  StyleSheet,
  Text,
  View,
  Image,
  TextInput,
  ScrollView,
  KeyboardAvoidingView,
} from 'react-native';
import { useState } from 'react';
import { SafeAreaProvider, SafeAreaView } from 'react-native-safe-area-context';
import monkeyImg from './assets/images/monkey.png';

const imageUrl =
  'https://upload.wikimedia.org/wikipedia/commons/thumb/1/18/Dog_Breeds.jpg/320px-Dog_Breeds.jpg';

export default function App() {
  const [text, setText] = useState('');

  return (
    <SafeAreaProvider>
      <SafeAreaView style={{ flex: 1, backgroundColor: '#fff' }}>
        <KeyboardAvoidingView behavior="padding" style={{ flex: 1 }}>
          <ScrollView>
            <View style={styles.container}>
              <Text style={styles.header}>Components and Layout App</Text>
              <Text style={styles.subHeader}>Local Image</Text>
              <Image source={monkeyImg} style={styles.localImage} />
              <Text style={styles.subHeader}>Remote Image</Text>
              <Image source={{ uri: imageUrl }} style={styles.networkImage} />
              <TextInput
                style={styles.textInput}
                value={text}
                onChangeText={setText}
                placeholder="Type here..."
                autoCorrect={false}
                autoComplete="off"
              />
            </View>
          </ScrollView>
        </KeyboardAvoidingView>
      </SafeAreaView>
    </SafeAreaProvider>
  );
}
```

The `behavior` prop controls how `KeyboardAvoidingView` responds when the keyboard appears:

- `"padding"`: adds padding at the bottom of the view (usually the least disruptive)
- `"height"`: reduces the height of the view (often works better on Android)
- `"position"`: shifts the entire view upward (more aggressive; can cause visible jumping)

**Device check:** the screen scrolls, and tapping the input field does not cause the keyboard to cover it.

> **Common mistake:** Placing `ScrollView` outside `KeyboardAvoidingView`. The scroll container must be inside the keyboard-avoiding wrapper so it can be resized when the keyboard appears. If you get this the wrong way around, the keyboard will still cover the input on iOS.

---

## Activity: Add a Notes Input

You now have a working screen with images and a text input. On your own, add a second labelled input field for notes.

**Task:** Below the existing `TextInput`, add:
- A `Text` label reading "Notes"
- A multi-line `TextInput` for entering notes

**Hints:**
1. The `multiline` prop on `TextInput` enables multi-line entry
2. Multi-line inputs typically need a larger `height` in the stylesheet (for example, `80`)
3. On iOS, `textAlignVertical` does not work on `Text`; use it on `TextInput` instead, setting it to `"top"` so text starts from the top of the field rather than the middle
4. Add a `subHeader` style `Text` label above the input

<details>
<summary>Reference solution</summary>

In the JSX, after the existing `TextInput`:

```jsx
<Text style={styles.subHeader}>Notes</Text>
<TextInput
  style={styles.notesInput}
  value={notes}
  onChangeText={setNotes}
  placeholder="Enter notes here..."
  multiline
  textAlignVertical="top"
  autoCorrect={false}
/>
```

Add state:

```jsx
const [notes, setNotes] = useState('');
```

Add the style:

```jsx
notesInput: {
  height: 80,
  width: '70%',
  borderWidth: 1,
  borderRadius: 10,
  borderColor: '#333',
  padding: 10,
  margin: 12,
},
```

</details>

---

## Part 4: Flexbox Layout

For this section, create a separate Expo app. This keeps your `CompAndLayoutApp` clean, and gives you a dedicated playground you can refer back to later when building more complex layouts.

```bash
npx create-expo-app --template blank LearnFlexApp
cd LearnFlexApp
npx expo start
```

Replace `App.js` with this starter code. The coloured boxes will make it easy to see how each Flexbox property affects the layout visually.

```jsx
import { StatusBar } from 'expo-status-bar';
import { StyleSheet, Text, View } from 'react-native';

export default function App() {
  return (
    <View style={styles.container}>
      <View style={[styles.item, { backgroundColor: '#51cf66' }]}>
        <Text>1</Text>
      </View>
      <View style={[styles.item, { backgroundColor: '#fcc419' }]}>
        <Text>2</Text>
      </View>
      <View style={[styles.item, { backgroundColor: '#ff6b6b' }]}>
        <Text>3</Text>
      </View>
      <StatusBar hidden />
    </View>
  );
}

const styles = StyleSheet.create({
  container: {
    flex: 1,
    backgroundColor: '#fff',
  },
  item: {
    justifyContent: 'center',
    alignItems: 'center',
  },
});
```

**Device check:** three coloured boxes appear stacked vertically, each taking only as much height as their content requires.

### The `flex` property on children

Adding `flex: 1` to a child item tells it to expand and fill the available space in the container, sharing it equally with other items that also have `flex: 1`.

Update the `item` style:

```jsx
item: {
  flex: 1,
  justifyContent: 'center',
  alignItems: 'center',
},
```

**Device check:** the three boxes now divide the screen equally into thirds.

You can use different `flex` values to create proportional splits. To give the first box three-fifths of the space and the others one-fifth each:

```jsx
<View style={[styles.item, { backgroundColor: '#51cf66', flex: 3 }]}>
<View style={[styles.item, { backgroundColor: '#fcc419', flex: 1 }]}>
<View style={[styles.item, { backgroundColor: '#ff6b6b', flex: 1 }]}>
```

> **Why does `flex: 1` on the container matter?** Without `flex: 1` on the outer `View`, the container only takes as much height as its children require. Properties like `justifyContent` have no visible effect because there is no extra space to distribute. Almost every root layout container in a React Native app should have `flex: 1`.

### `flexDirection`

By default, `View` lays out its children in a **column** (top to bottom). This is the opposite of CSS Flexbox, which defaults to `"row"`.

Try changing the `container` style:

```jsx
container: {
  flex: 1,
  backgroundColor: '#fff',
  flexDirection: 'row',
},
```

**Device check:** the three boxes now sit side by side in a row.

Other valid values: `"column-reverse"` and `"row-reverse"` reverse the order of children.

### Main axis and cross axis

The **main axis** is the direction items are placed (determined by `flexDirection`). The **cross axis** runs perpendicular to it.

| `flexDirection` | Main axis | Cross axis |
|---|---|---|
| `"column"` (default) | top to bottom | left to right |
| `"row"` | left to right | top to bottom |

### `justifyContent`: alignment along the main axis

With `flexDirection: "row"` set on the container, experiment with each value:

```jsx
justifyContent: 'flex-start',   // default: items packed at the start
justifyContent: 'flex-end',     // items packed at the end
justifyContent: 'center',       // items centred
justifyContent: 'space-between',// equal space between items; none at edges
justifyContent: 'space-around', // equal space around items; half-space at edges
justifyContent: 'space-evenly', // equal space between items and edges
```

### `alignItems`: alignment along the cross axis

With `flexDirection: "row"`, the cross axis is vertical. Experiment:

```jsx
alignItems: 'stretch',    // default: items stretch to fill the container height
alignItems: 'flex-start', // items align to the top
alignItems: 'flex-end',   // items align to the bottom
alignItems: 'center',     // items centre vertically
alignItems: 'baseline',   // items align by their text baseline
```

> **Default values to remember:** `alignItems` defaults to `"stretch"` (items fill the cross axis). `justifyContent` defaults to `"flex-start"` (items pack at the start of the main axis). These defaults explain why, for example, buttons inside a column container stretch full width without any explicit `width` setting.

### Style merging with arrays

The `style` prop accepts an array of style objects. Later values override earlier ones. This is the idiomatic pattern for applying a base style with a dynamic override:

```jsx
<View style={[styles.item, { backgroundColor: '#51cf66', flex: 3 }]}>
```

You have been using this throughout this section. It is also useful for conditional styles:

```jsx
<View style={[styles.button, isActive && styles.buttonActive]}>
```

---

## Activity: Build a Layout with Flex Ratios

Using only `View`, `Text`, `flexDirection`, `justifyContent`, `alignItems`, and `flex` ratios, build the following layout in `LearnFlexApp`:

- A **header bar** at the top taking one-quarter of the screen height, with a dark background and white centred text reading "Header"
- A **content area** taking the remaining three-quarters, with a light grey background and centred text reading "Content"

**Constraints:**
- Do not use any pixel-based `height` values. Use `flex` ratios only.
- The layout must look correct on both a small and large screen (test by resizing the emulator window if possible).

**Hints:**
1. The root container needs `flex: 1` and `flexDirection: 'column'`
2. Assign `flex: 1` to the header and `flex: 3` to the content area
3. Each section needs `justifyContent: 'center'` and `alignItems: 'center'` to centre its text

<details>
<summary>Reference solution</summary>

```jsx
import { StyleSheet, Text, View } from 'react-native';

export default function App() {
  return (
    <View style={styles.container}>
      <View style={[styles.section, styles.header]}>
        <Text style={styles.headerText}>Header</Text>
      </View>
      <View style={[styles.section, styles.content]}>
        <Text>Content</Text>
      </View>
    </View>
  );
}

const styles = StyleSheet.create({
  container: {
    flex: 1,
    flexDirection: 'column',
  },
  section: {
    justifyContent: 'center',
    alignItems: 'center',
  },
  header: {
    flex: 1,
    backgroundColor: '#343a40',
  },
  content: {
    flex: 3,
    backgroundColor: '#f1f3f5',
  },
  headerText: {
    color: '#fff',
    fontWeight: '700',
    fontSize: 18,
  },
});
```

</details>

---

## Bonus Challenges

Work on as many as you can. They are listed in order of difficulty. No solutions are provided.

### Challenge 1: Styled `TextInput` with focus state

In `CompAndLayoutApp`, style the `TextInput` so that its border colour changes when the field is focused.

**Hints:**
- Use `onFocus` and `onBlur` props on `TextInput`
- Store a boolean `isFocused` state value
- Use style array merging to apply a different `borderColor` when `isFocused` is `true`

### Challenge 2: Card component

Extract the image, label, and sub-header from `CompAndLayoutApp` into a reusable `Card` component that accepts `imageSource`, `title`, and `subtitle` as props. Display two cards in a column.

**Hints:**
- Create a `components/` folder and add `Card.js`
- The component should render a `View` wrapping a `Text` for the title, a `Text` for the subtitle, and an `Image`
- Import and use it in `App.js` twice with different props

### Challenge 3: Three-column grid

In `LearnFlexApp`, create a 3x2 grid of coloured boxes (three columns, two rows). Each box should be square and take equal width.

**Hints:**
- Use two row `View` containers, each with `flexDirection: 'row'`
- Each box inside a row should have `flex: 1`
- Set a fixed `height` on each row, or use `aspectRatio: 1` on each box

### Challenge 4: Responsive image width

In `CompAndLayoutApp`, make the images fill the full width of the screen regardless of the device size.

**Hints:**
- Import `Dimensions` from `react-native`
- `const { width } = Dimensions.get('window')` gives you the screen width in pixels
- Apply `width` dynamically in the style: `{ width: width - 32, height: width - 32 }`

---

## Summary

- React Native provides its own set of primitive components instead of HTML elements. `View`, `Text`, `Image`, `TextInput`, and `ScrollView` cover the majority of everyday mobile UI needs.
- All styling is JavaScript: camelCase property names, no units, no `className`. `StyleSheet.create()` is the preferred approach.
- Flexbox is the layout system in React Native. Key differences from CSS: `flexDirection` defaults to `"column"`, and `flex: 1` on the root container is required for alignment properties to take effect.
- Mobile-specific problems require dedicated solutions: `SafeAreaView` for the device notch, `KeyboardAvoidingView` for the software keyboard.

---

## Additional Resources

- [React Native: Core Components and APIs](https://reactnative.dev/docs/components-and-apis)
- [React Native: Style](https://reactnative.dev/docs/style)
- [React Native: Layout with Flexbox](https://reactnative.dev/docs/flexbox)
- [React Native: TextInput](https://reactnative.dev/docs/textinput)
- [Expo: Safe Area Context](https://docs.expo.dev/versions/latest/sdk/safe-area-context/)
