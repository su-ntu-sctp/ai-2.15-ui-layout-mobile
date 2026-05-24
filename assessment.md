# Assessment: UI Composition and Layout Management in Mobile Applications

## Overview

- **Lesson:** UI Composition and Layout Management in Mobile Applications / 2.15
- **Format:** 30 questions (MCQ and True/False)
- **Time:** ~30 minutes
- **Scoring:** 1 point each

## Questions

### Q1

Which React Native component is the closest equivalent to an HTML `<div>`?

A - `Container`

B - `Box`

C - `View`

D - `Section`

---

### Q2

Which React Native component must be used to display any text on screen?

A - `Label`

B - `Paragraph`

C - `Span`

D - `Text`

---

### Q3 (True/False)

In React Native, you can use heading tags such as `<h1>` and `<h2>` to create visual hierarchy in your layout.

A - True

B - False

---

### Q4

A learner writes the following code. What will happen when the app runs?

```jsx
<View>
  Welcome to React Native
</View>
```

A - The text is displayed as plain, unstyled text

B - React Native throws an error because bare strings cannot be rendered inside `View`

C - React Native silently ignores the string and renders an empty `View`

D - React Native wraps the string in a `Text` component automatically

---

### Q5

Which of the following is the correct way to set a background colour in React Native?

A - `style="background-color: #fff;"`

B - `style={{ 'background-color': '#fff' }}`

C - `className="bg-white"`

D - `style={{ backgroundColor: '#fff' }}`

---

### Q6 (True/False)

In React Native, numeric style values such as `fontSize: 16` are interpreted as density-independent pixels by default.

A - True

B - False

---

### Q7

Which of the following style definitions is valid in React Native?

A - `{ fontSize: '16px', fontWeight: 'bold' }`

B - `{ font-size: 16, font-weight: '700' }`

C - `{ fontSize: 16, fontWeight: '700' }`

D - `{ fontSize: '16', fontWeight: bold }`

---

### Q8

What is the primary advantage of using `StyleSheet.create()` over plain JavaScript style objects?

A - It allows you to use CSS class names in React Native

B - It validates style properties during development and separates styles from JSX

C - It automatically applies styles to all child components

D - It enables hot reload for style changes

---

### Q9 (True/False)

The CSS shorthand `border: '1px solid #ccc'` works in React Native the same way it does in a browser.

A - True

B - False

---

### Q10

A learner wants to draw a bottom border under a `Text` component. Which style properties should they use?

A - `borderBottom: '1px solid #000'`

B - `borderStyle: 'solid', borderBottomColor: '#000'`

C - `borderBottomWidth: 1, borderBottomColor: '#000'`

D - `outline: '1px solid #000'`

---

### Q11

How does the `Image` component load a local image file in React Native?

A - `<Image src="./assets/photo.png" />`

B - `<Image source="./assets/photo.png" />`

C - `<Image source={require('./assets/photo.png')} />` or by importing the file and passing the import as `source`

D - `<img src={require('./assets/photo.png')} />`

---

### Q12 (True/False)

A network image rendered with `<Image source={{ uri: url }} />` will appear on screen even if no `width` or `height` style is provided.

A - True

B - False

---

### Q13

Why does a network image not appear when no explicit dimensions are provided?

A - React Native blocks all network requests by default

B - The `uri` prop requires a `resizeMode` prop to be set before the image can render

C - React Native cannot determine the size of a remote image at bundle time, so the component occupies zero space without explicit dimensions

D - Network images must be downloaded to local storage before they can be displayed

---

### Q14

Which prop does `TextInput` use to notify the component of new text entered by the user?

A - `onChange`

B - `onInput`

C - `onChangeText`

D - `onTextChange`

---

### Q15 (True/False)

`TextInput` in React Native is an uncontrolled component by default; you do not need to pass a `value` prop to use it.

A - True

B - False

---

### Q16

A learner adds a `TextInput` to their screen and taps on it. On iOS, the software keyboard appears and covers the input field. Which component is designed to fix this problem?

A - `SafeAreaView`

B - `ScrollView`

C - `KeyboardAvoidingView`

