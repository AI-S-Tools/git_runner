# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Git Runner is a TypeScript-based automated Git operations tool designed for continuous development workflows. It performs periodic commits and pushes across multiple repositories within a project structure with intelligent repository discovery.

## Development Commands

### Building the Application
```bash
npm run build         # Compile TypeScript to JavaScript
```

### Running the Application
```bash
npm start             # Run compiled JavaScript
npm run dev           # Run TypeScript directly with ts-node
```

### Development Mode
```bash
npm run watch         # Watch mode - automatically recompile on changes
```

### Dependency Management
```bash
npm install           # Install dependencies
npm update            # Update dependencies
```

## Architecture Overview

### Core Components

**Main Entry Point**: `src/git-runner.ts` - Single file containing all functionality
- **Process Management**: Uses Node.js `child_process` for Git command execution
- **Repository Discovery**: Multi-layered scanning approach
- **Timer System**: Periodic execution every 5 minutes with manual controls
- **AI Integration**: OpenRouter agent integration for intelligent commit messages

### Repository Discovery Logic

The tool uses a sophisticated repository discovery system:

1. **Project Root Detection**: Scans upward from script directory to find `.git`
2. **Workspace Integration**: Parses `.code-workspace` files for additional repositories
3. **Submodule Processing**: Reads `.gitmodules` for submodule repositories
4. **Processing Order**: Submodules first, then main repositories (prevents conflicts)

### Key Functions

- `findProjectRoot()`: Locates project root via `.git` directory traversal
- `findGitRepositories()`: Discovers all Git repos (main + workspace + submodules)
- `runGitTasks()`: Orchestrates repository processing with proper ordering
- `generateCommitMessageWithOpenRouter()`: AI-powered commit message generation
- `gitCommit()`: Handles staging, sparse-checkout detection, and commits
- `gitPush()`: Manages upstream branch setup and pushing

### Interactive Commands

During execution, the tool accepts single-character commands:
- `r` - Manual re-scan and process all repositories
- `R` - Reload/restart the entire script
- `q` - Graceful shutdown with timer cleanup

### Integration Features

**OpenRouter Agent Integration**:
- Looks for `openrouter/bin/openrouter_agent.dart` in project root
- Generates intelligent commit messages using AI
- Falls back to timestamp-based messages if unavailable

**VS Code Task Integration**:
- Pre-configured tasks in `tasks.json` for background execution
- Can run as folder-open task for automatic startup

### Error Handling Patterns

- Continues operation if individual repositories fail
- Graceful fallback when AI commit generation fails
- Skips push operations when remotes are unavailable
- Proper cleanup on shutdown or restart