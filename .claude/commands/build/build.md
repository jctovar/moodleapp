---
description: Development build with source maps and fast compilation
allowed-tools: Bash(npm run build:*)
---

# Development Build

Create a development build with source maps enabled for debugging.

This will:
- Compile TypeScript with source maps
- Build Angular project in development mode
- Enable debugging tools
- Faster build time than production
- Suitable for testing in browsers

The build output will be in the `www/` directory.

For production builds, use `/build-prod`.
