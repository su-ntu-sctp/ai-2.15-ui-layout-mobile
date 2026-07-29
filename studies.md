# Pre-Reading: Lesson 2.15: UI Composition and Layout Management in Mobile Applications

Timebox **2.5–3 hours** across these resources before the lesson. You do not need to memorise everything; focus on building a mental model so the hands-on lab clicks faster.

---

## 1. Core Components: View, Text, Image, and TextInput

**Read (20 min)**

- [React Native: Core Components and APIs](https://reactnative.dev/docs/components-and-apis): Read the individual entries for `View`, `Text`, `Image`, and `TextInput`. For each one, pay attention to the required props and any constraints (for example, when `width` and `height` are mandatory on `Image`).

**Key ideas to take away:**

- `View` is a layout container. It cannot display text directly; any text must be inside a `Text` component.
- `Image` loads local files and remote URLs differently. Remote images require explicit dimensions because React Native cannot infer them at bundle time.
- `TextInput` is a controlled component: you pass `value` and `onChangeText` to manage its state yourself, the same pattern as in React for the web.

---

## 2. Styling in React Native

**Read (15 min)**

- [React Native: Style](https://reactnative.dev/docs/style): Read the whole page. Note the differences from CSS: camelCase property names, no units on numeric values, no `className` prop, and the `StyleSheet.create()` API.

**Key ideas to take away:**

- All styling in React Native is JavaScript. There are no CSS files and no `className`.
- Many CSS properties you are used to do not exist. For example, `border` shorthand is not supported; you must use `borderWidth`, `borderColor`, `borderRadius`, and side-specific variants like `borderBottomWidth`.
- `StyleSheet.create()` validates style properties during development and separates style definitions from JSX. Plain JavaScript objects work too and are useful for dynamic styles.

---

## 3. Layout with Flexbox

**Read (20 min)**

- [React Native: Layout with Flexbox](https://reactnative.dev/docs/flexbox): Read the full article. It is short and walks through `flexDirection`, `justifyContent`, `alignItems`, and the `flex` property with interactive examples.

**Watch (14 min)**

- [React Native Flexbox Crash Course](https://www.youtube.com/watch?v=R2eqAgR_KlU): A concise visual walkthrough of Flexbox in React Native. Focus on the section explaining the difference between the main axis and the cross axis.

**Key ideas to take away:**

- React Native uses Flexbox as its only layout system. There is no CSS Grid.
- `flexDirection` defaults to `'column'` in React Native, the opposite of CSS which defaults to `'row'`. This means children stack vertically by default.
- `flex: 1` on a container fills all available space in its parent. Without it, `justifyContent` and `alignItems` have no extra space to work with and produce no visible effect.
- `justifyContent` aligns items along the **main axis** (the direction set by `flexDirection`). `alignItems` aligns items along the **cross axis** (perpendicular to `flexDirection`).

---

## 4. The Button Component and Its Limits

**Read (10 min)**

- [React Native: Button](https://reactnative.dev/docs/button): Read the whole page. Note which props are required, and which platform difference affects the `color` prop.

**Key ideas to take away:**

- `Button` supports only a minimal, fixed set of props: `title`, `onPress`, `color`, and `disabled`. It does not accept a `style` prop, so its border, padding, font, and shape cannot be customised.
- The `color` prop is not consistent across platforms: on iOS it sets the text color, on Android it sets the background color. The same code can look different on each platform.
- For any custom appearance, React Native apps typically build their own button using `Pressable`, the base component for touchable elements, rather than relying on `Button`.

---

## 5. Scrollable Layouts and the Keyboard Problem

**Read (15 min)**

- [React Native: ScrollView](https://reactnative.dev/docs/scrollview): Skim the props list and read the note about performance. Pay attention to the warning that `ScrollView` renders all its children at once.
- [React Native: KeyboardAvoidingView](https://reactnative.dev/docs/keyboardavoidingview): Read the description and the `behavior` prop. Note the three options: `'padding'`, `'height'`, and `'position'`, and how they differ.

**Read (5 min)**

- [Expo: Safe Area Context](https://docs.expo.dev/versions/latest/sdk/safe-area-context/): Skim the "Usage" section. The key takeaway is that `SafeAreaProvider` must wrap your component tree and `SafeAreaView` applies the insets.

**Key ideas to take away:**

- `ScrollView` is needed whenever your content might overflow the screen. Unlike a browser, React Native does not make content scrollable by default.
- Modern devices have notches and status bars that can overlap your content. `SafeAreaView` from `react-native-safe-area-context` handles this correctly across all devices.
- The software keyboard can cover input fields on iOS. `KeyboardAvoidingView` adjusts the layout automatically when the keyboard appears.

---

## Reflection (5 min)

Before the lesson, write down answers to these three questions:

1. What is the difference between `flexDirection: 'column'` and `flexDirection: 'row'`? How does the main axis change between them?
2. Why does a network image not appear if you do not provide a `width` and `height`?
3. Why might a production app avoid the built-in `Button` component in favour of a custom `Pressable`-based one?
4. What is one concept from the pre-reading that you would like the instructor to demonstrate more clearly?

Bring question 4 to class.
