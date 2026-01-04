---
description: Development build for iOS with live reload
allowed-tools: Bash(npm run dev:ios:*)
---

# Development Build for iOS

Build and deploy the app to iOS device/simulator with live reload.

Requirements:
- macOS with Xcode installed
- iOS device connected via USB
- OR iOS simulator running

This will:
- Compile the app for iOS
- Deploy to connected device/simulator
- Watch for file changes
- Live reload on save
- Great for testing on real devices

Use this for iOS-specific testing and development.

For production iOS builds, use `/prod-ios`.
For Android development, use `/dev-android`.
