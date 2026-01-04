---
description: Install npm dependencies
allowed-tools: Bash(npm install:*)
---

# Install Dependencies

Install all npm dependencies defined in `package.json`.

This will:
- Download all required packages
- Install dev dependencies
- Update `package-lock.json`
- Prepare the project for development

Run this when:
- First setting up the project
- After pulling new commits with dependency changes
- `node_modules/` was deleted
- Adding new dependencies

Time: Typically 2-5 minutes depending on internet speed.
