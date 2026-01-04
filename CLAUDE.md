# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

The Moodle App is the official mobile application for Moodle LMS, built with Angular 20 and Ionic 8.7. It runs on iOS and Android via Cordova as a hybrid mobile app. Version 5.1.0 requires Node.js 22 LTS.

- **Repository**: https://github.com/moodlehq/moodleapp
- **Developer Docs**: https://moodledev.io/general/app
- **Bug Tracker**: https://moodle.atlassian.net/browse/MOBILE

## Common Development Commands

### Core Development
```bash
npm start                    # Start dev server (default: http://localhost:8100)
npm run build               # Development build
npm run build:prod          # Production build
npm run lint                # Run ESLint on src/**/*.ts and src/**/*.html
```

### Testing
```bash
npm test                    # Run all tests once
npm run test:watch          # Run tests in watch mode
npm run test:coverage       # Generate coverage reports
npm run test:ci              # CI test runner (single run, verbose)
```

### Running on Devices
```bash
npm run dev:android         # Development build for Android with live reload
npm run dev:ios             # Development build for iOS
npm run prod:android        # Production build for Android
npm run prod:ios            # Production build for iOS
```

### Build Tasks
```bash
npm run lang:update-langpacks       # Update language packs
npm run lang:detect-langpacks       # Detect available language packs
npm run lang:create-langindex       # Create language index
```

### Build Pre-requisites
All npm scripts automatically run `gulp` tasks before building. Gulp tasks handle:
- **lang**: Compiles language strings from `src/core` and `src/addons` into per-language JSON files
- **env**: Generates environment configuration based on `NODE_ENV`
- **icons**: Builds icon metadata
- **behat**: Generates Behat test plugin (if configured)

To manually run: `NODE_ENV=testing gulp` or `gulp watch` for development

## Project Architecture

### Main Directory Structure

```
src/
├── app/                 # Root component and app-level routing
├── core/                # Core services, components, and features used across app
├── addons/              # Feature-specific addon modules (courses, quizzes, forums, etc.)
├── theme/               # Theming system (SCSS, animations, Ionic customization)
├── types/               # TypeScript definitions for external libraries
├── assets/              # Static resources (fonts, images, language files)
└── testing/             # Jest setup and mock services
```

### Core (`src/core`)

The largest part of the codebase. Provides fundamental functionality used by all features:

**Key Subdirectories**:
- **`services/`** (~32 services) - Core business logic
  - Web service communication (`ws.ts`)
  - Site/authentication management (`sites.ts`)
  - File system operations (`file.ts`, `filepool.ts`)
  - Navigation (`navigator.ts`)
  - Database access (`db.ts`)
  - UI overlays (alerts, toasts, modals, popovers, loading indicators)
  - Caching, sync, network detection, QR scanning, analytics
  - Each service provides delegation points via handlers

- **`components/`** (~46 reusable components) - Common UI building blocks
  - Navigation (navbar, tabs, split-view)
  - File handling (attachments, file display)
  - Forms (inputs, password modals, selectors)
  - Content display (loading states, empty states, charts, avatars)
  - File operations (upload, download, refresh)

- **`features/`** (~35 major features) - Core functionality modules
  - `login/` - Authentication, SSO, session management
  - `mainmenu/` - App navigation structure
  - `courses/` - Course browsing
  - `course/` - Course content and sections
  - `compile/` - Dynamic content rendering and DOM compilation
  - `question/` - Question rendering engine
  - `grades/` - Grade display
  - `comments/` - Comment system
  - `messages/` - Messaging system
  - `contentlinks/` - Dynamic content link handling
  - `editor/` - Content editor integration
  - `h5p/` - H5P content viewer
  - `viewer/` - File viewer
  - `settings/` - App settings
  - `push/` - Push notifications
  - Plus others: filter, rating, enrol, block, search, sitehome, etc.

- **`classes/`** - Reusable base classes and utilities
  - Database classes (eager, lazy, proxy, debug variants)
  - Error classes
  - Element controllers
  - Items management (pagination, infinite loading)

- **`directives/`** - Custom Angular directives for DOM manipulation and behavior

- **`pipes/`** - Angular pipes for data transformation

- **`guards/`** - Route guards for authentication and permissions

- **`singletons/`** - Logger, subscriptions manager, utilities

- **`initializers/`** - Startup and initialization logic

### Addons (`src/addons`)

Optional feature modules (~169 addons) extending core functionality. Organized by Moodle features:

**Structure**:
- **Activity modules** (`mod/`) - 25 activity types (assign, quiz, forum, chat, lesson, scorm, wiki, book, glossary, data, workshop, etc.)
- **Question types** (`qtype/`) - Question rendering for different formats (essay, multichoice, shortanswer, truefalse, numerical, match, etc.)
- **Question behaviors** (`qbehaviour/`) - Question interaction logic
- **Other addons** - Messages, calendar, badges, competency, blog, notes, notifications, private files, reports, etc.

**Typical Addon Structure**:
```
addon-name/
├── services/       # Business logic, database operations
├── pages/          # Page components
├── components/     # Feature-specific components
├── classes/        # Local classes and types
├── tests/          # Unit tests
└── addon-name.module.ts
```

Each addon registers handlers with core delegates to extend functionality without modifying core code.

## Architecture Patterns

