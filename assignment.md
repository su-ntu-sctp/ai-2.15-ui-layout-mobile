# Optional Assignment: Contact Profile Screen

## Overview

- **Lesson:** UI Composition and Layout Management in Mobile Applications / 2.15
- **Type:** Optional Take-Home Assignment
- **Estimated Time:** 1.5–2.5 hours
- **Due:** Before next lesson
- **Submission:** GitHub repository link or ZIP file

## Learning Objectives Covered

This assignment reinforces:

- Composing a mobile screen from core React Native components (`View`, `Text`, `Image`, `TextInput`, `ScrollView`)
- Applying `StyleSheet.create()` with camelCase properties and Flexbox layout
- Handling the device notch and software keyboard correctly with `SafeAreaView` and `KeyboardAvoidingView`
- Designing proportional layouts using `flex` ratios

## Assignment Description

Build a **Contact Profile Screen** for a simple mobile contacts app. The screen displays a contact's details and lets the user add a personal note about that contact. The focus is on composing a polished, scrollable layout using the components and styling techniques from the lesson.

### What You Will Build

A single-screen app that displays:

- A profile header with an avatar image and the contact's name
- A details section showing a phone number, email address, and location
- A notes section with a labelled `TextInput` where the user can type a personal note
- A character counter below the notes field showing how many characters have been entered

## Requirements

### Core Requirements

#### 1. Project Setup

- [ ] Create a new Expo project: `npx create-expo-app --template blank contact-profile`
- [ ] Install the safe area library: `npx expo install react-native-safe-area-context`
- [ ] Confirm the app runs on your device via Expo Go or on the Android emulator

#### 2. Profile Header

Build a header section at the top of the screen:

- [ ] Display a circular avatar image (use any square image from the web via `source={{ uri: '...' }}`; set explicit `width` and `height` of `100`, and `borderRadius` of `50` to make it circular)
- [ ] Display the contact's name in large, bold text below the avatar
- [ ] Display a one-line subtitle such as a job title or relationship label below the name
- [ ] Centre everything in the header horizontally

#### 3. Details Section

Below the header, display three rows of information. Each row should contain a label and a value:

- [ ] Phone: label "Phone" and a value such as "+65 9123 4567"
- [ ] Email: label "Email" and a value such as "alex@example.com"
- [ ] Location: label "Location" and a value such as "Singapore"
- [ ] Each row should use `flexDirection: 'row'` with the label on the left and the value on the right
- [ ] Use `justifyContent: 'space-between'` to push the label and value apart

#### 4. Notes Section

- [ ] Add a `Text` label reading "Notes"
- [ ] Add a controlled `TextInput` (with `useState`) for entering notes; make it multi-line with `textAlignVertical: 'top'`
- [ ] Below the input, display a character counter: for example, "12 / 200 characters"
- [ ] Limit the input to 200 characters using the `maxLength` prop

#### 5. Layout and Keyboard Handling

- [ ] Wrap the entire screen in `SafeAreaProvider` and `SafeAreaView` so content is not hidden by the notch
- [ ] Wrap the scrollable content in `KeyboardAvoidingView` and `ScrollView` so the notes field is not covered by the keyboard
- [ ] Use `StyleSheet.create()` for all styles; no inline style objects except where a value must be computed dynamically

### Starter Structure

```jsx
import { useState } from 'react';
import {
  StyleSheet,
  Text,
  View,
  Image,
  TextInput,
  ScrollView,
  KeyboardAvoidingView,
} from 'react-native';
import { SafeAreaProvider, SafeAreaView } from 'react-native-safe-area-context';

const AVATAR_URL = 'https://i.pravatar.cc/200';
const MAX_NOTE_LENGTH = 200;

export default function App() {
  const [notes, setNotes] = useState('');

  return (
    <SafeAreaProvider>
      <SafeAreaView style={styles.safeArea}>
        <KeyboardAvoidingView behavior="padding" style={{ flex: 1 }}>
          <ScrollView>
            {/* Profile header goes here */}
            {/* Details section goes here */}
            {/* Notes section goes here */}
          </ScrollView>
        </KeyboardAvoidingView>
      </SafeAreaView>
    </SafeAreaProvider>
  );
}

const styles = StyleSheet.create({
  safeArea: {
    flex: 1,
    backgroundColor: '#f8f9fa',
  },
  // add more styles here
});
```

## Bonus Challenges

### Easy

- [ ] Add a divider line between the details section and the notes section. Use a `View` with a small fixed `height` (for example, `1`) and a `backgroundColor` to create the appearance of a horizontal rule.
- [ ] Change the notes field border colour to a highlight colour when the field is focused. Use `onFocus` and `onBlur` with a boolean state value, and apply styles using a style array.

### Medium

- [ ] Add a "Clear Notes" button below the character counter. Tapping it should reset the `notes` state to an empty string. Show the button only when `notes.length > 0`.
- [ ] Add a second tab-like section to the screen by rendering two `Text` buttons ("Details" and "Notes") in a `flexDirection: 'row'` row at the top of the content area. Use a boolean state to toggle which section is visible below.

### Hard

- [ ] Extract the profile header, details section, and notes section each into their own component file in a `components/` folder. Pass the necessary data as props.
- [ ] Make the details rows tappable. Import `Pressable` from `react-native`, wrap each row in a `Pressable`, and show an `Alert` when a row is pressed (for example, "Call +65 9123 4567?").

## Resources

- [React Native: Core Components and APIs](https://reactnative.dev/docs/components-and-apis)
- [React Native: TextInput](https://reactnative.dev/docs/textinput)
- [React Native: Layout with Flexbox](https://reactnative.dev/docs/flexbox)
- [Expo: Safe Area Context](https://docs.expo.dev/versions/latest/sdk/safe-area-context/)
