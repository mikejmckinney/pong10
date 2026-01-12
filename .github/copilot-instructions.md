## Required Context

- Always read `/AI_REPO_GUIDE.md` first and follow it as the canonical reference
- If `AI_REPO_GUIDE.md` is missing/stale: follow `.github/prompts/repo-onboarding.md` to rebuild context and update it in the same PR
- Read `AGENTS.md` for general agent guidelines (it references AI_REPO_GUIDE.md)

## Project Overview

This is a **dotfiles template repository** for GitHub Codespaces and AI-assisted development. It provides pre-configured AI agent prompts, automatic VS Code extension installation, and standardized files that can be copied to new repositories.

### Tech Stack
- Language: Bash (shell scripts)
- Build tool: None (pure shell + markdown)
- Test framework: Custom bash test script
- Package manager: None

## Quick Commands

### Setup
```bash
# No setup required - repository is ready to use
# For Codespaces: Link this repo in GitHub Codespaces settings
```

### Build
```bash
# No build step - this is a template repository
# Scripts are executed directly via bash
```

### Test
```bash
# Run template verification
./test.sh

# Validate shell script syntax
bash -n install.sh
bash -n test.sh

# Test install script (safe to run locally)
bash install.sh
```

### Lint
```bash
# Check shell scripts with shellcheck (if available)
shellcheck install.sh test.sh

# Find all markdown files
find . -name "*.md" -exec echo "Found: {}" \;
```

## Project Structure

```
/
├── AI_REPO_GUIDE.md          # Canonical AI reference (always read first)
├── AGENTS.md                 # Root agent instructions
├── AGENT.md                  # Deprecated redirect to AGENTS.md
├── README.md                 # User-facing documentation
├── install.sh                # Codespace bootstrap script
├── test.sh                   # Verification script
├── .cursor/
│   └── BUGBOT.md             # Cursor Bugbot PR review rules
├── .gemini/
│   └── styleguide.md         # Gemini Code Assist review style
└── .github/
    ├── copilot-instructions.md   # This file - Copilot instructions
    ├── agents/
    │   └── judge.agent.md        # Plan-gate + diff-gate reviewer agent
    └── prompts/
        ├── copilot-onboarding.md # Guide for customizing this file
        └── repo-onboarding.md    # Comprehensive repo onboarding prompt
```

## Key Files

| File | Purpose |
|------|---------|
| `AI_REPO_GUIDE.md` | Canonical reference for AI agents - contains authoritative commands, structure, conventions |
| `AGENTS.md` | Root agent instructions - points to AI_REPO_GUIDE.md |
| `install.sh` | Codespace bootstrap - installs VS Code extensions, copies prompts |
| `test.sh` | Verification - ensures all required template files exist and are valid |
| `.github/copilot-instructions.md` | This file - specific instructions for GitHub Copilot |
| `.github/agents/judge.agent.md` | Custom agent for plan/diff reviews |
| `.github/prompts/repo-onboarding.md` | Comprehensive repo onboarding workflow for new AI agents |
| `.github/prompts/copilot-onboarding.md` | Guide for customizing copilot-instructions.md |
| `.cursor/BUGBOT.md` | PR review rules for Cursor Bugbot |
| `.gemini/styleguide.md` | PR review style guide for Gemini Code Assist |

## Conventions

### File Naming
- Agent instruction files: `AGENTS.md`, `*.agent.md`, or tool-specific paths (e.g., `.cursor/BUGBOT.md`)
- Prompts: `*.prompt.md` or in `.github/prompts/` directory
- Style guides: `styleguide.md` in tool-specific directories (e.g., `.gemini/`)
- Scripts: Use `.sh` extension with `#!/bin/bash` shebang

### Content Guidelines
- Keep instructions concise (aim for < 2 pages per file)
- Include verification commands where applicable
- Use structured output formats (checklists, tables, code blocks)
- Reference `AI_REPO_GUIDE.md` for canonical commands
- No secrets or sensitive data in any files

### Shell Script Style
- Use `set -e` for error handling
- Include descriptive comments and section headers
- Use colored output for better readability (RED, GREEN, YELLOW, NC)
- Validate inputs and provide helpful error messages
- Check command availability before using (e.g., `command -v code`)

## Common Gotchas

- **DOTFILES environment variable**: `install.sh` requires `$DOTFILES` (set automatically by GitHub Codespaces). Falls back to script directory if not set.
- **code command**: The `code` command may not be available outside VS Code/Codespaces. Install script handles this gracefully.
- **Tool-specific paths**: Different AI tools read from specific paths:
  - GitHub Copilot: `.github/copilot-instructions.md`, `.github/agents/*.agent.md`
  - Cursor: `.cursor/BUGBOT.md`
  - Gemini: `.gemini/styleguide.md`
  - Most tools: `AGENTS.md` at root
- **AGENT.md vs AGENTS.md**: `AGENT.md` is deprecated - use `AGENTS.md` instead
- **Workspace detection**: Install script auto-detects workspace path; prioritizes `/workspaces` (Codespaces default)

## Before Submitting Changes

Always verify:
1. [ ] Tests pass: `./test.sh`
2. [ ] Shell syntax valid: `bash -n install.sh && bash -n test.sh`
3. [ ] If shellcheck available: `shellcheck install.sh test.sh`
4. [ ] All required files present (21 checks should pass in test.sh)
5. [ ] Update `AI_REPO_GUIDE.md` if you changed:
   - Commands or scripts
   - File structure or locations
   - Conventions or guidelines
   - Entry points or key files
6. [ ] No sensitive information added to any files

## Maintenance Guidelines

When making changes to this template:
- **Minimal changes**: Keep diffs small and focused
- **Test first**: Run `./test.sh` before and after changes
- **Document changes**: Update `AI_REPO_GUIDE.md` if structure/commands/conventions change
- **Update README**: If user-facing behavior changes, update README.md
- **Verify all platforms**: Consider impact on Cursor, Gemini, and GitHub Copilot users
- **Keep consistent**: Match existing code style and file organization
