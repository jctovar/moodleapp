---
description: Detect available language packs in the project
allowed-tools: Bash(npm run lang:detect-langpacks:*)
---

# Detect Language Packs

Scan and detect all available language packs in the project.

This will:
- Scan `src/core` and `src/addons` for language definitions
- List all available languages
- Detect missing translations
- Report language pack status
- Show which languages are supported

Use this to understand what languages the app supports and identify missing translations.
