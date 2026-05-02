# Agent Skills

A collection of agent skills that extend capabilities across planning, development, and tooling.

## Install All Skills

Clone the repo and copy all skills to your global Claude Code skills directory:

```bash
git clone https://github.com/heimanmorad/mattpocock-skills
cd mattpocock-skills
cp -r $(ls -d */ | grep -v -E '^\.') ~/.claude/skills/
```

Works across all Claude Code interfaces on the same machine: CLI, Desktop app, VS Code / JetBrains extension.

## Install a Single Skill

```bash
npx skills@latest add mattpocock/skills/<skill-name>
```

---

## Workflow Systems

- **get-shit-done** — Spec-driven development system for AI-assisted projects. Structured phases (discuss → plan → execute → verify), wave-based parallel execution, and persistent state files to prevent context rot.

  ```bash
  cp -r mattpocock-skills/get-shit-done ~/.claude/skills/
  ```

  > Based on [gsd-build/get-shit-done](https://github.com/gsd-build/get-shit-done). Install the full GSD system separately via `npx get-shit-done-cc@latest`.

## Planning & Design

These skills help you think through problems before writing code.

- **write-a-prd** — Create a PRD through an interactive interview, codebase exploration, and module design. Filed as a GitHub issue.

  ```
  npx skills@latest add mattpocock/skills/write-a-prd
  ```

- **prd-to-plan** — Turn a PRD into a multi-phase implementation plan using tracer-bullet vertical slices.

  ```
  npx skills@latest add mattpocock/skills/prd-to-plan
  ```

- **prd-to-issues** — Break a PRD into independently-grabbable GitHub issues using vertical slices.

  ```
  npx skills@latest add mattpocock/skills/prd-to-issues
  ```

- **grill-me** — Get relentlessly interviewed about a plan or design until every branch of the decision tree is resolved.

  ```
  npx skills@latest add mattpocock/skills/grill-me
  ```

- **design-an-interface** — Generate multiple radically different interface designs for a module using parallel sub-agents.

  ```
  npx skills@latest add mattpocock/skills/design-an-interface
  ```

- **request-refactor-plan** — Create a detailed refactor plan with tiny commits via user interview, then file it as a GitHub issue.

  ```
  npx skills@latest add mattpocock/skills/request-refactor-plan
  ```

## Business Management

These skills help managers plan operational work, ownership, people leadership, and business execution rhythm.

- **run-the-business-weekly-planner** — Help a manager open the week with a practical Run the Business operating plan: current health, key metrics, must-win priorities, risks, escalations, team focus, daily rhythm, and possible RPM candidates.

  ```
  npx skills@latest add mattpocock/skills/run-the-business-weekly-planner
  ```

- **lead-the-team-weekly-planner** — Help a manager open the week with a practical Lead the Team plan: team health, people focus, ownership, accountability, feedback conversations, development, alignment, culture, standards, and manager commitments.

  ```
  npx skills@latest add mattpocock/skills/lead-the-team-weekly-planner
  ```

## Development

These skills help you write, refactor, and fix code.

- **tdd** — Test-driven development with a red-green-refactor loop. Builds features or fixes bugs one vertical slice at a time.

  ```
  npx skills@latest add mattpocock/skills/tdd
  ```

- **triage-issue** — Investigate a bug by exploring the codebase, identify the root cause, and file a GitHub issue with a TDD-based fix plan.

  ```
  npx skills@latest add mattpocock/skills/triage-issue
  ```

- **improve-codebase-architecture** — Explore a codebase for architectural improvement opportunities, focusing on deepening shallow modules and improving testability.

  ```
  npx skills@latest add mattpocock/skills/improve-codebase-architecture
  ```

- **migrate-to-shoehorn** — Migrate test files from `as` type assertions to @total-typescript/shoehorn.

  ```
  npx skills@latest add mattpocock/skills/migrate-to-shoehorn
  ```

- **scaffold-exercises** — Create exercise directory structures with sections, problems, solutions, and explainers.

  ```
  npx skills@latest add mattpocock/skills/scaffold-exercises
  ```

## Tooling & Setup

- **setup-pre-commit** — Set up Husky pre-commit hooks with lint-staged, Prettier, type checking, and tests.

  ```
  npx skills@latest add mattpocock/skills/setup-pre-commit
  ```

- **git-guardrails-claude-code** — Set up Claude Code hooks to block dangerous git commands (push, reset --hard, clean, etc.) before they execute.

  ```
  npx skills@latest add mattpocock/skills/git-guardrails-claude-code
  ```

## Writing & Knowledge

- **write-a-skill** — Create new skills with proper structure, progressive disclosure, and bundled resources.

  ```
  npx skills@latest add mattpocock/skills/write-a-skill
  ```

- **edit-article** — Edit and improve articles by restructuring sections, improving clarity, and tightening prose.

  ```
  npx skills@latest add mattpocock/skills/edit-article
  ```

- **ubiquitous-language** — Extract a DDD-style ubiquitous language glossary from the current conversation.

  ```
  npx skills@latest add mattpocock/skills/ubiquitous-language
  ```

- **obsidian-vault** — Search, create, and manage notes in an Obsidian vault with wikilinks and index notes.

  ```
  npx skills@latest add mattpocock/skills/obsidian-vault
  ```
