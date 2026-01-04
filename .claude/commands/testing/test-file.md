---
description: Run tests for a specific file
argument-hint: [file-path]
allowed-tools: Bash(jest --testPathPattern:*)
---

# Run Tests for Specific File

Run tests only for a specific test file.

Usage: `/test-file src/core/services/ws.test.ts`

This will:
- Run only the specified test file
- Skip all other tests
- Show detailed results for that file
- Useful for focused testing

Argument should be the path to the `.test.ts` or `.spec.ts` file.

For all tests, use `/test`.
For watch mode, use `/test-watch`.
