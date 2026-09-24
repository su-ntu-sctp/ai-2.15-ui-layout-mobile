# Lesson 2.15: UI Composition and Layout Management in Mobile Applications

## Overview

- **Duration:** ~2 hours (hands-on lab)
- **Prerequisites:** Lesson 2.14 (Expo environment set up, Expo Go working on your device or emulator, basic familiarity with `View`, `Text`, and `StyleSheet`)
- **Timing guide:**
  - Setup: 5 min
  - Part 1 (Core Components and Styling): 20 min
  - Part 2 (The `Image` Component): 15 min
  - Part 3 (`TextInput`, `ScrollView`, `KeyboardAvoidingView`): 35 min
  - Part 4 (The `Button` Component): 20 min
  - Part 5 (Flexbox Layout): 20 min
  - Buffer / wrap-up: 5 min
  - The closing "Build a Layout with Flex Ratios" activity is optional; treat it as a stretch goal alongside the Bonus Challenges if time is short.

## Learning Objectives

By the end of this lesson, you will be able to:

1. **Use** core React Native components (`View`, `Text`, `Image`, `TextInput`, `ScrollView`, `Button`) to compose a functional mobile screen
2. **Apply** `StyleSheet.create()` to style components, and solve mobile-specific layout problems with `SafeAreaView` and `KeyboardAvoidingView`
3. **Explain** how Flexbox works in React Native, including the main axis, the cross axis, and the core layout properties
4. **Build** mobile layouts using Flexbox properties such as `flexDirection`, `justifyContent`, `alignItems`, and `flex`

---

## Setup: Create the Project

This lesson uses a fresh Expo app. Open a terminal and run:

```bash
npx create-expo-app --template blank components-app
```

You will again be prompted to choose an Expo SDK version:

```
? Select an Expo SDK version: › - Use arrow-keys. Return to submit.
❯   Latest (SDK 57) - Recommended for most projects
    For learning with Expo Go (SDK 54)
    Other SDK version…
```

Select **"Latest (SDK 57)"** and press Return. Expo Go now only supports SDK 57, so this keeps the project compatible with the Expo Go app installed on your device or emulator.

```bash
cd components-app
npx expo start
```

Open the emulator or scan the QR code with Expo Go. Confirm the default screen loads.

> This app is separate from your Lesson 2.14 counter app. It is a sandbox for exploring components and layout, so keeping it simple and focused is intentional.

Open `App.js` and replace its contents with the following starting point:

```jsx
// App.js
import { StyleSheet, Text, View } from 'react-native';

export default function App() {
  return (
    <View style={styles.container}>
      <Text style={styles.header}>Components App</Text>
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

**Device check:** you should see "Components App" centred on screen.

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
// App.js
import { StyleSheet, Text, View } from 'react-native';

export default function App() {
  return (
    <View style={styles.container}>
      <Text style={styles.header}>Components App</Text>
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

### Step 2: Extract a `Header` component

The `header` style is only used once, but as an app grows, a title like this is often reused across multiple screens with a different colour each time. Extract it into its own component to see how React Native components are built from the same primitives you have already been using.

Create a `components` folder in your project root, and inside it a file named `Header.js`:

```jsx
// components/Header.js
import { StyleSheet, Text } from 'react-native';

function Header({ title, color = 'darkblue' }) {
  return (
    <Text style={[styles.header, { color, borderBottomColor: color }]}>
      {title}
    </Text>
  );
}

const styles = StyleSheet.create({
  header: {
    fontSize: 24,
    fontWeight: 'bold',
    borderBottomWidth: 1,
    marginBottom: 20,
    paddingBottom: 5,
  },
});

export default Header;
```

Notice the `style` prop here is an array: `[styles.header, { color, borderBottomColor: color }]`. React Native merges the objects in order, so the second object overrides any matching keys in `styles.header`. Here `borderBottomColor` is set from the same `color` prop as the text, so the underline always matches the title, whatever colour is passed in. This is the standard way to combine a shared base style with values that change per instance; you will see this pattern again, formally, in the Flexbox section later in this lesson.

> **Common mistake:** In CSS you would write `border-bottom: 1px solid darkblue` as a single shorthand property. React Native has no shorthand; `borderBottomWidth` and `borderBottomColor` must be set separately, exactly as you saw earlier in this lesson.

The `color` prop also has a default value of `'darkblue'`, so `<Header title="Components App" />` works even without passing a `color`.

Import `Header` in `App.js` and replace the existing `Text` title with it:

```jsx
// App.js
import { StyleSheet, Text, View } from 'react-native';
import Header from './components/Header';

