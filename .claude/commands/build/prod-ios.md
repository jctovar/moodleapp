---
description: Production build for iOS ready for App Store release
allowed-tools: Bash(npm run prod:ios:*)
---

# Production Build for iOS

Create an optimized production build for iOS with full optimization and signing.

Requirements:
- macOS with Xcode installed
- Apple Developer account
- Signing certificates and provisioning profiles configured

This will:
- Compile with production optimizations
- Minify code and resources
- Generate production build artifact (IPA)
- Ready for App Store submission
- Optimized for performance and device storage

The build output will be in `platforms/ios/build/`.

For development iOS builds, use `/dev-ios`.
For Android production builds, use `/prod-android`.
