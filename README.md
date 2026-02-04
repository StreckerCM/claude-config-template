# Claude Config Template

Standardized Claude Code configuration files for AI-assisted development across repositories.

## What's Included

| File | Purpose |
|------|---------|
| `CLAUDE.md.template` | Main Claude Code configuration with placeholders |
| `docs/prompts/PERSONAS.md` | 11 development personas for Ralph Loop rotation |
| `docs/prompts/templates/ROTATING_FEATURE.md` | Ralph Loop prompt templates |
| `docs/prompts/README.md` | Persona documentation |

## Setup for a New Repository

### 1. Copy the template

```bash
# Clone this repo
git clone https://github.com/StreckerCM/claude-config-template.git

# Copy files to your target repo
cp CLAUDE.md.template /path/to/your-repo/CLAUDE.md
mkdir -p /path/to/your-repo/docs/prompts/templates
cp docs/prompts/PERSONAS.md /path/to/your-repo/docs/prompts/
cp docs/prompts/templates/ROTATING_FEATURE.md /path/to/your-repo/docs/prompts/templates/
cp docs/prompts/README.md /path/to/your-repo/docs/prompts/
```

### 2. Fill in the placeholders

Open `CLAUDE.md` and replace all `{{PLACEHOLDER}}` variables:

| Placeholder | Description | Example |
|-------------|-------------|--------|
| `{{PROJECT_NAME}}` | Project name | XactSpot MSA |
| `{{PROJECT_DESCRIPTION}}` | One-line description | Wellbore survey calibration app |
| `{{TECH_STACK}}` | Technology stack | .NET 8.0 WPF (C#) |
| `{{BUILD_COMMAND}}` | Build command | `dotnet build MySolution.sln` |
| `{{TEST_COMMAND}}` | Test command | `dotnet test MySolution.sln` |
| `{{MAIN_BRANCH}}` | Main/production branch | `master` or `main` |
| `{{PREVIEW_BRANCH}}` | Pre-release branch | `preview` |
| `{{DEFAULT_BRANCH}}` | Branch to create features from | `preview` or `main` |

### 3. Add project-specific sections

After filling in placeholders, add:
- Architecture / Solution Structure
- Key Source Files
- Key Patterns
- Platform Constraints
- Any project-specific conventions

## Repositories Using This Template

- [XactSpot-MSA](https://github.com/StreckerCM/XactSpot-MSA)
- [DrillCalc](https://github.com/StreckerCM/DrillCalc)
- [Salesforce-Sandbox-Utility](https://github.com/StreckerCM/Salesforce-Sandbox-Utility)
- [GeoMagSharpGUI](https://github.com/StreckerCM/GeoMagSharpGUI)

## Core Principles

1. **Ralph Loop is mandatory** for all feature branch work
2. **tasks.md is a gate** — no code without a task file
3. **Rotating personas** ensure multi-perspective review
4. **PR comments** create an audit trail
5. **2 clean cycles** required for completion
