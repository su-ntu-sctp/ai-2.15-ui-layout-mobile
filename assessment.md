# Assessment: UI Composition and Layout Management in Mobile Applications

## Overview

- **Lesson:** UI Composition and Layout Management in Mobile Applications / 2.15
- **Format:** 10 questions (MCQ and True/False)
- **Time:** ~10–15 minutes
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

A learner wants to display a network image from a URL. Which of the following is the correct way to set the `source` prop?

A - `source={imageUrl}`

B - `source={{ href: imageUrl }}`

C - `source={{ uri: imageUrl }}`

D - `source={<img src={imageUrl} />}`

---

### Q3

A learner adds a network `Image` component with a valid `source` prop but no explicit `width` or `height` style. What will happen?

A - React Native fetches the image and renders it at its natural dimensions

B - The image is invisible because React Native cannot determine its size before fetching

C - React Native throws a build error requiring a `size` prop

D - The image stretches to fill the screen automatically

---

### Q4

Which of the following correctly describes how `TextInput` works in React Native?

A - It manages its own value internally like an HTML `<input>` and does not need state

B - It is a controlled component: the value is stored in state and updated via `onChangeText`

C - It requires a `ref` to read its value; there is no `value` prop

D - It only supports single-line text; multi-line input requires a separate `TextArea` component

---

### Q5

What is the purpose of wrapping your screen in `SafeAreaView` from `react-native-safe-area-context`?

A - It prevents the app from rotating to landscape mode

B - It automatically adds padding so content is not hidden behind the device notch or status bar

C - It wraps the component tree in an error boundary so crashes are handled gracefully

D - It enables hardware-accelerated rendering on devices that support it

---

### Q6

A learner places `KeyboardAvoidingView` inside `ScrollView`. The keyboard still covers the text input on iOS. What is the likely cause?

A - `KeyboardAvoidingView` is not supported on iOS; use `KeyboardAwareScrollView` instead

B - The `behavior` prop is missing

C - `ScrollView` must be nested inside `KeyboardAvoidingView`, not the other way around

D - `SafeAreaView` must be removed before `KeyboardAvoidingView` will work

---

### Q7

What is the primary advantage of using `StyleSheet.create()` over plain JavaScript style objects?

A - It allows you to use CSS class names in React Native

B - It validates style properties during development and separates styles from JSX

C - It automatically applies styles to all child components

D - It enables hot reload for style changes

---

### Q8 (True/False)

In React Native, the default value of `flexDirection` on a `View` is `"column"`, which is the opposite of the CSS Flexbox default.

A - True

B - False

---

### Q9

A container has `flexDirection: "row"`. Which property controls how child items are spaced along the horizontal axis, and which controls their alignment on the vertical axis?

A - `alignItems` controls horizontal spacing; `justifyContent` controls vertical alignment

B - `justifyContent` controls horizontal spacing; `alignItems` controls vertical alignment

C - Both `justifyContent` and `alignItems` operate on the horizontal axis when `flexDirection` is `"row"`

D - `flexGrow` controls horizontal spacing; `alignSelf` controls vertical alignment

---

### Q10

A screen has three child `View` components inside a root container with `flex: 1`. The first child has `flex: 2` and the other two each have `flex: 1`. What fraction of the screen height does the first child occupy?

A - One-third

B - One-half

C - Two-thirds

D - Two-fifths
