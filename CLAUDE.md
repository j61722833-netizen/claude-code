# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Overview

This is the Claude Code CLI tool repository, containing the official CLI for Claude. The repository is primarily focused on GitHub issue management automation and providing examples for Claude Code users.

## Repository Structure

- `scripts/` - TypeScript automation scripts for GitHub issue management
  - `auto-close-duplicates.ts` - Automatically closes issues marked as duplicates after a period of inactivity
  - `backfill-duplicate-comments.ts` - Triggers duplicate detection workflows for existing issues
- `examples/` - Example implementations for Claude Code users
  - `hooks/bash_command_validator_example.py` - Example hook for validating bash commands before execution
- `Script/` - PowerShell scripts for development environment setup
- Root files: README.md, CHANGELOG.md, LICENSE.md, SECURITY.md

## Development Commands

This repository uses Bun as the runtime for TypeScript scripts:

```bash
# Run the auto-close duplicates script
bun run scripts/auto-close-duplicates.ts

# Run the backfill duplicate comments script  
bun run scripts/backfill-duplicate-comments.ts
```

## Environment Variables

Both GitHub automation scripts require:
- `GITHUB_TOKEN` - GitHub personal access token with repo and actions permissions
- `GITHUB_REPOSITORY_OWNER` - Repository owner (defaults to "anthropics")
- `GITHUB_REPOSITORY_NAME` - Repository name (defaults to "claude-code")

For the backfill script:
- `DRY_RUN` - Set to "false" to actually trigger workflows (default: true for safety)
- `DAYS_BACK` - How many days back to look for old issues (default: 90)

## Architecture

### GitHub Issue Management Scripts

The repository contains two main automation scripts:

1. **Auto-close Duplicates** (`scripts/auto-close-duplicates.ts`)
   - Scans for issues with duplicate detection bot comments older than 3 days
   - Checks for user disagreement (thumbs down reactions)
   - Automatically closes confirmed duplicates with appropriate labels and comments

2. **Backfill Duplicate Comments** (`scripts/backfill-duplicate-comments.ts`)
   - Identifies issues without existing duplicate detection comments
   - Triggers the dedupe workflow for these issues
   - Includes dry-run mode for safe testing

Both scripts use the GitHub REST API and include comprehensive logging for debugging.

### Hook Examples

The `examples/hooks/` directory provides a Python example of a Claude Code hook that validates bash commands, specifically converting `grep` commands to use `rg` (ripgrep) instead.

## Key Implementation Details

- Scripts use TypeScript with Bun runtime
- GitHub API integration with proper error handling and rate limiting
- Comprehensive logging with debug levels
- Safety mechanisms including dry-run modes and user disagreement detection
- Pagination handling for large result sets