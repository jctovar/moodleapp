---
description: Production build for Android ready for App Store release
allowed-tools: Bash(npm run prod:android:*)
---

# Production Build for Android

Create an optimized production build for Android with full optimization and signing.

Requirements:
- Android SDK properly configured
- Signing keys configured for release

This will:
- Compile with production optimizations
- Minify code and resources
- Generate production build artifact (APK/AAB)
- Ready for Google Play submission
- Optimized for performance

The build output will be in `platforms/android/app/build/outputs/`.

For development Android builds, use `/dev-android`.
For iOS production builds, use `/prod-ios`.
