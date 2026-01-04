---
description: Create language index from language pack definitions
allowed-tools: Bash(npm run lang:create-langindex:*)
---

# Create Language Index

Generate the language index from all language pack definitions.

This will:
- Scan all language pack files
- Create a master index of available languages
- Generate `src/assets/lang/index.json`
- Build language metadata
- Enable language selection in the app

Use this after:
- Adding new languages
- Updating language packs
- Modifying language definitions

The index is used by the app to show available language options to users.
