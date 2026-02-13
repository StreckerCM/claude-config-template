# Claude Config Template

Standardized Claude Code configuration files for AI-assisted development across repositories.

## What's Included

| File | Purpose |
|------|---------|
| `CLAUDE.md.template` | Main Claude Code configuration with placeholders |
| `settings.json.template` | Claude Code permissions configuration for `.claude/settings.json` |
| `gitignore-snippet.txt` | `.gitignore` entries for Claude Code local files |
| `docs/ARCHITECTURE.md.template` | Architecture documentation template |
| `docs/prompts/PERSONAS.md` | 11 development personas for Ralph Loop rotation |
| `docs/prompts/README.md` | Persona documentation |
| `docs/prompts/templates/README.md` | Prompt template index |
| `docs/prompts/templates/ROTATING_FEATURE.md` | Rotating persona feature implementation |
| `docs/prompts/templates/IMPLEMENT_FEATURE.md` | Single-persona feature implementation |
| `docs/prompts/templates/FIX_BUG.md` | Bug investigation and fixing |
| `docs/prompts/templates/ADD_TESTS.md` | Adding unit tests |
| `docs/prompts/templates/REFACTOR.md` | Code refactoring tasks |

## Setup for a New Repository

### 1. Copy the files

```bash
# Clone this repo
git clone https://github.com/StreckerCM/claude-config-template.git

# Copy files to your target repo
cp CLAUDE.md.template /path/to/your-repo/CLAUDE.md
mkdir -p /path/to/your-repo/docs/prompts/templates
cp docs/prompts/PERSONAS.md /path/to/your-repo/docs/prompts/
cp docs/prompts/README.md /path/to/your-repo/docs/prompts/
cp docs/prompts/templates/* /path/to/your-repo/docs/prompts/templates/
cp docs/ARCHITECTURE.md.template /path/to/your-repo/docs/ARCHITECTURE.md
```

### 2. Fill in CLAUDE.md placeholders

Open `CLAUDE.md` and replace all `{{PLACEHOLDER}}` variables:

| Placeholder | Description | Example |
|-------------|-------------|--------|
| `{{PROJECT_DESCRIPTION}}` | One-line description | Wellbore survey calibration app |
| `{{TECH_STACK}}` | Technology stack | .NET 8.0 WPF (C#) |
| `{{BUILD_COMMAND}}` | Build command | `dotnet build MySolution.sln` |
| `{{TEST_COMMAND}}` | Test command | `dotnet test MySolution.sln` |
| `{{MAIN_BRANCH}}` | Main/production branch | `master` or `main` |

### 3. Fill in ARCHITECTURE.md

Open `docs/ARCHITECTURE.md` and replace placeholders with your project's structure:

| Placeholder | Description | Example |
|-------------|-------------|--------|
| `{{PROJECT_NAME}}` | Main project name | `MyApp` |
| `{{SOURCE_DIR}}` | Source directory | `src/MyApp` |
| `{{TEST_DIR}}` | Test directory | `tests/MyApp.Tests` |
| `{{MAIN_FILE}}` | Primary source file | `Program.cs` |

### 4. Customize prompt templates

The templates in `docs/prompts/templates/` use `{{BUILD_COMMAND}}` and `{{TEST_COMMAND}}` placeholders. Replace these with your actual build/test commands, and customize the project-specific persona details (e.g., UI framework, security concerns).

### 5. Set up permissions

Copy `settings.json.template` to `.claude/settings.json` and fill in placeholders:

```bash
mkdir -p /path/to/your-repo/.claude
cp settings.json.template /path/to/your-repo/.claude/settings.json
```

| Placeholder | Description | Example |
|-------------|-------------|--------|
| `{{SOURCE_DIR}}` | Path to source code | `Application` or `src` |
| `{{TEST_DIR}}` | Path to test projects | `Tests` or `test` |
| `{{BUILD_TOOL}}` | Build tool command prefix | `dotnet build *` or `npm *` |
| `{{TEST_TOOL}}` | Test tool command prefix | `dotnet test *` or `npm test *` |

**What the permissions do:**

- **allow** -- Grants Claude Code access to read cross-repo files, edit source/test/config files, run build/test commands, and use git/GitHub CLI operations
- **deny** -- Blocks editing package directories, force-pushing, hard-resetting, and recursive deletes

### 6. Update .gitignore

Add the entries from `gitignore-snippet.txt` to your repo's `.gitignore`:

```
# Claude Code local files (allow settings.json to be tracked)
.claude/*
!.claude/settings.json
*.local.md
```

This ensures `.claude/settings.json` is tracked in version control while other Claude Code local files are ignored.

## Repositories Using This Template

- [XactSpot-MSA](https://github.com/StreckerCM/XactSpot-MSA)
- [DrillCalc](https://github.com/StreckerCM/DrillCalc)
- [Salesforce-Sandbox-Utility](https://github.com/StreckerCM/Salesforce-Sandbox-Utility)
- [GeoMagSharpGUI](https://github.com/StreckerCM/GeoMagSharpGUI)

## Core Principles

1. **Ralph Loop is mandatory** for all feature branch work
2. **tasks.md is a gate** -- no code without a task file
3. **Rotating personas** ensure multi-perspective review
4. **PR comments** create an audit trail
5. **2 clean cycles** required for completion
