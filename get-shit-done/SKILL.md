---
name: get-shit-done
description: Spec-driven development system for AI-assisted projects using structured phases, context engineering, and wave-based parallel execution. Use when user wants to build a new project with AI, mentions GSD, wants structured planning phases, or needs systematic context management across sessions.
---

# Get Shit Done (GSD)

A meta-prompting system that fights "context rot" through structured phases, atomic commits, and persistent state files.

## Installation

```bash
npx get-shit-done-cc@latest
```

## Core Workflow

```
/gsd-new-project          → Creates PROJECT.md, REQUIREMENTS.md, ROADMAP.md, STATE.md
/gsd-discuss-phase <n>    → Clarifies implementation details, produces CONTEXT.md
/gsd-plan-phase <n>       → Creates atomic task PLAN.md files with dependency graph
/gsd-execute-phase <n>    → Runs plans in parallel waves, atomic git commits per task
/gsd-verify-work <n>      → User acceptance testing with auto-diagnosis of failures
/gsd-ship                 → Creates PRs from verified work
```

## Quick Tasks

```bash
/gsd-quick <task>           # Skip research/discussion, just do it
/gsd-quick --discuss <task> # Lightweight discussion before planning
/gsd-quick --research <task># Focused research before planning
/gsd-quick --full <task>    # Full pipeline: discuss → research → plan → verify
```

## Navigation

```bash
/gsd-do <description>   # Smart dispatcher — routes to the right command
/gsd-next               # Auto-detect and run the next step
/gsd-progress           # Show current project state
```

## State Files

All state lives in `.planning/`:

| File             | Purpose                              |
| ---------------- | ------------------------------------ |
| `PROJECT.md`     | Project context and goals            |
| `REQUIREMENTS.md`| Scoped requirements                  |
| `ROADMAP.md`     | Phased milestones                    |
| `STATE.md`       | Todos, threads, quick tasks log      |
| `CONTEXT.md`     | Phase-specific implementation notes  |
| `RESEARCH.md`    | Findings from research agent         |
| `PLAN-*.md`      | Atomic task plans with dependencies  |

## Execution Model

`execute-phase` groups plans into dependency-ordered **waves** and runs each wave in parallel using subagents. Each subagent gets a fresh 200k-token context — preventing token waste. Use `--wave N` to run a single wave, `--interactive` for sequential execution with checkpoints.
