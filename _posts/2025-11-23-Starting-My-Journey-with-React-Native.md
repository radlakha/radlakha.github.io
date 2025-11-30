---
author: Raman Adlakha
categories: note
image: "pigeon-202509.jpg"
layout: post
tags:
- react-native
- expo
- typescript
title: Starting My Journey with React Native
---

Today marks the beginning of a new learning adventure. I am diving into **Mobile App Development** using **React Native** and **TypeScript**, with **Expo** as the framework of choice.

## The Goal
I am building a social mobile app called **InPact**. The goal is not just to build an app, but to master the concepts of mobile architecture, strong typing with TypeScript, and the React Native ecosystem.

## The Setup
We started by initializing the project using Expo.
```bash
npx create-expo-app@latest InPact -t expo-template-blank-typescript
```

I chose the **blank TypeScript template** to start from scratch and understand every piece of the puzzle.

## Key Learnings
Coming from a web background, the biggest shift is in the UI primitives:
*   Instead of `<div>`, we use `<View>`.
*   Instead of `<span>` or `<p>`, we use `<Text>`.
*   There is no CSS file (yet); we use `StyleSheet` in JavaScript.

## First Steps
I explored the project structure:
*   `App.tsx`: The entry point.
*   `app.json`: The configuration.

And I successfully modified the app to say "Hello InPact!".

Stay tuned for the next session where we will tackle **User Onboarding** and **Authentication**.
