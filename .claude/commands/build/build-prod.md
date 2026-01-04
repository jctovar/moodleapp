---
description: Production build with optimizations and minification
allowed-tools: Bash(npm run build:prod:*)
---

# Production Build

Create an optimized production build with minification and tree-shaking.

This will:
- Minify JavaScript and CSS
- Remove unused code (tree-shaking)
- Disable source maps
- Optimize bundle size (target <20MB)
- Enable aggressive optimizations
- Production-ready output

The build output will be in the `www/` directory.

This build is used for:
- App Store releases
- Google Play releases
- Final deployments

For development builds, use `/build`.
