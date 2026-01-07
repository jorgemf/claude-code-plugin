# Code Toolkit

A comprehensive developer toolkit for Claude Code with git workflow commands, PR review agents, and IDE/browser integrations.

## Overview

This plugin provides:
- **5 Slash Commands** for git workflows (branching, commits, PRs, code review)
- **6 Review Agents** for comprehensive code quality analysis
- **2 MCP Servers** for JetBrains IDE and Chrome DevTools integration

## Slash Commands

### `/code-toolkit:branch-new`
Create a new branch with standardized naming: `[username]/[type]/[date]-[title]`

```bash
/code-toolkit:branch-new Add user authentication
# Creates: jorge/feature/2025-01-07-add-user-authentication

/code-toolkit:branch-new fix payment bug
# Creates: jorge/bugfix/2025-01-07-fix-payment-bug
```

Auto-detects branch type from keywords: feature, bugfix, hotfix, refactor, docs, test, chore, ci, perf, experimental.

### `/code-toolkit:commit-push`
Create a commit and push with code quality checks.

- Auto-stages modified/deleted tracked files (`git add -u`)
- Checks for TODO/FIXME/HACK comments
- Detects fallback code and incomplete patterns
- Validates documentation consistency
- Generates commit message following repo style

### `/code-toolkit:pr-new`
Create a draft PR assigned to yourself.

- Analyzes all commits since branch diverged
- Generates comprehensive PR summary
- Creates with `--draft --assign @me`
- Includes Summary, Changes, and Test Plan sections

### `/code-toolkit:code-review`
Automated code review using multiple parallel agents.

- Checks CLAUDE.md compliance
- Scans for bugs and issues
- Reviews git history context
- Analyzes code comments
- Filters false positives with confidence scoring

### `/code-toolkit:pr-review`
Comprehensive PR review using specialized agents.

```bash
/code-toolkit:pr-review              # Full review
/code-toolkit:pr-review tests errors # Specific aspects
/code-toolkit:pr-review all parallel # All agents in parallel
```

## Review Agents

Claude automatically triggers these agents based on context:

| Agent | Focus | Triggers |
|-------|-------|----------|
| **comment-analyzer** | Comment accuracy and maintainability | After adding docs, reviewing comments |
| **pr-test-analyzer** | Test coverage and quality | Before PR, after adding tests |
| **silent-failure-hunter** | Error handling, silent failures | When reviewing catch blocks |
| **type-design-analyzer** | Type design and invariants | When adding/modifying types |
| **code-reviewer** | General code quality, CLAUDE.md compliance | Before commits, during reviews |
| **code-simplifier** | Code clarity and refactoring | After passing review |

### Usage Examples

```
"Check if the tests cover all edge cases"
-> Triggers pr-test-analyzer

"Review the error handling in the API client"
-> Triggers silent-failure-hunter

"Simplify this code"
-> Triggers code-simplifier
```

## MCP Servers

### JetBrains IDE Integration
Tools for interacting with IntelliJ-based IDEs:

- `get_file_problems` - Get diagnostics/errors from IDE
- `get_symbol_info` - Look up symbol information
- `get_run_configurations` - List available run configs
- `execute_terminal_command` - Run terminal commands
- `execute_run_configuration` - Execute run configs
- `rename_refactoring` - Perform rename refactoring

### Chrome DevTools Integration
Tools for browser automation and debugging:

- `navigate_page` - Navigate to URLs
- `take_snapshot` / `take_screenshot` - Capture page state
- `evaluate_script` - Execute JavaScript
- `list_console_messages` / `get_console_message` - Read console
- `list_network_requests` / `get_network_request` - Monitor network
- `click` / `fill` - Interact with elements
- `wait_for` - Wait for conditions
- `handle_dialog` - Handle browser dialogs

## Recommended Workflow

```
1. /code-toolkit:branch-new Add new feature
2. Write code
3. "Review my changes" -> code-reviewer
4. Fix issues
5. /code-toolkit:commit-push
6. /code-toolkit:pr-new
7. /code-toolkit:pr-review
8. Address feedback
9. Merge
```

## Installation

Add to your Claude Code plugins:

```bash
claude --plugin-dir ./plugins/code-toolkit
```

Or install from marketplace if published.

## Requirements

- `gh` CLI for GitHub operations
- JetBrains IDE with MCP plugin for IDE integration
- Chrome with DevTools MCP extension for browser tools

## License

MIT
