---
description: Generate code coverage report for all tests
allowed-tools: Bash(npm run test:coverage:*)
---

# Generate Test Coverage Report

Run all tests and generate a detailed code coverage report.

This will:
- Execute all tests
- Calculate coverage metrics for:
  - Line coverage
  - Branch coverage
  - Function coverage
  - Statement coverage
- Generate HTML report in `coverage/` directory
- Show coverage summary in terminal

Coverage reports show which parts of your code are tested and which aren't.

Open `coverage/index.html` in a browser to view the detailed report.