export default function App() {
  return (
    <View style={styles.container}>
      <Header title="Components App" color="darkblue" />
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
  subHeader: {
    fontSize: 18,
    fontWeight: '600',
    color: 'darkblue',
    marginBottom: 10,
  },
});
```

The `header` style and its underline are no longer needed in `App.js`; they now live inside `Header.js`.

**Device check:** the title still reads "Components App" in dark blue, exactly as before, but it is now rendered by a reusable component.

> **Common mistake:** Forgetting the array brackets and writing `style={styles.header, { color, borderBottomColor: color }}`. This is a JavaScript comma expression, not a style merge; it silently evaluates to just the last object, so `styles.header` is dropped entirely and the text loses its `fontSize`, `fontWeight`, and underline.

## Activity: Build a `SubHeader` Component

You have just extracted `Header` into a reusable component. The `subHeader` style is used even more often across this lesson (you will see it again for "Local Image", "Remote Image", and later "Sign Up Form"). On your own, extract it the same way.

**Task:** Create `components/SubHeader.js`, a component that accepts `title` and `color` props and renders the text with the `subHeader` style. Replace the `<Text style={styles.subHeader}>Hello World!</Text>` line in `App.js` with your new component.

**Hints:**
1. Follow the same shape as `Header.js`: a function component, a local `StyleSheet.create()`, a default export
2. Give `color` a default value of `'darkblue'`, the same way `Header` defaults `color`
3. Merge the base style with the color override using the array form: `[styles.subHeader, { color }]`
4. Remember to remove the now-unused `subHeader` style from `App.js`'s own stylesheet once it lives in `SubHeader.js`
5. Import `SubHeader` in `App.js` and use it as `<SubHeader title="Hello World!" />`

<details>
<summary>Reference solution</summary>

```jsx
// components/SubHeader.js
import { StyleSheet, Text } from 'react-native';

function SubHeader({ title, color = 'darkblue' }) {
  return <Text style={[styles.subHeader, { color }]}>{title}</Text>;
}

const styles = StyleSheet.create({
  subHeader: {
    fontSize: 18,
    fontWeight: '600',
    marginBottom: 10,
  },
});

export default SubHeader;
```

```jsx
// App.js
import { StyleSheet, View } from 'react-native';
import Header from './components/Header';
import SubHeader from './components/SubHeader';

export default function App() {
  return (
    <View style={styles.container}>
      <Header title="Components App" color="darkblue" />
      <SubHeader title="Hello World!" />
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
});
```

</details>

From this point on, the lesson uses `<Header title="Components App" color="darkblue" />` and `<SubHeader title="..." />` in place of the raw `Text` elements.

---

## Part 2: The `Image` Component

The `Image` component displays images from two sources: local files bundled with the app, and remote URLs.

### Step 1: Add a local image

Copy `ntu-building.webp` (provided in the lesson `assets/images/` folder) into your project's `assets/images/` folder. Then import and display it:

```jsx
// App.js
import { StyleSheet, View, Image } from 'react-native';
import Header from './components/Header';
import SubHeader from './components/SubHeader';

export default function App() {
  return (
    <View style={styles.container}>
      <Header title="Components App" color="darkblue" />
      <SubHeader title="Local Image" />
      <Image
        source={require('./assets/images/ntu-building.webp')}
        style={styles.bannerImg}
      />
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
  bannerImg: {
    width: '90%',
    resizeMode: 'contain',
  },
});
```

**Device check:** the NTU building image appears, scaled to 90% of the screen width.

### Step 2: Add a network image

Network images require an explicit `width` and `height` because React Native cannot determine their dimensions ahead of time.

Add a second `Image` component below the local one:

```jsx
// App.js
<SubHeader title="Remote Image" />
<Image
  source={{ uri: 'https://i.imgur.com/9wvRTDo.png' }}
  style={styles.image}
/>
```

And the style:

```jsx
// App.js
image: {
  width: 350,
  height: 350,
  marginBottom: 20,
},
```

**Device check:** both images appear on screen.

> **Common mistake:** Using `source={'https://i.imgur.com/9wvRTDo.png'}` instead of `source={{ uri: 'https://i.imgur.com/9wvRTDo.png' }}`. Network images require the object form with a `uri` key.

> **Why does nothing appear without a size?** React Native needs to know the rendered dimensions before it can fetch and display a network image. Without `width` and `height` (or a `flex` value that gives the component a size), the image occupies zero space and is invisible.

---

## Part 3: `TextInput`, `ScrollView`, and `KeyboardAvoidingView`

From this point on, `components-app` becomes a sign-up page for an "AI Engineering Course", so you can practise composing a screen that looks closer to a real app.

### Step 1: Build the sign-up page content

Replace the contents of `App.js` with the new screen. This reuses the same two images from Part 2, but with new headings and a paragraph of body copy:

```jsx
// App.js
import { StyleSheet, View, Image, Text } from 'react-native';
import { StatusBar } from 'expo-status-bar';
import Header from './components/Header';
import SubHeader from './components/SubHeader';

