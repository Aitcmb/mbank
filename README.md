# mbank

Structured project development skill for Claude Code, Codex, and Cursor.

`mbank` helps an agent move from vague product ideas to a disciplined implementation workflow using layered project documents instead of ad hoc chat memory.

## Why mbank

Most agent workflows are good at writing code but weak at:
- turning fuzzy ideas into a usable design brief
- preserving project context across long conversations
- keeping architecture, progress, and implementation docs in sync

`mbank` addresses that with a document-first workflow:

```text
/discover -> /mbank -> /scaffold -> build -> /check -> /archive
```

## What It Does

- `/discover`
  Expand and refine a vague idea into `mbank/discovery.md` and `mbank/design.md`
- `/mbank`
  Turn `mbank/design.md` into a clarified plan and generate `mbank/tech-stack.md`
- `/scaffold`
  Generate project operating docs like `AGENTS.md`, `implementation-plan.md`, `progress.md`, `architecture.md`, and `quickref.md`
- `/check`
  Run explicit verification and document consistency checks
- `/archive`
  Snapshot a milestone and reset active progress tracking

## Installation

```bash
# Claude Code
git clone https://github.com/Aitcmb/mbank.git ~/.claude/skills/mbank

# Codex / OpenAI Agents
git clone https://github.com/Aitcmb/mbank.git ~/.codex/skills/mbank

# Cursor
git clone https://github.com/Aitcmb/mbank.git ~/.cursor/skills/mbank
```

## Quick Start

### 1. Create a project folder

```bash
mkdir mbank
```

### 2. Start with discovery

If the project idea is still fuzzy:

```text
/discover
```

This stage produces:
- `mbank/discovery.md`
- `mbank/design.md`

### 3. Confirm the technical direction

```text
/mbank
```

This stage:
- reads `mbank/design.md`
- fills in missing constraints
- evaluates complexity
- generates `mbank/tech-stack.md`

### 4. Generate the working scaffold

```text
/scaffold
```

This stage generates:
- `AGENTS.md`
- `CLAUDE.md` (optional compatibility file)
- `mbank/implementation-plan.md`
- `mbank/progress.md`
- `mbank/architecture.md`
- `mbank/quickref.md`
- `mbank/context/`

### 5. Develop, verify, archive

```text
/check
/archive
```

## Core Files

```text
your-project/
├── AGENTS.md
├── CLAUDE.md
├── .claude/
│   └── rules/
└── mbank/
    ├── discovery.md
    ├── design.md
    ├── tech-stack.md
    ├── quickref.md
    ├── architecture.md
    ├── implementation-plan.md
    ├── progress.md
    ├── context/
    └── archive/
```

## Context Model

`mbank` uses layered context instead of loading everything all the time:

- `AGENTS.md`
  Primary project rules file
- `.claude/rules/`
  Global hard constraints
- `mbank/quickref.md`
  Always-read fast reference
- `mbank/architecture.md`
  Architecture and responsibilities
- `mbank/design.md`
  Formal product/design definition
- `mbank/discovery.md`
  Early-stage reasoning and scope tradeoffs
- `mbank/progress.md`
  Current progress and active state
- `mbank/context/`
  Module-specific deep context loaded only when needed

## Repository Layout

- `SKILL.md`
  Main skill instructions
- `defaults.md`
  Example preference file
- `references/`
  Templates loaded only when needed
- `CHANGELOG.md`
  Release history

## Templates

The repository includes reusable templates in `references/`:

- `discovery-template.md`
- `design-template.md`
- `scaffold-templates.md`

## License

MIT