D - `KeyboardDismissView`

---

### Q17

What does `SafeAreaView` from `react-native-safe-area-context` do?

A - Prevents the software keyboard from covering input fields

B - Makes all content in the view scrollable

C - Applies automatic padding to keep content within the device's safe area, away from notches and status bars

D - Locks the screen orientation to portrait mode

---

### Q18

Why should `npx expo install` be used instead of `npm install` when adding a library to an Expo project?

A - `npm install` does not work inside Expo projects

B - `npx expo install` selects the library version compatible with the installed Expo SDK automatically

C - `npx expo install` is faster than `npm install` for large packages

D - `npm install` cannot install native modules

---

### Q19

In the correct nesting order for handling both safe area and keyboard, which component sits immediately inside `SafeAreaView`?

A - `ScrollView`

B - `View`

C - `KeyboardAvoidingView`

D - `TextInput`

---

### Q20 (True/False)

`ScrollView` renders only the items currently visible on screen, making it suitable for very long lists.

A - True

B - False

---

### Q21

What is the default value of `flexDirection` in React Native?

A - `row`

B - `row-reverse`

C - `column-reverse`

D - `column`

---

### Q22

A `View` with `flex: 1` is placed inside a container that also has `flex: 1`. What does `flex: 1` on the child mean?

A - The child takes exactly 1 pixel of space

B - The child expands to fill all available space in the container, sharing it equally with other `flex: 1` siblings

C - The child is hidden until the user scrolls to it

D - The child takes 1% of the parent's size

---

### Q23

Three `View` children are placed inside a container. The first has `flex: 2`, the second has `flex: 1`, and the third has `flex: 1`. How is the available space divided?

A - The first takes half the space and the second and third take a quarter each

B - All three take equal space because the values are relative

C - The first takes two-thirds and the second and third each take one-sixth

D - The first takes 2 pixels and the second and third take 1 pixel each

---

### Q24 (True/False)

Removing `flex: 1` from the root container `View` has no effect on how `justifyContent` and `alignItems` behave.

A - True

B - False

---

### Q25

Which property controls alignment of items along the **main axis** in a flex container?

A - `alignItems`

B - `alignSelf`

C - `justifyContent`

D - `alignContent`

---

### Q26

Which property controls alignment of items along the **cross axis** in a flex container?

A - `justifyContent`

B - `alignItems`

C - `flexGrow`

D - `flexWrap`

---

### Q27

What is the default value of `alignItems` in a React Native `View`?

A - `flex-start`

B - `center`

C - `flex-end`

D - `stretch`

---

### Q28

A learner wants to apply a shared base style and a conditional override to the same component. Which approach is idiomatic in React Native?

A - `style={isActive ? styles.base + styles.active : styles.base}`

B - `style={[styles.base, isActive && styles.active]}`

C - `style={Object.assign(styles.base, styles.active)}`

D - `className={isActive ? 'base active' : 'base'}`

---

### Q29

A learner has a container with `flexDirection: 'row'` and three equal children, each with `flex: 1`. They set `justifyContent: 'space-between'` on the container. What will they observe?

A - The three children are centred in a single column

B - The first child is at the left edge, the last is at the right edge, and the middle child is centred between them

C - All three children are pushed to the right edge of the container

D - The children overflow outside the container

---

### Q30

A learner writes the following code. What problem will they encounter on iOS when the keyboard appears?

```jsx
<SafeAreaProvider>
  <SafeAreaView style={{ flex: 1 }}>
    <ScrollView>
      <KeyboardAvoidingView behavior="padding">
        <TextInput placeholder="Enter text" />
      </KeyboardAvoidingView>
    </ScrollView>
  </SafeAreaView>
</SafeAreaProvider>
```

A - The `SafeAreaView` will not apply safe-area insets when nested inside `SafeAreaProvider`

B - The `ScrollView` will not scroll because it is missing a `style` prop

C - `KeyboardAvoidingView` will not function correctly because it is inside `ScrollView` instead of wrapping it

D - `TextInput` will not accept any keyboard input when placed inside `KeyboardAvoidingView`

---