export default function App() {
  return (
    <View style={styles.container}>
      <Image
        source={require('./assets/images/ntu-building.webp')}
        style={styles.bannerImg}
      />
      <Header title="AI Engineering Course" color="darkblue" />
      <Image
        source={{ uri: 'https://i.imgur.com/9wvRTDo.png' }}
        style={styles.image}
      />
      <SubHeader title="Sign Up Form" />
      <Text style={styles.mainText}>
        Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do
        eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad
        minim veniam, quis nostrud exercitation ullamco laboris nisi ut
        aliquip ex ea commodo consequat. Duis aute irure dolor in
        reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla
        pariatur. Excepteur sint occaecat cupidatat non proident, sunt in
        culpa qui officia deserunt mollit anim id est laborum. At vero eos et
        accusamus et iusto odio dignissimos ducimus qui blanditiis praesentium
        voluptatum deleniti atque corrupti quos dolores et quas molestias
        excepturi sint occaecati cupiditate non provident, similique sunt in
        culpa qui officia deserunt mollitia animi, id est laborum et dolorum
        fuga. Et harum quidem rerum facilis est et expedita distinctio. Nam
        libero tempore, cum soluta nobis est eligendi optio cumque nihil impedit
        quo minus id quod maxime placeat facere possimus, omnis voluptas
        assumenda est, omnis dolor repellendus. Temporibus autem quibusdam et
        aut officiis debitis aut rerum necessitatibus saepe eveniet ut et
        voluptates repudiandae sint et molestiae non recusandae. Itaque earum
        rerum hic tenetur a sapiente delectus, ut aut reiciendis voluptatibus
        maiores alias consequatur aut perferendis doloribus asperiores repellat.
      </Text>
      <StatusBar style="dark" />
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
  bannerImg: {
    width: '90%',
    resizeMode: 'contain',
  },
  image: {
    width: 350,
    height: 350,
    marginBottom: 20,
  },
  mainText: {
    fontSize: 16,
    marginBottom: 20,
    paddingHorizontal: 20,
  },
});
```

**Device check:** the content overflows past the bottom of the screen. There is no way to reach the text below the fold.

> **Why does the content overflow?** A plain `View` does not scroll. Once its children need more vertical space than the screen provides, the extra content simply renders off-screen with no way to reach it.

### Step 2: Fix the overflow with `ScrollView`

`ScrollView` makes its content scrollable whenever it does not fit on screen. Wrap the existing content in one, and move `flex: 1` out of `styles.container` and onto the `ScrollView` itself:

```jsx
// App.js
import { StyleSheet, Image, Text, ScrollView } from 'react-native';
import { StatusBar } from 'expo-status-bar';
import Header from './components/Header';
import SubHeader from './components/SubHeader';

export default function App() {
  return (
    <ScrollView style={{ flex: 1 }} contentContainerStyle={styles.container}>
      <Image
        source={require('./assets/images/ntu-building.webp')}
        style={styles.bannerImg}
      />
      <Header title="AI Engineering Course" color="darkblue" />
      <Image
        source={{ uri: 'https://i.imgur.com/9wvRTDo.png' }}
        style={styles.image}
      />
      <SubHeader title="Sign Up Form" />
      <Text style={styles.mainText}>
        Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do
        eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad
        minim veniam, quis nostrud exercitation ullamco laboris nisi ut
        aliquip ex ea commodo consequat. Duis aute irure dolor in
        reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla
        pariatur. Excepteur sint occaecat cupidatat non proident, sunt in
        culpa qui officia deserunt mollit anim id est laborum. At vero eos et
        accusamus et iusto odio dignissimos ducimus qui blanditiis praesentium
        voluptatum deleniti atque corrupti quos dolores et quas molestias
        excepturi sint occaecati cupiditate non provident, similique sunt in
        culpa qui officia deserunt mollitia animi, id est laborum et dolorum
        fuga. Et harum quidem rerum facilis est et expedita distinctio. Nam
        libero tempore, cum soluta nobis est eligendi optio cumque nihil impedit
        quo minus id quod maxime placeat facere possimus, omnis voluptas
        assumenda est, omnis dolor repellendus. Temporibus autem quibusdam et
        aut officiis debitis aut rerum necessitatibus saepe eveniet ut et
        voluptates repudiandae sint et molestiae non recusandae. Itaque earum
        rerum hic tenetur a sapiente delectus, ut aut reiciendis voluptatibus
        maiores alias consequatur aut perferendis doloribus asperiores repellat.
      </Text>
      <StatusBar style="dark" />
    </ScrollView>
  );
}

