---
author: Raman Adlakha
categories: note
hidden: "false"
image: ""
layout: post
tags:
- react-native
- typescript
- expo
- props
- flexbox
title: "React Native Fundamentals: Components, Props, and Styling"
---

In this session, we moved beyond "Hello World" and started building the actual UI for **InPact**. We explored the core building blocks of React Native and how they differ from the web.

## Core Concepts

### 1. Components vs HTML
There are no HTML tags in React Native.
*   **`<View>`**: The container. Replaces `<div>`.
*   **`<Text>`**: The only way to display text. Replaces `<span>`, `<p>`, `<h1>`, etc.

### 2. Styling with JavaScript
We don't use CSS files. We use the `StyleSheet` API to create style objects.
```typescript
const styles = StyleSheet.create({
  container: {
    flex: 1,
    backgroundColor: '#fff',
    alignItems: 'center',
    justifyContent: 'center',
  },
});
```
*   **Flexbox**: Layout is handled by Flexbox, which defaults to `flexDirection: 'column'` (vertical) in React Native, unlike `row` on the web.

## Hands-on: The Welcome Screen

We created our first custom component, `WelcomeScreen`, to understand **Props**.

### The Pattern: Data Flows Down
We defined a `WelcomeProps` interface to specify what data the component expects.

```typescript
interface WelcomeProps {
  appName: string;
}

const WelcomeScreen = ({ appName }: WelcomeProps) => {
  return (
    <View style={styles.container}>
      <Text style={styles.title}>Welcome to {appName}!</Text>
    </View>
  );
};
```

Then, in `App.tsx`, we passed the data down:
```typescript
<WelcomeScreen appName="InPact" />
```

### Key Takeaway
**"Data flows down, Actions flow up."**
We learned that while data (like `appName`) is passed down via props, behavior (like what happens on a button press) is defined in the parent and passed down as a function prop to be executed by the child.

Next up: **User Onboarding & Authentication**.