### Delegation Pattern
Core services expose delegation points through `Delegate` classes. Addons register handlers:
```typescript
// Core defines a delegate
export class CustomDelegate { }

// Addon registers a handler
CoreDelegate.registerHandler(MyFeatureHandler.instance);
```

This allows addons to extend functionality without modifying core.

### Lazy Loading
- Features and addons are lazy-loaded via Angular routes
- Routes use dynamic imports with `loadChildren`
- Reduces initial bundle size

### Multi-Site Support
- `SitesService` manages multiple Moodle site connections
- Each site has separate database and cache
- Users can switch between sites

### Offline-First Architecture
- SQLite database (`cordova-sqlite-storage`) caches all data
- `FilePoolService` manages downloaded files with expiration
- `SyncService` handles background synchronization
- App remains functional offline for downloaded content

### Standalone Components
The app is migrating to Angular standalone components. New code should use:
```typescript
@Component({
  selector: 'app-my-component',
  standalone: true,
  imports: [CommonModule],
  template: '...',
})
export class MyComponent { }
```

## Testing

### Test Structure
- Tests use Jest with `jest-preset-angular`
- Tests colocate with source: `component.ts` and `component.test.ts` in same directory
- Setup file: `src/testing/setup.ts`

### Running Tests
```bash
npm test                              # Run all tests
npm run test:watch                    # Watch mode
npm run test:coverage                 # Coverage reports
jest --testPathPattern=path/to/file   # Single test file
```

### Test Utilities
- `src/testing/utils.ts` - Helper functions for component testing
- `src/testing/services/` - Mock services
- `src/testing/stubs/` - Service stubs

### Behat E2E Tests
The app includes Behat integration for end-to-end testing:
- Feature files in `src/**/*.feature` directories
- Behat tests run against a Moodle server
- Configure via `.behat.yml` and `moodle.config.json`
- Run with `gulp watch-behat` during development
- Snapshots stored in `src/**/tests/behat/snapshots/`

## Code Organization Principles

### Modular Design
- Each feature is self-contained
- Services handle business logic
- Components handle presentation
- Clear separation of concerns

### Reusable Components
The core provides 46+ reusable components for common UI patterns. Check `src/core/components/` before building custom solutions.

### Handlers and Delegates
Use the delegation pattern for extensibility:
- Define interfaces for handlers
- Create a delegate service
- Addons register handlers
- Core calls appropriate handlers

### Dependency Injection
Use Angular's dependency injection throughout:
```typescript
constructor(private service: CoreService) { }
```

### Path Aliases
TypeScript paths are configured for clean imports:
- `@/` → `src/`
- `@addons/` → `src/addons/`
- `@classes/` → `src/core/classes/`
- `@components/` → `src/core/components/`
- `@services/` → `src/core/services/`
- etc. (see `tsconfig.json`)

## File Organization

### Naming Conventions
- Components: `component-name.ts` (dash-separated)
- Services: `service-name.service.ts`
- Modules: `feature-name.module.ts`
- Tests: `feature-name.test.ts`
- Handlers: `handler-name.handler.ts`

### File Structure
Place files near their usage:
- Components in `components/` subdirectory
- Services in `services/` subdirectory
- Pages in `pages/` subdirectory
- Tests colocated with source files

## Configuration

### Environment Configuration
Environment-specific settings via `moodle.config.json`:
```json
{
  "app_id": "com.moodle.moodlemobile",
  "app_name": "Moodle",
  "filemanager": "moodle",
  "wsservice": "moodle_mobile_app"
}
```

### Ionic Configuration
`ionic.config.json` configures the Ionic framework

### Build Configuration
`angular.json` defines build configurations:
- `development` - Fast rebuilds, source maps enabled
- `production` - Optimized, source maps disabled
- `testing` - Test configuration

## Performance Considerations

### Bundle Size
- Production builds target <20MB limit (see `angular.json` budgets)
- Lazy loading keeps initial bundle small
- Tree-shaking removes unused code

### Offline Performance
- Data cached in SQLite
- Files cached with expiration strategy
- Network requests queued for later sync

### Memory Management
- Unsubscribe from observables in `ngOnDestroy`
- Use `takeUntil` pattern for subscription cleanup
- Monitor for memory leaks in development

### Database Performance
- Use appropriate query methods for data size
- Index frequently-queried fields
- Lazy-load database tables on demand

## Debugging

### Browser DevTools
```bash
npm start  # Start dev server
# Open http://localhost:8100 in Chrome
# Use Chrome DevTools for debugging
```

### Logging
Use the logger service for debug output:
```typescript
CoreLogger.debug('message');
CoreLogger.info('message');
CoreLogger.warn('message');
CoreLogger.error('message');
```

### Behat Tests
- Run `gulp watch-behat` to rebuild Behat plugin on file changes
- Snapshots help compare expected vs actual output
- Use `@wip` tag for work-in-progress tests

## Git Workflow

- Main branch: `main` (stable releases)
- Development branch: `main` (continuous development)
- Feature branches: Use descriptive names
- Pull requests require passing tests and linting

## Related Resources

- **Setup Guide**: https://moodledev.io/general/app/development/setup
- **Development Guide**: https://moodledev.io/general/app/development
- **App Releases**: https://moodledev.io/general/app_releases
- **Contributing**: https://github.com/moodlehq/moodleapp/blob/main/.github/CONTRIBUTING.md
- **Security**: https://github.com/moodlehq/moodleapp/blob/main/.github/SECURITY.md
