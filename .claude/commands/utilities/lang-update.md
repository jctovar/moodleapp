---
description: Update language packs from Moodle strings
allowed-tools: Bash(npm run lang:update-langpacks:*)
---

# Update Language Packs

Update and compile language packs from Moodle strings.

This will:
- Fetch the latest language strings from Moodle
- Compile language strings from `src/core` and `src/addons`
- Generate per-language JSON files in `src/assets/lang/`
- Update translation files
- Prepare strings for localization

Use this when:
- Adding new translatable strings to the app
- Updating language definitions
- Syncing with Moodle's latest strings
