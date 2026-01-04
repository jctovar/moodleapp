---
description: Watch for changes and rebuild Behat test plugin
allowed-tools: Bash(gulp watch-behat:*)
---

# Watch Behat Tests

Watch for file changes and rebuild the Behat test plugin automatically.

Requirements:
- `.behat.yml` configured
- `moodle.config.json` configured
- Moodle server available for testing

This will:
- Monitor Behat feature files
- Auto-rebuild test plugin on changes
- Watch for snapshot updates
- Compile Behat specifications

Use this when:
- Developing new Behat scenarios
- Fixing end-to-end tests
- Testing against a Moodle server
- Building test snapshots

Behat tests in `src/**/*.feature` directories will be compiled and available.
