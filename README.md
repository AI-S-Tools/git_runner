# Git Runner

**Automated Git Operations Tool for Multi-Repository Projects**

## Core Function
Automated periodic git commits and push operations across multiple repositories within a project structure. Designed for continuous development workflows with intelligent repository discovery.

## Executable Information
- **Primary Executable**: `git_runner.dart`
- **Location**: `/Users/lpm/Repo/mfp_tools/git_runner/git_runner.dart`
- **Runtime**: Dart VM
- **Execution**: `dart run git_runner.dart`

## Key Features

### Repository Discovery
- **Main Repository**: Automatically detects project root via `.git` directory
- **Workspace Integration**: Parses `.code-workspace` files for additional repositories
- **Submodule Support**: Processes all Git submodules defined in `.gitmodules`
- **Processing Order**: Submodules first, then main repositories (prevents conflicts)

### Automation Capabilities
- **Periodic Commits**: Automatic commits every 5 minutes
- **Intelligent Commit Messages**: Uses `trae-cli` integration when available
- **Remote Detection**: Automatically pushes only when remote repositories exist
- **Interactive Control**: Real-time keyboard commands during execution

### Interactive Commands
- `r` - Manual re-scan and process all repositories
- `R` - Reload/restart the entire script
- `q` - Graceful shutdown with timer cleanup

### Integration Features
- **Trae-CLI Integration**: Generates intelligent commit messages when available
- **VS Code Task Integration**: Can be configured as VS Code task
- **Process Management**: Proper cleanup and restart capabilities
- **Error Handling**: Continues operation even if individual repositories fail

## Technical Implementation

### Repository Processing Logic
1. Scans from script directory upward to find project root
2. Identifies all Git repositories (main + workspace + submodules)
3. Processes submodules first to avoid merge conflicts
4. Commits changes with intelligent or fallback messages
5. Pushes to remote only if remote exists

### Commit Message Strategy
- **With trae-cli**: Generates contextual commit messages
- **Fallback**: Uses timestamp-based commit messages
- **Error Recovery**: Continues with basic messages if trae-cli fails

### File System Operations
- **Path Resolution**: Uses `package:path` for cross-platform compatibility
- **Directory Traversal**: Recursive scanning with safety limits
- **JSON Parsing**: Workspace file parsing with error handling

## Configuration Requirements

### Dependencies (pubspec.yaml)
```yaml
dependencies:
  path: ^1.8.0
  process: ^4.2.0
```

### Optional External Tools
- **trae-cli**: For intelligent commit message generation
- **VS Code**: For workspace file integration

## Usage Patterns

### Standalone Execution
```bash
cd /path/to/project
dart run /Users/lpm/Repo/mfp_tools/git_runner/git_runner.dart
```

### VS Code Task Integration
Add to `.vscode/tasks.json`:
```json
{
  "label": "Git Runner",
  "type": "shell",
  "command": "dart",
  "args": ["run", "/Users/lpm/Repo/mfp_tools/git_runner/git_runner.dart"],
  "group": "build",
  "presentation": {
    "echo": true,
    "reveal": "always",
    "panel": "new"
  }
}
```

### Development Workflow Integration
- Start at beginning of development session
- Runs continuously in background
- Provides automatic backup of work progress
- Enables easy rollback via Git history

## Error Handling
- **Repository Access**: Continues if individual repositories fail
- **Network Issues**: Skips push operations when remote unavailable
- **Tool Dependencies**: Graceful fallback when trae-cli unavailable
- **Process Management**: Proper cleanup on shutdown or restart

## Performance Characteristics
- **Memory Usage**: Minimal - processes repositories sequentially
- **CPU Impact**: Low - only active during 5-minute intervals
- **Network Usage**: Only when pushing to remotes
- **Disk I/O**: Minimal - only Git operations and log output