const styles = StyleSheet.create({
  container: {
    backgroundColor: '#fff',
    alignItems: 'center',
    justifyContent: 'center',
  },
  bannerImg: {
    width: '90%',
    resizeMode: 'contain',
  },
  image: {
    width: 350,
    height: 350,
    marginBottom: 20,
  },
  mainText: {
    fontSize: 16,
    marginBottom: 20,
    paddingHorizontal: 20,
  },
});
```

**Device check:** you can now scroll down and read the entire paragraph.

> **Why `contentContainerStyle` instead of `style`?** `ScrollView` has two style props. `style` sizes the scrollable viewport itself; `contentContainerStyle` styles the inner content, the same way `styles.container` styled your root `View` before. `flex: 1` goes on `style`, so the viewport fills the screen. Styles that apply to the content (alignment, padding, background) belong on `contentContainerStyle`.

### Step 3: Add `TextInput`s for name and email

`TextInput` is React Native's equivalent of `<input type="text">`. Like its web counterpart, it can be used as a controlled component: you manage its value with `useState`, which is how this lesson uses it throughout.

> **Different from React web:** `onChangeText` passes the new string directly, not an event. There is no `event.target.value` to read, as there would be with `onChange` on a web `<input>`.

Add two inputs, one for name and one for email:

```jsx
// App.js
import { StyleSheet, Image, Text, ScrollView, TextInput } from 'react-native';
import { StatusBar } from 'expo-status-bar';
import { useState } from 'react';
import Header from './components/Header';
import SubHeader from './components/SubHeader';

export default function App() {
  const [name, setName] = useState('');
  const [email, setEmail] = useState('');

  return (
    <ScrollView style={{ flex: 1 }} contentContainerStyle={styles.container}>
      <Image
        source={require('./assets/images/ntu-building.webp')}
        style={styles.bannerImg}
      />
      <Header title="AI Engineering Course" color="darkblue" />
      <Image
        source={{ uri: 'https://i.imgur.com/9wvRTDo.png' }}
        style={styles.image}
      />
      <SubHeader title="Sign Up Form" />
      <Text style={styles.mainText}>
        Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do
        eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad
        minim veniam, quis nostrud exercitation ullamco laboris nisi ut
        aliquip ex ea commodo consequat. Duis aute irure dolor in
        reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla
        pariatur. Excepteur sint occaecat cupidatat non proident, sunt in
        culpa qui officia deserunt mollit anim id est laborum. At vero eos et
        accusamus et iusto odio dignissimos ducimus qui blanditiis praesentium
        voluptatum deleniti atque corrupti quos dolores et quas molestias
        excepturi sint occaecati cupiditate non provident, similique sunt in
        culpa qui officia deserunt mollitia animi, id est laborum et dolorum
        fuga. Et harum quidem rerum facilis est et expedita distinctio. Nam
        libero tempore, cum soluta nobis est eligendi optio cumque nihil impedit
        quo minus id quod maxime placeat facere possimus, omnis voluptas
        assumenda est, omnis dolor repellendus. Temporibus autem quibusdam et
        aut officiis debitis aut rerum necessitatibus saepe eveniet ut et
        voluptates repudiandae sint et molestiae non recusandae. Itaque earum
        rerum hic tenetur a sapiente delectus, ut aut reiciendis voluptatibus
        maiores alias consequatur aut perferendis doloribus asperiores repellat.
      </Text>
      <TextInput
        style={styles.textInput}
        placeholder="Enter your name"
        value={name}
        onChangeText={setName}
        autoCorrect={false}
        autoComplete="off"
      />
      <TextInput
        style={styles.textInput}
        placeholder="Enter your email"
        value={email}
        onChangeText={setEmail}
        autoCorrect={false}
        autoComplete="off"
      />
      <StatusBar style="dark" />
    </ScrollView>
  );
}
```

Add the `textInput` style:

```jsx
// App.js
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

