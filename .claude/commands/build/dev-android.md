---
description: Development build for Android with live reload
allowed-tools: Bash(npm run dev:android:*)
---

# Development Build for Android

Build and deploy the app to Android device/emulator with live reload.

Requirements:
- Android device connected via USB with USB debugging enabled
- OR Android emulator running
- ANDROID_HOME environment variable set

This will:
- Compile the app for Android
- Deploy to connected device/emulator
- Watch for file changes
- Live reload on save
- Great for testing on real devices

Use this for Android-specific testing and development.

For production Android builds, use `/prod-android`.
For iOS development, use `/dev-ios`.
