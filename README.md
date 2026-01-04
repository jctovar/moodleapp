# Moodle App

This is the primary repository of source code for the official mobile application for Moodle LMS. The Moodle App is a hybrid mobile application built with Angular and Ionic, providing a native-like experience on both iOS and Android platforms.

## Table of Contents

- [About](#about)
- [Key Features](#key-features)
- [Prerequisites](#prerequisites)
- [Installation](#installation)
- [Quick Start](#quick-start)
- [Development](#development)
- [Testing](#testing)
- [Building for Production](#building-for-production)
- [Project Structure](#project-structure)
- [Resources](#resources)
- [License](#license)

## About

The Moodle App is the official mobile application for Moodle LMS, enabling users to access their courses, assignments, and learning materials on the go. This project is built with:

- **Angular 20** - Modern web application framework
- **Ionic 8.7** - Cross-platform mobile UI framework
- **Cordova** - Framework for building native mobile apps with web technologies
- **TypeScript** - Typed superset of JavaScript

**Current Version**: 5.1.0

## Key Features

- Offline-first architecture for seamless offline functionality
- Multi-site support for managing multiple Moodle instances
- Native mobile apps for iOS and Android
- Real-time synchronization and push notifications
- Complete course and assignment management
- File handling and media viewing capabilities
- Comprehensive question engine for quizzes
- Message and notification system

## Prerequisites

Before you begin development, ensure you have the following tools installed on your system:

### Core Requirements

- **Node.js 22 LTS** - JavaScript runtime (includes npm)
- **npm** - Node package manager (included with Node.js)
- **Git** - Version control system

### For Web Development

- **Ionic CLI 7+** - Command-line interface for Ionic framework
- **Angular CLI 20+** - Command-line interface for Angular

### For Native Mobile Development

#### iOS Development (macOS only)

- **Xcode 14+** - Apple's integrated development environment
- **CocoaPods** - Dependency manager for iOS projects (usually installed with Xcode)
- **iOS SDK** - Part of Xcode installation

#### Android Development

- **Android Studio** - Official IDE for Android development
- **Android SDK** - Minimum API level 24 (Android 7.0) or higher
- **Java Development Kit (JDK)** - Version 11 or higher

### Optional

- **BrowserStack** - For testing on real devices (used in CI/CD pipeline)

## Installation

### Step 1: Clone the Repository

```bash
git clone https://github.com/moodlehq/moodleapp.git
cd moodleapp
```

### Step 2: Install Node.js Dependencies

```bash
npm install
```

This command installs all dependencies defined in `package.json`, including:

- Ionic and Angular frameworks
- Build tools and task runners
- Testing frameworks and utilities
- Linting tools

### Step 3: Verify Installation

To verify your installation is correct, run:

```bash
npm start
```

This will start the development server. You should see output similar to:

```bash
> moodleapp@5.1.0 start
> ionic serve

[OK] Server ready: http://localhost:8100/
```

## Quick Start

### Start Development Server

Launch the local development server with live reload:

```bash
npm start
```

Then open your browser and navigate to `http://localhost:8100`

### Development Build

Create a development build with source maps for debugging:

```bash
npm run build
```

### Production Build

Create an optimized production build:

```bash
npm run build:prod
```

### Check Code Quality

Run ESLint to identify and fix code style issues:

```bash
npm run lint
```

### Common Development Tasks

| Command | Description |
| --- | --- |
| `npm start` | Start development server (browser-based) |
| `npm run build` | Build for development |
| `npm run build:prod` | Build for production |
| `npm run lint` | Run ESLint on TypeScript and HTML files |
| `npm test` | Run all unit tests |
| `npm run test:watch` | Run tests in watch mode |
| `npm run test:coverage` | Generate test coverage reports |

## Development

### Running on Physical Devices

#### Development on Android

Start a development build with live reload on an Android device:

```bash
npm run dev:android
```

Build and deploy to an Android device for production testing:

```bash
npm run prod:android
```

#### Development on iOS

Start a development build with live reload on an iOS device (macOS only):

```bash
npm run dev:ios
```

Build and deploy to an iOS device for production testing (macOS only):

```bash
npm run prod:ios
```

### Code Organization

The project follows a modular architecture:

- **`src/app/`** - Root component and application-level routing
- **`src/core/`** - Core services, components, and features used across the application
- **`src/addons/`** - Feature-specific addon modules (courses, quizzes, forums, etc.)
- **`src/theme/`** - Theming system, SCSS styles, and animations
- **`src/assets/`** - Static resources, fonts, images, and language files
- **`src/testing/`** - Jest setup and mock services

### Important Notes

All npm scripts automatically run Gulp tasks before building. Gulp handles:

- Language compilation (compiles language strings into per-language JSON files)
- Environment configuration generation
- Icon metadata building
- Behat test plugin generation (if configured)

To manually run Gulp tasks:

```bash
# Run Gulp once with testing environment
NODE_ENV=testing gulp

# Watch Gulp files for changes
gulp watch
```

## Testing

### Run All Tests

Execute all unit tests once:

```bash
npm test
```

### Watch Mode

Run tests in watch mode for continuous testing during development:

```bash
npm run test:watch
```

### Coverage Reports

Generate detailed test coverage reports:

```bash
npm run test:coverage
```

### Run Specific Test File

Run tests for a specific file or directory:

```bash
jest --testPathPattern=path/to/file
```

### Test Structure

- Tests are colocated with source files: `component.ts` and `component.test.ts` in the same directory
- Tests use Jest with `jest-preset-angular`
- Test utilities are available in `src/testing/`
- Mock services and stubs are provided for testing

### End-to-End Testing

The app includes Behat integration for E2E testing:

```bash
# Watch Behat tests during development
gulp watch-behat
```

Behat feature files are located in `src/**/*.feature` directories and run against a Moodle server.

## Building for Production

### Prerequisites for Production Build

Ensure you have completed all steps in the [Installation](#installation) section and have the appropriate platform SDKs installed.

### Production Build Process

1. **Build the application**

   ```bash
   npm run build:prod
   ```

2. **Deploy to device/store**

   For Android:

   ```bash
   npm run prod:android
   ```

   For iOS (macOS):

   ```bash
   npm run prod:ios
   ```

3. **Follow platform-specific guidelines**

   - For Google Play Store: Follow Android app publishing guidelines
   - For Apple App Store: Follow iOS app publishing guidelines

## Project Structure

### Core Services

The `src/core/services/` directory contains approximately 32 core services providing:

- Web service communication
- Site and authentication management
- File system operations and caching
- Database access and synchronization
- Navigation management
- UI overlays (alerts, toasts, modals, etc.)
- Network detection and offline support
- Analytics and QR scanning

### Reusable Components

The `src/core/components/` directory provides 46+ reusable components including:

- Navigation components (navbar, tabs, split-view)
- File handling components
- Form inputs and selectors
- Content display components (loading states, empty states, charts)
- File upload and download components

### Addons and Features

The `src/addons/` directory contains approximately 169 optional feature modules:

- **Activity modules** - 25 different activity types (assign, quiz, forum, chat, lesson, scorm, wiki, etc.)
- **Question types** - Question rendering for various formats
- **Question behaviors** - Question interaction logic
- **Other addons** - Messages, calendar, badges, competency, blog, and more

## Resources

### Official Documentation

- [User Documentation](https://docs.moodle.org/en/Moodle_app) - End-user guide for the Moodle App
- [Developer Documentation](https://moodledev.io/general/app) - Comprehensive developer guide
- [Development Setup Guide](https://moodledev.io/general/app/development/setup) - Detailed setup instructions
- [Release Notes](https://moodledev.io/general/app_releases) - Version history and changes

### Project Resources

- [Bug Tracker](https://moodle.atlassian.net/browse/MOBILE) - Report and track issues
- [GitHub Repository](https://github.com/moodlehq/moodleapp) - Source code and pull requests
- [Contributing Guidelines](https://github.com/moodlehq/moodleapp/blob/main/.github/CONTRIBUTING.md) - How to contribute
- [Security Policy](https://github.com/moodlehq/moodleapp/blob/main/.github/SECURITY.md) - Security reporting

### Testing Infrastructure

This project is tested with [BrowserStack](https://www.browserstack.com/) for compatibility across real devices and browsers.

## License

This project is licensed under the [Apache License 2.0](http://www.apache.org/licenses/LICENSE-2.0). You are free to use, modify, and distribute this software in accordance with the license terms.

---

For questions or support, please refer to the [Developer Documentation](https://moodledev.io/general/app) or open an issue on the [GitHub repository](https://github.com/moodlehq/moodleapp).
