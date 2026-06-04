# Agents.md - AI Agent Instructions for Acode

## Project Overview

Acode is a powerful, open-source code editor designed for Android devices. It provides features like syntax highlighting, code completion, terminal integration, file management, and plugin support. The project is built using Apache Cordova for Android, with a modern JavaScript/TypeScript frontend using CodeMirror 6.

**Repository**: https://github.com/Acode-Foundation/Acode
**License**: MIT
**Current Version**: 1.12.2 (versionCode 972)

---

## Technologies

### Core Stack
- **Language**: JavaScript (ES2020+) with TypeScript for type checking
- **UI Framework**: Custom components using `html-tag-js` for DOM manipulation
- **Editor Engine**: CodeMirror 6 (replacing legacy Ace editor)
- **Mobile Framework**: Apache Cordova (Android target SDK 36)
- **Package Manager**: pnpm (preferred) or bun
- **Build System**: Rspack (primary) with Webpack fallback
- **Linting/Formatting**: Biome 2.4.11 (replaces ESLint + Prettier)

### Key Dependencies
- **CodeMirror 6**: Editor core with language support for 20+ languages
- **xterm.js**: Terminal emulation
- **marked/markdown-it**: Markdown rendering with plugins
- **mermaid**: Diagram rendering in markdown
- **DOMPurify**: HTML sanitization
- **dayjs**: Date manipulation
- **JSZip**: File compression
- **Mustache**: Template rendering

### Development Tools
- **Java JDK**: 21 (for Android builds)
- **Android SDK**: API 36
- **Gradle**: 8.x
- **Node.js**: 18+ (22 recommended)
- **Docker/DevContainers**: For consistent development environments

---

## Project Structure

```
Acode/
├── src/                    # Core application source code
│   ├── cm/                 # CodeMirror 6 extensions and configurations
│   │   ├── lsp/           # Language Server Protocol client
│   │   ├── modes/         # Custom editor modes
│   │   └── themes/        # Editor themes
│   ├── components/        # Reusable UI components
│   ├── dialogs/           # Dialog/modal components
│   ├── fileSystem/        # File system abstraction layer
│   ├── handlers/          # Event handlers (keyboard, intents, etc.)
│   ├── lang/              # Internationalization (i18n) files
│   ├── lib/               # Core library modules
│   ├── pages/             # Application pages/views
│   ├── plugins/           # Built-in Cordova plugins (native code)
│   ├── settings/          # Settings management
│   ├── sidebarApps/       # Sidebar application modules
│   ├── styles/            # Global stylesheets
│   ├── theme/             # Theme system
│   ├── utils/             # Utility functions
│   └── views/             # Handlebars templates
├── www/                   # Public web assets and build output
│   └── build/             # Compiled JavaScript/CSS bundles
├── utils/                 # Build scripts and development tools
├── hooks/                 # Cordova build hooks
├── platforms/             # Generated Android project (gitignored)
├── plugins/               # Generated plugin builds (gitignored)
├── .devcontainer/         # Docker development environment
└── config.xml             # Cordova configuration
```

---

## Useful Commands

### Development Setup
```bash
# Install dependencies and set up Cordova
pnpm run setup

# Or with bun
bun run setup
```

### Building
```bash
# Build debug APK (paid version)
pnpm run build paid dev apk
# Or shorthand
pnpm run build p d

# Build release APK (paid version)
pnpm run build paid release apk
# Or shorthand
pnpm run build p r

# Build debug APK (free version)
pnpm run build free dev apk
# Or shorthand
pnpm run build f d
```

### Code Quality
```bash
# Run Biome linter and formatter (auto-fix issues)
pnpm run check

# Lint only (auto-fix)
pnpm run lint

# Format only (auto-fix)
pnpm run format

# TypeScript type checking (no emit)
pnpm run typecheck
```

### Internationalization (i18n)
```bash
# Add new translation string
pnpm run lang add

# Remove translation string
pnpm run lang remove

# Search translation strings
pnpm run lang search

# Update translations
pnpm run lang update

# Check translation files (used in CI)
npm run lang check
```

### Utility Commands
```bash
# Clean build artifacts
pnpm run clean

# Start development server
pnpm run start

# Update Ace editor (legacy)
pnpm run updateAce

# Plugin development
pnpm run plugin
```

---

## Best Practices and Guidelines

### Code Style

1. **Biome Configuration**: The project uses Biome for linting and formatting
   - Tabs for indentation (not spaces)
   - Auto-organize imports
   - Run `pnpm run check` before committing

2. **JavaScript/TypeScript**
   - Use ES2020+ features (optional chaining, nullish coalescing, etc.)
   - Prefer `const` over `let`, avoid `var`
   - Use named exports where possible
   - Follow existing naming conventions (camelCase for variables/functions, PascalCase for classes/components)

3. **Imports**
   - Use path aliases defined in `tsconfig.json` (e.g., `import foo from "lib/foo"`)
   - Organize imports: external libraries first, then internal modules
   - Avoid circular dependencies

### Component Development

1. **UI Components**
   - Components are in `src/components/`
   - Use `html-tag-js` for DOM manipulation
   - Follow existing component patterns (see `Page`, `Contextmenu`, `toast` as examples)
   - Components should be self-contained and reusable

2. **Pages**
   - Pages are in `src/pages/`
   - Each page should export a function that returns a `Page` component
   - Handle page lifecycle (init, destroy) properly

3. **Dialogs**
   - Dialogs are in `src/dialogs/`
   - Use existing dialog patterns (`alert`, `confirm`, `prompt`, `select`)
   - Return promises for async dialog results

### File System Operations