**Device check:** two labelled inputs appear below the paragraph.

You will now notice two problems:

1. The content at the top of the screen is hidden behind the device notch and status bar
2. On iOS, the software keyboard covers the input field when you tap on it

You will fix both of these in the next steps.

### Step 4: Fix the notch with `SafeAreaView`

A hardcoded `marginTop` is not reliable because different devices have notches and status bars of different sizes. The correct solution is `react-native-safe-area-context`, which measures the safe area on any device automatically.

Install it:

```bash
npx expo install react-native-safe-area-context
```

> **Why `npx expo install` and not `npm install`?** Expo manages native library versions to match the installed Expo SDK. Using `npx expo install` picks the correct version for your project automatically.

Update your imports and wrap the `ScrollView` in `SafeAreaProvider` and `SafeAreaView`:

```jsx
// App.js
import { StyleSheet, Image, Text, ScrollView, TextInput } from 'react-native';
import { StatusBar } from 'expo-status-bar';
import { useState } from 'react';
import { SafeAreaProvider, SafeAreaView } from 'react-native-safe-area-context';
import Header from './components/Header';
import SubHeader from './components/SubHeader';

export default function App() {
  const [name, setName] = useState('');
  const [email, setEmail] = useState('');

  return (
    <SafeAreaProvider>
      <SafeAreaView style={{ flex: 1, backgroundColor: '#fff' }}>
        <ScrollView style={{ flex: 1 }} contentContainerStyle={styles.container}>
          <Image
            source={require('./assets/images/ntu-building.webp')}
            style={styles.bannerImg}
          />
          <Header title="AI Engineering Course" color="darkblue" />
          <Image
            source={{ uri: 'https://i.imgur.com/9wvRTDo.png' }}
            style={styles.image}
          />
          <SubHeader title="Sign Up Form" />
          <Text style={styles.mainText}>
            Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do
            eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut
            enim ad minim veniam, quis nostrud exercitation ullamco laboris
            nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in
            reprehenderit in voluptate velit esse cillum dolore eu fugiat
            nulla pariatur. Excepteur sint occaecat cupidatat non proident,
            sunt in culpa qui officia deserunt mollit anim id est laborum. At
            vero eos et accusamus et iusto odio dignissimos ducimus qui
            blanditiis praesentium voluptatum deleniti atque corrupti quos
            dolores et quas molestias excepturi sint occaecati cupiditate non
            provident, similique sunt in culpa qui officia deserunt mollitia
            animi, id est laborum et dolorum fuga. Et harum quidem rerum facilis
            est et expedita distinctio. Nam libero tempore, cum soluta nobis est
            eligendi optio cumque nihil impedit quo minus id quod maxime placeat
            facere possimus, omnis voluptas assumenda est, omnis dolor
            repellendus. Temporibus autem quibusdam et aut officiis debitis aut
            rerum necessitatibus saepe eveniet ut et voluptates repudiandae sint
            et molestiae non recusandae. Itaque earum rerum hic tenetur a
            sapiente delectus, ut aut reiciendis voluptatibus maiores alias
            consequatur aut perferendis doloribus asperiores repellat.
          </Text>
          <TextInput
            style={styles.textInput}
            placeholder="Enter your name"
            value={name}
            onChangeText={setName}
            autoCorrect={false}
            autoComplete="off"
          />
          <TextInput
            style={styles.textInput}
            placeholder="Enter your email"
            value={email}
            onChangeText={setEmail}
            autoCorrect={false}
            autoComplete="off"
          />
          <StatusBar style="dark" />
        </ScrollView>
      </SafeAreaView>
    </SafeAreaProvider>
  );
}
```

**Device check:** content is no longer hidden behind the notch.

> `SafeAreaProvider` sits at the root of the tree and calculates the safe-area insets for the device. `SafeAreaView` is a `View` that automatically applies those insets as padding. The two components must always be used together.

### Step 5: Fix the keyboard with `KeyboardAvoidingView`

`KeyboardAvoidingView` shifts or resizes the layout when the software keyboard appears, preventing it from covering the input.

The correct nesting order is:

```
SafeAreaProvider
  SafeAreaView
    KeyboardAvoidingView
      ScrollView
        (your content)
```

Update your imports and wrap the `ScrollView` in `KeyboardAvoidingView`:

