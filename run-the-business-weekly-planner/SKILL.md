---
name: run-the-business-weekly-planner
description: Help a manager open the week with a practical Run the Business operating plan: current health, key metrics, weekly must-win priorities, risks, escalations, team focus, daily rhythm, and possible RPM candidates. Use when the user writes "run the business", "rtb", "תכנון שבועי לשוטף", "תעשה לי run the business", "בוא נפתח שבוע למנהל", "תכנון שבוע למנהל שירות", "תכנון run the business למכירות", or otherwise asks to plan the operational week for a manager or team.
---

# Run the Business Weekly Planner

Run the Business answers:

> How do we keep the business healthy, stable, responsive, and under control this week?

This is different from RPM:

> RPM asks what important change we are leading to move the company forward.

Use this skill to help a manager start the week with a clear operating plan: what must stay healthy, what must be closed, what is at risk, who owns what, and when to escalate.

The full output template, question bank, operating metrics, escalation rules, and examples live in [REFERENCE.md](REFERENCE.md). Load it before writing the final file.

## Triggers

Activate on any of:

- `run the business`
- `rtb`
- `תכנון שבועי לשוטף`
- `תעשה לי run the business`
- `בוא נפתח שבוע למנהל`
- `תכנון שבוע למנהל שירות`
- `תכנון run the business למכירות`
- `תכנון שוטף שבועי`
- Any request to plan the weekly operating rhythm for a manager, team, or department.

## Language

Mirror the user's language. If the trigger is Hebrew, the entire conversation and saved Markdown body are Hebrew. If English, English.

The Markdown section headings stay in English either way — they are the template structure.

### RTL wrapping for Hebrew output

When the body is Hebrew, wrap the **entire saved file** in a right-aligned RTL block so Hebrew renders correctly in GitHub, Obsidian, and most Markdown viewers:

```md
<div dir="rtl" align="right">

# Run the Business Weekly Plan: {Area / Manager / Week}

...all sections of the template...

</div>
```

The blank lines after the opening `<div>` and before the closing `</div>` are required. For English output, do **not** wrap — leave the file as plain Markdown.

## Core Behavior

Behave like a sharp operating partner, not a passive questionnaire.

1. Identify the manager or operating area. If unclear, ask one short question.
2. Walk through the Run the Business sections in order.
3. **Ask one question at a time.** Do not bundle many questions together.
4. For every question, use the Question Pattern below.
5. Push for measurable operating health, not vague intentions.
6. Separate routine operating work from RPM-style change projects.
7. Identify recurring operating problems that may need to become RPM projects.
8. When the user gives a partial answer, fill the gap with a reasonable assumption and mark it clearly: "Assuming X — push back if wrong."
9. Once there is enough material, save the weekly plan file.

## Question Pattern

Each question has four parts:

1. The question itself.
2. One line explaining why it matters.
3. 2–5 numbered concrete suggestions so the user can reply "2 + a tweak."
4. An invitation to pick, edit, reject, or replace.

Use the question bank and suggestion lists in [REFERENCE.md](REFERENCE.md).

## Behavior Rules

- Reject vague answers like "לטפל בשוטף", "להתקדם", "לסגור דברים", or "להיות על זה".
- Reframe vague work into weekly outcomes that can be checked by the end of the week.
- Choose only 3–5 operating metrics. Too many metrics create noise.
- Choose only 3–5 must-win priorities. This is a weekly plan, not a task dump.
- Every important priority needs an owner and deadline.
- Every meaningful risk needs an escalation trigger.
- Use Green / Yellow / Red for operating health.
- Ask what has been stuck for more than 48 hours.
- Ask what recurring operational issue may need an RPM.
- Keep the plan practical enough for a manager to use immediately on Sunday or Monday morning.

## Run the Business vs RPM

Always keep this distinction clear:

| Example | Type |
|---|---|
| Answer open support tickets | Run the Business |
| Reduce repeated support tickets by 30% | RPM |
| Call new leads | Run the Business |
| Improve sales close rate from 20% to 30% | RPM |
| Fix customer bugs | Run the Business |
| Build a system that prevents recurring bugs | RPM |
| Handle slow websites | Run the Business |
| Build monitoring that detects slow websites automatically | RPM |

If the user describes a recurring operational problem, say:

> This belongs in this week's Run the Business plan, but it may also be an RPM candidate if it keeps repeating and hurting the metrics.

In Hebrew:

> זה חלק מהשוטף השבוע, אבל יכול להיות שזה גם מועמד ל־RPM אם זה חוזר על עצמו ופוגע במדדים.

## Saving the File

Slug the area and week to lowercase English:

- `שירות לקוחות שבוע 2026-05-03` → `customer-service-2026-05-03`
- `Sales weekly plan` → `sales-2026-05-03`
- `DevOps` → `devops-2026-05-03`

Save to `projects/run-the-business/{slug}.md` relative to the current working directory. Create `projects/run-the-business/` if it doesn't exist. If you're not inside a folder where this makes sense, ask the user where to save.

### If the file already exists

Ask the user before writing. Offer three options:

1. **Overwrite** the existing file.
2. **Save as a new version** — `{slug}-v2.md`, `{slug}-v3.md`, etc.
3. **Update in place** — merge new answers into the existing file's sections.

Default recommendation in the prompt: option 2.

## Final Response After Saving

Reply in the user's language. English version:

> Created your Run the Business weekly plan: **{Area / Manager / Week}**
> Saved to: `{file_path}`

Hebrew version:

> יצרתי עבורך את תוכנית ה־Run the Business השבועית: **{Area / Manager / Week}**
> הקובץ נשמר כאן: `{file_path}`

Then a short summary:

- Operating health
- Main metrics
- Must-win priorities
- Main risk
- First action
- Potential RPM candidate

## Output Template

Use the full Markdown template in [REFERENCE.md](REFERENCE.md). Adapt the metrics and priorities to the manager's area: service, sales, development, DevOps, SEO/performance, onboarding, customer operations, account management, or another operating area.
