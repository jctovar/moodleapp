---
description: Watch for file changes and rebuild automatically with gulp
allowed-tools: Bash(gulp watch:*)
---

# Watch for Changes

Watch all source files and automatically rebuild when changes are detected.

This will:
- Run gulp watch task
- Monitor all TypeScript, HTML, and style files
- Automatically compile on changes
- Compile language strings
- Generate build metadata
- Ready for immediate testing

This is useful when:
- Making multiple changes
- Testing build output
- Developing features
- Debugging build issues

The app doesn't auto-reload in this mode; refresh manually to see changes.

For automatic reload, use `/start` (dev server with auto-reload).
