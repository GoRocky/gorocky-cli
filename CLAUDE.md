# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is the Gemini CLI (gorocky-cli fork) - a command-line AI workflow tool that connects to Google's Gemini API. It's a TypeScript monorepo using npm workspaces with two main packages:
- `packages/cli` - React/Ink-based terminal UI
- `packages/core` - Backend handling Gemini API communication and tool execution

## Essential Commands

### Development
```bash
npm install              # Install dependencies
npm run build           # Build all packages
npm start               # Run the CLI from source
npm run preflight       # Run before committing (format, lint, build, test)
```

### Testing
```bash
npm test                # Run unit tests
npm run test:e2e        # Run integration tests
npm run test:coverage   # Run tests with coverage
```

### Code Quality
```bash
npm run lint            # Run ESLint
npm run format          # Format with Prettier
npm run typecheck       # TypeScript type checking
```

### Debugging
```bash
npm run debug           # Start with debugging enabled
DEV=true npm start      # Enable React DevTools
```

## Architecture

### Package Structure
- **CLI Package** (`packages/cli/`): User interface, themes, configuration
  - `src/ui/` - React components for terminal UI
  - `src/config/` - Configuration management
  - Entry point: `src/index.ts`

- **Core Package** (`packages/core/`): API communication, tools, services
  - `src/tools/` - Tool implementations (file, shell, web operations)
  - `src/services/` - File discovery, Git integration
  - `src/core/` - Gemini API client
  - Entry point: `src/server.ts`

### Key Architectural Patterns
- Tools are modular and located in `packages/core/src/tools/`
- UI components use React hooks and functional components
- Server-client communication via JSON-RPC
- Sandboxing support for security (Docker/Podman or macOS Seatbelt)

### Authentication Flow
1. Google OAuth (default)
2. API key via `GEMINI_API_KEY` environment variable
3. Vertex AI credentials
4. Configuration stored in `.gemini/settings.json`

## Development Guidelines

### Before Making Changes
1. Run `npm run preflight` to ensure everything passes
2. For new tools, add to `packages/core/src/tools/`
3. For UI changes, work in `packages/cli/src/ui/`
4. Tests go alongside source files (`*.test.ts`, `*.test.tsx`)

### Code Style
- TypeScript with strict mode enabled
- Prefer plain objects over classes
- Use `unknown` instead of `any`
- Follow existing patterns in neighboring files
- React components should be functional with hooks

### Testing
- Use Vitest for unit tests
- Mock external dependencies appropriately
- Integration tests go in `/integration-tests/`
- Run tests before committing

### Git Workflow
- Main branch: `main`
- Create feature branches for changes
- Keep commits focused and use conventional commit messages
- Link PRs to issues

## Common Tasks

### Adding a New Tool
1. Create tool file in `packages/core/src/tools/`
2. Follow existing tool patterns (see `file.ts`, `shell.ts` as examples)
3. Register tool in the tool registry
4. Add appropriate tests

### Modifying UI Components
1. Components are in `packages/cli/src/ui/`
2. Use Ink components for terminal UI
3. Follow React hooks patterns
4. Test with `DEV=true npm start` for React DevTools

### Working with Themes
- Themes are defined in `packages/cli/src/themes/`
- Users can switch themes via settings
- Follow existing theme structure

## Important Notes

- Node.js 18+ required
- This is a Google open source project requiring CLA for contributions
- Sandbox mode restricts file system access for security
- Settings stored in `.gemini/settings.json` (home and project directories)
- Environment variables can be set in `.env` files
- Build output goes to `dist/` directories and `bundle/` for distribution