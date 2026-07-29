# 2.15 UI Composition and Layout Management in Mobile Applications

## Lesson Overview

This lesson builds on the React Native environment set up in Lesson 2.14. Learners explore the full set of core React Native components, learn how styling in JavaScript differs from CSS, and develop a working understanding of Flexbox layout for mobile screens. By the end, learners can compose a scrollable, keyboard-aware screen from scratch using `View`, `Text`, `Image`, `TextInput`, `ScrollView`, `Button`, `SafeAreaView`, and `KeyboardAvoidingView`.

## Dependencies

- [Self Studies](./studies.md)
- [Lesson](./lesson.md)
- [Assignment](./assignment.md)

## Lesson Objectives

- Use core React Native components (`View`, `Text`, `Image`, `TextInput`, `ScrollView`, `Button`) to compose a functional mobile screen
- Apply React Native styling with `StyleSheet.create()`, understanding how it differs from CSS, and solve mobile-specific layout problems with `SafeAreaView` and `KeyboardAvoidingView`
- Explain how Flexbox works in React Native, including the main axis, the cross axis, and the core layout properties
- Design mobile layouts using Flexbox, controlling axis direction, alignment, and proportional sizing

## Lesson Plan

| Duration | What | How or Why |
|---|---|---|
| 10 min | Welcome and recap | Briefly revisit Lesson 2.14: components, StyleSheet, Flexbox defaults; set context for today's deeper dive |
| 35 min | Lecture: UI Composition and Layout | Slides: core components, styling system, Flexbox model, SafeAreaView, KeyboardAvoidingView |
| 5 min | Break | |
| 20 min | Lab Part 1: Core components and styling | Code-along: `View`, `Text`, `StyleSheet.create()`, camelCase properties, border differences, extracting a `Header` component; Activity: build a `SubHeader` component |
| 15 min | Lab Part 2: Image component | Code-along: local image import, network image with explicit dimensions, common mistakes |
| 35 min | Lab Part 3: TextInput, ScrollView, SafeAreaView, KeyboardAvoidingView | Code-along: sign-up page content, ScrollView overflow fix, controlled TextInputs, notch problem and SafeAreaView fix, keyboard problem and KeyboardAvoidingView, nesting order; Activity: add a Notes field |
| 5 min | Break | |
| 20 min | Lab Part 4: Button component | Code-along: submit button, the problem with the built-in `Button`, building a reusable `Button` component |
| 20 min | Lab Part 5: Flexbox layout | Code-along: `flex` on children, flexDirection, main/cross axis, justifyContent, alignItems, style arrays; optional Activity: build a layout with flex ratios |
| 15 min | Wrap up and Q&A | Recap objectives, common pitfalls, preview Lesson 2.16 (Dogstagram coaching project) |
| **Total** | | **~180 min, allows ~5 min buffer for pacing; the flex-ratio activity is a stretch goal alongside the Bonus Challenges if time is short** |
