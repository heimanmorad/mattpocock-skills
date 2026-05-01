---
name: rpm-project-builder
description: Turn any topic, idea, task, goal, or project into a structured RPM (What/Why/How/Who/When) project document through an interactive interview, then save it as a Markdown file. Use when the user writes "rpm <topic>", "RPM <topic>", "תעשה לי rpm על <topic>", "בוא נעשה rpm ל <topic>", "תבנה לי rpm עבור <topic>", or otherwise asks to build, plan, or scope a project using the RPM framework.
---

# RPM Project Builder

RPM has five questions:

1. What do I want?
2. Why do I want it?
3. How can that be done?
4. Who will do it?
5. When will it be done?

Behave like a strategic project partner — challenge weak answers, push for measurable outcomes, named owners, and concrete dates. Don't run a passive questionnaire.

The full output template, per-section suggestion banks, and a worked example live in [REFERENCE.md](REFERENCE.md). Load it before writing the final file.

## Triggers

Activate on any of:

- `rpm <topic>` / `RPM <topic>`
- `תעשה לי rpm על <topic>`
- `בוא נעשה rpm ל <topic>`
- `תבנה לי rpm עבור <topic>`

## Language

Mirror the user's language. If the trigger is Hebrew, the entire conversation and the saved Markdown body are Hebrew. If English, English. The Markdown section headings (`## 1. What do I want?`, etc.) stay in English either way — they're the template structure.

## Core Behavior

1. Identify the topic. If ambiguous, confirm it in one short line.
2. Walk the five RPM sections in order.
3. **Ask one question at a time.** Do not bundle a section's sub-questions. After each answer, react briefly (validate, push back if vague, or refine), then ask the next.
4. For every question, follow the question pattern below.
5. When the user gives a partial answer, fill the gap with a reasonable assumption and mark it clearly: "Assuming X — push back if wrong."
6. Track which section you're in silently — don't announce all sub-questions up front.
7. Once all five sections have enough material, save the file.

## Question Pattern

Each question has four parts:

1. The question itself.
2. One line on why it matters.
3. 2–5 numbered concrete suggestions, so the user can reply "2 + a tweak."
4. An invitation to pick, edit, reject, or replace.

The per-section question lists and suggestion banks are in [REFERENCE.md](REFERENCE.md).

## Behavior Rules

- Push for measurable outcomes. Reject "work on X"; reframe as "increase X by Y by date Z."
- Push for named people, not "the team."
- Push for concrete dates, not "soon." Today's date is the anchor — when the user says "within 7 days," compute and use the actual date in the timeline table.
- Prefer the simplest version that ships. Suggest an MVP before a full build.
- Suggestions are numbered for easy reference.
- Mark assumptions explicitly.

## Saving the File

Slug the topic to lowercase English (`שיפור שירות לקוחות` → `customer-service-improvement`, `Youleap launch` → `youleap-launch`). If the slug is unclear, ask.

Save to `projects/rpm/{slug}.md` relative to the current working directory. Create `projects/rpm/` if it doesn't exist. If you're not inside a folder where this makes sense, ask the user where to save.

### If the file already exists

Ask the user before writing. Offer three options:

1. **Overwrite** the existing file.
2. **Save as a new version** — `{slug}-v2.md`, `{slug}-v3.md`, etc.
3. **Update in place** — merge new answers into the existing file's sections.

Default recommendation in the prompt: option 2 (preserves history).

## Final Response After Saving

Reply in the user's language. English version:

> Created your RPM project: **{Project Name}**
> Saved to: `{file_path}`

Hebrew version:

> יצרתי עבורך את קובץ ה־RPM לפרויקט: **{Project Name}**
> הקובץ נשמר כאן: `{file_path}`

Then a short summary:

- Desired outcome
- Why it matters
- First action
- Owner
- Target date

## Output Template

Use the full Markdown template in [REFERENCE.md](REFERENCE.md). Adapt the Participants table to the project type — always keep `Owner` and `Approval` rows; choose other roles based on whether this is software, business, personal, ops, etc. (REFERENCE.md has role examples per type.)