```jsx
// App.js
import {
  StyleSheet,
  Image,
  Text,
  ScrollView,
  TextInput,
  KeyboardAvoidingView,
  Platform,
} from 'react-native';
import { StatusBar } from 'expo-status-bar';
import { useState } from 'react';
import { SafeAreaProvider, SafeAreaView } from 'react-native-safe-area-context';
import Header from './components/Header';
import SubHeader from './components/SubHeader';

export default function App() {
  const [name, setName] = useState('');
  const [email, setEmail] = useState('');

  return (
    <SafeAreaProvider>
      <SafeAreaView style={{ flex: 1, backgroundColor: '#fff' }}>
        <KeyboardAvoidingView
          behavior={Platform.OS === 'ios' ? 'padding' : undefined}
          style={{ flex: 1 }}
        >
          <ScrollView style={{ flex: 1 }} contentContainerStyle={styles.container}>
            <Image
              source={require('./assets/images/ntu-building.webp')}
              style={styles.bannerImg}
            />
            <Header title="AI Engineering Course" color="darkblue" />
            <Image
              source={{ uri: 'https://i.imgur.com/9wvRTDo.png' }}
              style={styles.image}
            />
            <SubHeader title="Sign Up Form" />
            <Text style={styles.mainText}>
              Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do
              eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut
              enim ad minim veniam, quis nostrud exercitation ullamco laboris
              nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in
              reprehenderit in voluptate velit esse cillum dolore eu fugiat
              nulla pariatur. Excepteur sint occaecat cupidatat non proident,
              sunt in culpa qui officia deserunt mollit anim id est laborum. At
              vero eos et accusamus et iusto odio dignissimos ducimus qui
              blanditiis praesentium voluptatum deleniti atque corrupti quos
              dolores et quas molestias excepturi sint occaecati cupiditate non
              provident, similique sunt in culpa qui officia deserunt mollitia
              animi, id est laborum et dolorum fuga. Et harum quidem rerum
              facilis est et expedita distinctio. Nam libero tempore, cum soluta
              nobis est eligendi optio cumque nihil impedit quo minus id quod
              maxime placeat facere possimus, omnis voluptas assumenda est,
              omnis dolor repellendus. Temporibus autem quibusdam et aut
              officiis debitis aut rerum necessitatibus saepe eveniet ut et
              voluptates repudiandae sint et molestiae non recusandae. Itaque
              earum rerum hic tenetur a sapiente delectus, ut aut reiciendis
              voluptatibus maiores alias consequatur aut perferendis doloribus
              asperiores repellat.
            </Text>
            <TextInput
              style={styles.textInput}
              placeholder="Enter your name"
              value={name}
              onChangeText={setName}
              autoCorrect={false}
              autoComplete="off"
            />
            <TextInput
              style={styles.textInput}
              placeholder="Enter your email"
              value={email}
              onChangeText={setEmail}
              autoCorrect={false}
              autoComplete="off"
            />
            <StatusBar style="dark" />
          </ScrollView>
        </KeyboardAvoidingView>
      </SafeAreaView>
    </SafeAreaProvider>
  );
}
```

The `behavior` prop controls how `KeyboardAvoidingView` responds when the keyboard appears. Expo's own guidance is to set it conditionally by platform:

- **iOS:** use `"padding"`. iOS does not resize the screen when the keyboard appears, so the view needs padding added at the bottom to push content up above it.
- **Android:** use `undefined` (no behavior at all). Android already resizes the window when the keyboard appears by default, so adding a `KeyboardAvoidingView` behavior on top of that tends to cause the exact double-resizing glitches you are trying to avoid.