1. **File System Abstraction**
   - Always use `fsOperation` from `fileSystem` module
   - Never use native Cordova file API directly
   - Handle errors gracefully with try-catch

2. **File Paths**
   - Use `Url` utility for path manipulation
   - Be aware of Android file system permissions
   - Test with different storage locations (internal, external, SD card)

### Editor Integration

1. **CodeMirror 6**
   - Extensions are in `src/cm/`
   - Use the command registry for editor commands
   - Follow CodeMirror 6 patterns for state management
   - Test with multiple languages

2. **Language Support**
   - Language modes are in `src/cm/modes/`
   - Register new modes in `src/cm/modelist.ts`
   - Provide syntax highlighting and completion where possible

### Plugin Development

1. **Built-in Plugins**
   - Native plugins are in `src/plugins/`
   - Follow Cordova plugin conventions
   - Use TypeScript for type safety where possible

2. **External Plugins**
   - See [Acode Plugin Documentation](https://docs.acode.app/)
   - Plugin starter: https://github.com/Acode-Foundation/acode-plugin

### Testing

1. **Manual Testing**
   - Test on real Android devices when possible
   - Test with different Android versions (minimum SDK 21)
   - Test file operations with different storage types

2. **Code Quality Checks**
   - Run `pnpm run check` before committing
   - Ensure TypeScript type checking passes (`pnpm run typecheck`)
   - Verify no linting errors

### Git Workflow

1. **Branching**
   - Create feature branches from `main`
   - Use descriptive branch names (e.g., `feat/dark-mode`, `fix/crash-on-large-files`)

2. **Commits**
   - Use conventional commit messages:
     ```
     feat: add dark mode toggle to settings
     fix: resolve crash when opening large files
     docs: update build instructions
     refactor: simplify file loading logic
     ```
   - Keep commits focused and atomic
   - Reference related issues when applicable

3. **Pull Requests**
   - Provide clear description of changes
   - Include screenshots/GIFs for UI changes
   - Ensure CI checks pass
   - Reference related issue

### Performance

1. **Bundle Size**
   - Use dynamic imports for large modules
   - Avoid unnecessary dependencies
   - Tree-shake unused code

2. **Runtime Performance**
   - Debounce expensive operations (search, autocomplete)
   - Use Web Workers for heavy computations
   - Minimize DOM manipulation

3. **Memory Management**
   - Clean up event listeners
   - Dispose of unused resources
   - Handle large files carefully

### Security

1. **Input Validation**
   - Sanitize user input with DOMPurify
   - Validate file paths
   - Check file permissions

2. **Data Storage**
   - Don't store sensitive data in localStorage
   - Use secure storage for credentials
   - Encrypt sensitive data when necessary

3. **Network Operations**
   - Validate URLs before making requests
   - Use HTTPS when possible
   - Handle network errors gracefully

---

## Development Environment

### DevContainer (Recommended)
- Use VS Code with Dev Containers extension
- Automatic setup with all dependencies
- Consistent environment across team

### Manual Setup
- Node.js 18+ (22 recommended)
- Java JDK 21
- Android SDK with API 36
- pnpm or bun

### IDE Configuration
- VS Code with Biome extension
- Format on save enabled
- Biome as default formatter

---

## Common Patterns

### Event Handling
```javascript
// Use action stack for back button handling
actionStack.push({
  id: 'my-action',
  action: () => { /* cleanup */ },
  Backspace: () => { /* handle backspace */ },
});

// Remove action when done
actionStack.remove('my-action');
```

### File Operations
```javascript
import fsOperation from "fileSystem";

// Read file
const content = await fsOperation(url).readFile('utf8');

// Write file
await fsOperation(url).writeFile(content);

// List directory
const files = await fsOperation(url).lsDir();
```

### Dialogs
```javascript
import alert from "dialogs/alert";
import confirm from "dialogs/confirm";
import prompt from "dialogs/prompt";

await alert('Title', 'Message');
const result = await confirm('Title', 'Message');
const value = await prompt('Title', 'Placeholder', 'Default value');
```

### Settings
```javascript
import settings from "lib/settings";

// Get setting
const value = settings.get('settingName');

// Set setting
settings.set('settingName', value);
```

---

## Troubleshooting

### Build Issues
- Ensure Java JDK 21 is installed and `JAVA_HOME` is set
- Verify Android SDK is installed with API 36
- Run `pnpm run clean` before rebuilding
- Check Cordova platform version: `cordova platform ls`

### Development Issues
- Run `pnpm run setup` to reinstall dependencies
- Clear `node_modules` and reinstall if issues persist
- Check Biome configuration in `biome.json`

### Performance Issues
- Use Chrome DevTools remote debugging for Android
- Profile JavaScript execution
- Check for memory leaks in event listeners

---

## Resources

- [Acode Documentation](https://docs.acode.app/)
- [Plugin Development Guide](https://github.com/Acode-Foundation/acode-plugin)
- [CodeMirror 6 Documentation](https://codemirror.net/6/docs/)
- [Cordova Android Documentation](https://cordova.apache.org/docs/en/latest/guide/platforms/android/index.html)
- [Biome Documentation](https://biomejs.dev/)

---

## Notes for AI Agents

1. **File Paths**: Use forward slashes for cross-platform compatibility
2. **Imports**: Use path aliases (e.g., `import foo from "lib/foo"`)
3. **Error Handling**: Always handle errors gracefully, especially for file operations
4. **Async/Await**: Prefer async/await over Promise chains
5. **TypeScript**: Run `pnpm run typecheck` to verify types
6. **Linting**: Run `pnpm run check` before suggesting changes
7. **Testing**: Test changes on Android device/emulator when possible
8. **Documentation**: Update documentation when adding new features

---

*Last Updated: 2026-06-04*
