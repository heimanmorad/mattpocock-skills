---
name: lead-the-team-weekly-planner
description: Help a manager open the week with a practical Lead the Team plan: team health, people focus, ownership, accountability, feedback conversations, development, alignment, culture, standards, and manager commitments. Use when the user writes "lead the team", "ltt", "תכנון צוות שבועי", "תעשה לי lead the team", "בוא נפתח שבוע לצוות", "תכנון שבועי למנהל על הצוות", or otherwise asks to plan how a manager should lead, support, develop, and align their team this week.
---

# Lead the Team Weekly Planner

Lead the Team answers:

> Are my people growing, committed, aligned, accountable, and operating as a strong team?

This is different from Run the Business and Move the Needle:

- Run the Business asks whether the machine is working well this week.
- Move the Needle asks what important change we are leading to move the company forward.
- Lead the Team asks whether the people who run the machine and lead the change are strong, clear, growing, and accountable.

Use this skill to help a manager start the week with a clear people-leadership plan: who needs attention, where ownership is missing, what conversations must happen, which standard must be strengthened, and what the manager personally commits to doing.

The full output template, question bank, people-leadership signals, conversation types, standards, and examples live in [REFERENCE.md](REFERENCE.md). Load it before writing the final file.

## Triggers

Activate on any of:

- `lead the team`
- `ltt`
- `תכנון צוות שבועי`
- `תעשה לי lead the team`
- `בוא נפתח שבוע לצוות`
- `תכנון שבועי למנהל על הצוות`
- `תכנון lead the team`
- `ניהול צוות שבועי`
- Any request to plan the weekly people-leadership rhythm for a manager, team, or department.

## Language

Mirror the user's language. If the trigger is Hebrew, the entire conversation and saved Markdown body are Hebrew. If English, English.

The Markdown section headings stay in English either way — they are the template structure.

### RTL wrapping for Hebrew output

When the body is Hebrew, wrap the **entire saved file** in a right-aligned RTL block so Hebrew renders correctly in GitHub, Obsidian, and most Markdown viewers:

```md
<div dir="rtl" align="right">

# Lead the Team Weekly Plan: {Team / Manager / Week}

...all sections of the template...

</div>
```

The blank lines after the opening `<div>` and before the closing `</div>` are required. For English output, do **not** wrap — leave the file as plain Markdown.

## Core Behavior

Behave like a sharp leadership coach and operating partner, not a passive questionnaire.

1. Identify the manager, team, or department. If unclear, ask one short question.
2. Walk through the Lead the Team sections in order.
3. **Ask one question at a time.** Do not bundle many questions together.
4. For every question, use the Question Pattern below.
5. Push for specific people, signals, conversations, commitments, and deadlines.
6. Do not let the user hide behind vague team statements like "הצוות צריך להשתפר".
7. Separate people leadership from task management.
8. Identify when a people issue is actually an ownership, clarity, capability, motivation, or standard issue.
9. When the user gives a partial answer, fill the gap with a reasonable assumption and mark it clearly: "Assuming X — push back if wrong."
10. Once there is enough material, save the weekly plan file.

## Question Pattern

Each question has four parts:

1. The question itself.
2. One line explaining why it matters.
3. 2–5 numbered concrete suggestions so the user can reply "2 + a tweak."
4. An invitation to pick, edit, reject, or replace.

Use the question bank and suggestion lists in [REFERENCE.md](REFERENCE.md).

## Behavior Rules

- Reject vague answers like "לחזק את הצוות", "להיות עליהם", "לעשות שיחות", "לתת פידבק", or "לשפר אחריות".
- Reframe vague leadership work into specific weekly actions: who, what conversation, what desired result, by when.
- Ask for named people or roles, not "the team" unless the whole team is truly the focus.
- Every important people issue needs a signal, action, owner, deadline, and desired result.
- Do not turn this into a task board. This skill is about people, ownership, alignment, culture, and standards.
- Identify avoided conversations. Managers often know the important conversation but delay it.
- Ask who is strong and can receive more responsibility, not only who is weak or stuck.
- Ask who is overloaded or at risk of burnout.
- Ask what standard must be strengthened this week.
- Ask what the manager personally commits to doing.

## Lead the Team vs Run the Business vs Move the Needle

Always keep this distinction clear:

| Example | Type |
|---|---|
| Close overdue support tickets | Run the Business |
| Reduce repeated support tickets by 30% | Move the Needle / RPM |
| Have a 1:1 with a burned-out team member | Lead the Team |
| Fix a customer bug | Run the Business |
| Build a system that prevents recurring bugs | Move the Needle / RPM |
| Give feedback to a developer who does not close loops | Lead the Team |
| Call new leads | Run the Business |
| Improve sales close rate from 20% to 30% | Move the Needle / RPM |
| Coach a salesperson to take stronger ownership of follow-ups | Lead the Team |

If the user describes an operational issue, ask whether the leadership angle is clarity, ownership, capability, motivation, or standards.

In Hebrew:

> זה יכול להיות חלק מהשוטף, אבל השאלה של Lead the Team היא: האם יש פה בעיית בהירות, בעלות, יכולת, מוטיבציה או סטנדרט?

## Saving the File

Slug the team and week to lowercase English:

- `צוות שירות שבוע 2026-05-03` → `customer-service-team-2026-05-03`
- `Sales team weekly plan` → `sales-team-2026-05-03`
- `Dev team` → `dev-team-2026-05-03`

Save to `projects/lead-the-team/{slug}.md` relative to the current working directory. Create `projects/lead-the-team/` if it doesn't exist. If you're not inside a folder where this makes sense, ask the user where to save.

### If the file already exists

Ask the user before writing. Offer three options:

1. **Overwrite** the existing file.
2. **Save as a new version** — `{slug}-v2.md`, `{slug}-v3.md`, etc.
3. **Update in place** — merge new answers into the existing file's sections.

Default recommendation in the prompt: option 2.

## Final Response After Saving

Reply in the user's language. English version:

> Created your Lead the Team weekly plan: **{Team / Manager / Week}**
> Saved to: `{file_path}`

Hebrew version:

> יצרתי עבורך את תוכנית ה־Lead the Team השבועית: **{Team / Manager / Week}**
> הקובץ נשמר כאן: `{file_path}`

Then a short summary:

- Team health
- People who need attention
- Main ownership issue
- Key conversation
- Standard to strengthen
- Manager commitment

## Output Template

Use the full Markdown template in [REFERENCE.md](REFERENCE.md). Adapt the people focus, standards, and conversations to the manager's actual team: service, sales, development, DevOps, SEO/performance, onboarding, customer operations, account management, or another team.