`Platform.OS === 'ios' ? 'padding' : undefined` captures exactly this split, and is the pattern used in [Expo's keyboard handling guide](https://docs.expo.dev/guides/keyboard-handling/).

The `behavior` prop also accepts two other values, and Expo's guide encourages experimenting with them, since a different option can work better depending on your specific layout:

- `"height"`: reduces the height of the view instead of adding padding
- `"position"`: shifts the entire view's position upward; more aggressive, and can cause visible jumping

Try swapping `"padding"` for `"height"` on iOS and compare the feel; there is no single right answer for every screen.

**Device check:** tapping an input field no longer causes the keyboard to cover it.

> **Common mistake:** Placing `ScrollView` outside `KeyboardAvoidingView`. The scroll container must be inside the keyboard-avoiding wrapper so it can be resized when the keyboard appears. If you get this the wrong way around, the keyboard will still cover the input on iOS.

---

## Activity: Add a Phone Number Field

You now have a working sign-up form with a name and email input. On your own, add a third field for a phone number.

**Task:** Below the existing email `TextInput`, add a controlled `TextInput` for entering a phone number, following the same style as the name and email fields (no separate label; use a `placeholder` instead). When the field is focused, the device should show a numeric keypad rather than the full keyboard.

**Hints:**
1. You need a new piece of state, just like `name` and `email`
2. The `keyboardType` prop controls which keyboard appears; set it to `"phone-pad"`
3. The `maxLength` prop limits how many characters can be entered (for example, `8` for a Singapore number)
4. You can reuse the existing `styles.textInput` style; no new style is needed

<details>
<summary>Reference solution</summary>

Add state:

```jsx
// App.js
const [phone, setPhone] = useState('');
```

In the JSX, after the existing email `TextInput`:

```jsx
// App.js
<TextInput
  style={styles.textInput}
  placeholder="Enter your phone number"
  value={phone}
  onChangeText={setPhone}
  keyboardType="phone-pad"
  maxLength={8}
/>
```

**Device check:** tapping the phone field opens a numeric keypad, and typing stops after 8 digits.

</details>

---

## Part 4: The `Button` Component

### Step 1: Add a submit button

React Native ships a basic `Button` component for triggering an action. Add one below your phone number input to submit the form:

```jsx
// App.js
import { StyleSheet, Image, Text, ScrollView, TextInput, Button, KeyboardAvoidingView } from 'react-native';
```

```jsx
// App.js
<Button
  title="Submit"
  onPress={() => console.log({ name, email, phone })}
/>
```

**Device check:** a button labelled "Submit" appears below the phone number field. Tapping it logs the current form values to the terminal running `npx expo start`.

> **Common mistake:** Expecting to see the console output on the device screen. `console.log` output appears in the terminal where Metro is running, not on the phone or emulator.

### Step 2: The problem with `Button`

Try adding a `style` prop to your `Button`:

```jsx
// App.js
<Button
  title="Submit"
  onPress={() => console.log({ name, email, phone })}
  style={{ backgroundColor: 'darkblue', padding: 20 }}
/>
```

**Device check:** nothing changes. `Button` silently ignores the `style` prop.

`Button` only accepts a small, fixed set of props: `title`, `onPress`, `color`, and `disabled`. There is no way to customise its border, padding, font, or shape. Worse, the `color` prop behaves differently per platform: on iOS it changes the text color, and on Android it changes the background color. The same line of code produces two different-looking buttons.

For any custom appearance, you need to build your own button using `Pressable`, the base component React Native provides for building custom touchable elements.

Remove the `style` prop you just added; you will replace `Button` with your own component next.

### Step 3: Build a reusable `Button` component

Create a `components` folder in your project root, and inside it a file named `Button.js`:

```jsx
// components/Button.js
import { Pressable, Text, StyleSheet } from 'react-native';

export default function Button({ title, onPress, color = '#4263eb' }) {
  return (
    <Pressable
      onPress={onPress}
      style={({ pressed }) => [
        styles.button,
        { backgroundColor: color, opacity: pressed ? 0.8 : 1 },
      ]}
    >
      <Text style={styles.text}>{title}</Text>
    </Pressable>
  );
}

const styles = StyleSheet.create({
  button: {
    paddingVertical: 12,
    paddingHorizontal: 20,
    borderRadius: 8,
    alignItems: 'center',
  },
  text: {
    color: '#fff',
    fontWeight: '700',
  },
});
```

A few things to notice:

- `Pressable` accepts a normal `style` prop, so it works with `StyleSheet` like any other component
- The `style` prop can be a function that receives `{ pressed }`, letting you change the appearance while the button is being held down; here it dims the button to `opacity: 0.8`
- The `color` prop has a default value, so `<Button title="Submit" onPress={...} />` still works without specifying a color

Import your new component in `App.js` and replace the native `Button`:

```jsx
// App.js
import Button from './components/Button';
```

```jsx
// App.js
<Button title="Submit" onPress={() => console.log({ name, email, phone })} />
```

**Device check:** the submit button now has a rounded, filled background, and dims slightly when pressed.

> **Why does this look and feel more like a "real" button?** Most production apps never use the built-in `Button` component for exactly this reason. Building one custom `Pressable`-based button and reusing it everywhere keeps the appearance consistent across iOS and Android.

---

## Part 5: Flexbox Layout

For this section, create a separate Expo app. This keeps your `components-app` clean, and gives you a dedicated playground you can refer back to later when building more complex layouts.

```bash
npx create-expo-app --template blank learn-flex-app
cd learn-flex-app
npx expo start
```

Replace `App.js` with this starter code. The coloured boxes will make it easy to see how each Flexbox property affects the layout visually.

```jsx
// App.js
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
// App.js
item: {
  flex: 1,
  justifyContent: 'center',
  alignItems: 'center',
},
```

**Device check:** the three boxes now divide the screen equally into thirds.

You can use different `flex` values to create proportional splits. To give the first box three-fifths of the space and the others one-fifth each:

```jsx
// App.js
<View style={[styles.item, { backgroundColor: '#51cf66', flex: 3 }]}>
<View style={[styles.item, { backgroundColor: '#fcc419', flex: 1 }]}>
<View style={[styles.item, { backgroundColor: '#ff6b6b', flex: 1 }]}>
```

> **Why does `flex: 1` on the container matter?** Without `flex: 1` on the outer `View`, the container only takes as much height as its children require. Properties like `justifyContent` have no visible effect because there is no extra space to distribute. Almost every root layout container in a React Native app should have `flex: 1`.

### `flexDirection`

By default, `View` lays out its children in a **column** (top to bottom). This is the opposite of CSS Flexbox, which defaults to `"row"`.

Try changing the `container` style:

```jsx
// App.js
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
// App.js
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
// App.js
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
// App.js
<View style={[styles.item, { backgroundColor: '#51cf66', flex: 3 }]}>
```

You have been using this throughout this section, and it is the same pattern behind the `[styles.header, { color }]` merge inside your `Header` component from Part 1. It is also useful for conditional styles:

```jsx
<View style={[styles.button, isActive && styles.buttonActive]}>
```

---

## Activity (Optional): Build a Layout with Flex Ratios

If time allows, treat this as a stretch goal alongside the Bonus Challenges below.

Using only `View`, `Text`, `flexDirection`, `justifyContent`, `alignItems`, and `flex` ratios, build the following layout in `learn-flex-app`:

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
// App.js
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

In `components-app`, style the `TextInput` so that its border colour changes when the field is focused.

**Hints:**
- Use `onFocus` and `onBlur` props on `TextInput`
- Store a boolean `isFocused` state value
- Use style array merging to apply a different `borderColor` when `isFocused` is `true`

### Challenge 2: Card component

Extract the banner image, header, and sub-header at the top of the sign-up screen in `components-app` into a reusable `Card` component that accepts `imageSource`, `title`, and `subtitle` as props.

**Hints:**
- Create a `components/` folder and add `Card.js`
- The component should render a `View` wrapping an `Image`, a `Text` for the title, and a `Text` for the subtitle
- Import and use it in `App.js` in place of the existing banner image and headings

### Challenge 3: Three-column grid

In `learn-flex-app`, create a 3x2 grid of coloured boxes (three columns, two rows). Each box should be square and take equal width.

**Hints:**
- Use two row `View` containers, each with `flexDirection: 'row'`
- Each box inside a row should have `flex: 1`
- Set a fixed `height` on each row, or use `aspectRatio: 1` on each box

### Challenge 4: Responsive image width

In `components-app`, make the images fill the full width of the screen regardless of the device size.

**Hints:**
- Import `Dimensions` from `react-native`
- `const { width } = Dimensions.get('window')` gives you the screen width in pixels
- Apply `width` dynamically in the style: `{ width: width - 32, height: width - 32 }`

---

## Summary

- React Native provides its own set of primitive components instead of HTML elements. `View`, `Text`, `Image`, `TextInput`, `ScrollView`, and `Button` cover the majority of everyday mobile UI needs.
- All styling is JavaScript: camelCase property names, no units, no `className`. `StyleSheet.create()` is the preferred approach.
- The built-in `Button` component has no `style` prop and behaves differently per platform. `Pressable` is the base component for building custom, reusable touchables.
- Flexbox is the layout system in React Native. Key differences from CSS: `flexDirection` defaults to `"column"`, and `flex: 1` on the root container is required for alignment properties to take effect.
- Mobile-specific problems require dedicated solutions: `SafeAreaView` for the device notch, `KeyboardAvoidingView` for the software keyboard.

---

## Additional Resources

- [React Native: Core Components and APIs](https://reactnative.dev/docs/components-and-apis)
- [React Native: Style](https://reactnative.dev/docs/style)
- [React Native: Layout with Flexbox](https://reactnative.dev/docs/flexbox)
- [React Native: TextInput](https://reactnative.dev/docs/textinput)
- [React Native: Pressable](https://reactnative.dev/docs/pressable)
- [Expo: Safe Area Context](https://docs.expo.dev/versions/latest/sdk/safe-area-context/)
