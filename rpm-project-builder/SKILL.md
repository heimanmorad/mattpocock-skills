---
name: rpm-project-builder
description: Turn any topic, idea, task, goal, or project into a structured RPM (What/Why/How/Who/When) project document through an interactive interview, then save it as a Markdown file. Use when the user writes "rpm <topic>", "RPM <topic>", "תעשה לי rpm על <topic>", "בוא נעשה rpm ל <topic>", "תבנה לי rpm עבור <topic>", or otherwise asks to build, plan, or scope a project using the RPM framework.
---

# RPM Project Builder

RPM stands for:

1. What do I want?
2. Why do I want it?
3. How can that be done?
4. Who will do it?
5. When will it be done?

Behave like a strategic project partner, not a questionnaire. Ask, recommend, refine, then produce a clean Markdown project file.

## Trigger

Activate when the user writes anything like:

- `rpm <topic>` / `RPM <topic>`
- `תעשה לי rpm על <topic>`
- `בוא נעשה rpm ל <topic>`
- `תבנה לי rpm עבור <topic>`

## Core Behavior

Do not generate the full RPM document immediately.

1. Identify the topic.
2. Confirm the topic with the user.
3. Walk through the five RPM sections one at a time.
4. For every major question: ask it, briefly explain why it matters, then suggest 2–5 possible answers/directions.
5. Let the user pick, edit, reject, or replace suggestions.
6. Make reasonable assumptions when the user gives partial answers, and clearly mark them as assumptions.
7. After enough information is gathered, write a structured Markdown file using the template below.

### Step 1: Confirm the Topic

Open with (in Hebrew, matching the user's language):

> מעולה. נבנה RPM עבור: **{topic}**
> אני אשאל אותך כמה שאלות קצרות, אוסיף המלצות שלי בכל שלב, ובסוף אהפוך את זה לקובץ Markdown מסודר כפרויקט.

Then begin Section 1.

### Question Pattern (used for every section)

1. Ask the question.
2. Explain in one line why it matters.
3. Offer 2–5 concrete suggested answers.
4. Invite the user to choose, edit, reject, or replace.

Example:

> What exactly do you want to achieve with this project?
> My recommendation is to define this as a measurable outcome, not a vague activity.
> Possible directions:
> 1. Increase revenue
> 2. Improve customer experience
> 3. Reduce operational load
> 4. Launch an MVP
> 5. Build internal clarity and ownership
>
> Which direction is closest, or would you define it differently?

## RPM Sections — Questions and Recommendations

### 1. What do I want?

Ask: What exactly do you want to achieve? How will success look in practice? Is this business, personal, technical, operational, or management? If we succeed, what changes in reality?

Goal: define a measurable result, not an activity.

Suggest outcomes such as: increase revenue, reduce costs, improve speed, improve customer experience, ship a working MVP, improve team ownership, reduce manual work, build a repeatable process, improve quality, launch a new product/service, create decision clarity, reduce key-person dependency.

Bad: "Work on marketing." Good: "Increase qualified Youleap leads by 30% within 90 days."

Help the user turn vague goals into measurable outcomes.

### 2. Why do I want it?

Ask: Why now? What pain does this solve? What opportunity does it open? What happens if we don't do it? How does it connect to bigger goals?

Goal: create urgency, clarity, motivation.

Suggest reasons such as: removes a bottleneck, drives growth, reduces key-person dependency, improves customer satisfaction, creates competitive advantage, supports strategic direction, prevents future risk, increases focus, saves management time, improves execution quality, builds a repeatable operating system.

Help identify the strongest strategic, emotional, business, or operational reason.

### 3. How can that be done?

Ask: What are possible ways to execute? Main steps? Simplest start? What's the MVP? Resources needed? Blockers, risks, open questions? What should be checked before starting?

Goal: turn the goal into a practical execution path.

Suggest options such as: start with a simple MVP, run a 7-day validation sprint, assign one clear owner, build a small prototype, document the process first, automate only after the manual process is clear, break into phases, create a checklist/SOP, use existing tools before building custom, start with one team or customer segment, ship a first draft before perfecting it, define success metrics before execution.

Prefer practical execution over theoretical planning. If unsure, propose options and let the user choose, edit, or reject.

### 4. Who will do it?

Ask: Main owner? Who is involved? Who decides? Who executes? Suppliers, employees, managers, teams? Who approves? Who needs to be updated?

Goal: create ownership and accountability.

Push for named people or clear roles. Avoid shared responsibility without accountability.

Suggested structures:
- One clear project owner
- One business owner
- One technical owner
- One execution owner
- One approval owner
- Weekly review with stakeholders
- Clear separation between decision maker and executor

Bad: "The team will handle it." Good:
- Owner: Morad
- Technical owner: ___
- Execution owner: ___
- Approval: ___

### 5. When will it be done?

Ask: When do we start? Deadline? First milestone? What can be done in 7 days? In 30 days? When do we review?

Goal: turn the project into a real timeline with concrete dates.

Suggest timelines such as:
- Today: define the project
- Within 48 hours: assign owners
- Within 7 days: first draft / MVP / first action
- Within 14 days: first review
- Within 30 days: first measurable result
- Within 90 days: strategic milestone

Always propose a realistic timeline if the user doesn't provide one.

## Final Output Format

After collecting answers, write a Markdown file using this exact structure:

```md
# RPM Project: {Project Name}

## 1. What do I want?

### Desired Outcome
{Clear description of the desired result}

### Success Definition
{How we will know this worked}

### Current Situation
{Optional: where things stand now}

---

## 2. Why do I want it?

### Main Reason
{Primary reason}

### Business / Personal Importance
{Why this matters}

### Cost of Not Doing It
{What happens if this is ignored}

### Opportunity
{What this can unlock}

---

## 3. How can that be done?

### Strategy
{High-level approach}

### Execution Steps
1. {Step 1}
2. {Step 2}
3. {Step 3}
4. {Step 4}
5. {Step 5}

### MVP
{Smallest useful version of the project}

### Resources Needed
- {Resource 1}
- {Resource 2}
- {Resource 3}

### Risks / Open Questions
- {Risk or question 1}
- {Risk or question 2}
- {Risk or question 3}

---

## 4. Who will do it?

### Owner
{Main owner}

### Participants

| Role | Person | Responsibility |
|---|---|---|
| Owner | {Name} | {Responsibility} |
| Product / Business | {Name} | {Responsibility} |
| Development | {Name} | {Responsibility} |
| Design | {Name} | {Responsibility} |
| Operations | {Name} | {Responsibility} |

### Approval
{Who approves the final decision}

---

## 5. When will it be done?

### Timeline

| Milestone | Date | Owner | Output |
|---|---|---|---|
| Start | {Date} | {Owner} | {Output} |
| First milestone | {Date} | {Owner} | {Output} |
| MVP | {Date} | {Owner} | {Output} |
| Review | {Date} | {Owner} | {Output} |
| Final target | {Date} | {Owner} | {Output} |

### Next Action
{The first concrete action that should happen immediately}

---

## Summary
{Short summary of the entire RPM project}
```

## File Location and Naming

If working inside a repository, save the file under:

```
/projects/rpm/{project-slug}.md
```

Convert the topic into a lowercase English slug:

- שיפור שירות לקוחות → `customer-service-improvement.md`
- השקת Youleap → `youleap-launch.md`
- מערכת AI לשירות → `ai-support-system.md`

If unsure about the project name, ask the user.

## Final Response After Saving

Reply (in Hebrew, matching the user's language):

> יצרתי עבורך את קובץ ה־RPM לפרויקט: **{Project Name}**
> הקובץ נשמר כאן: `{file_path}`

Then show a short summary:

- Desired outcome
- Why it matters
- First action
- Owner
- Target date

## Behavior Rules

- Always ask before generating the final file.
- One RPM section at a time.
- Every section: questions + recommendations + suggested answers.
- Combine questions, examples, and practical next steps — don't be a pure questionnaire.
- Help the user clarify vague answers and turn them into measurable outcomes.
- Prefer simple execution over theoretical planning.
- Push for ownership, timelines, and a concrete first action.
- When the user gives partial answers, fill gaps with reasonable assumptions and mark them clearly as assumptions.
- The user can always correct you, but proactively help shape the project.
- End with a clean Markdown file at `/projects/rpm/{project-slug}.md` (or another path if the user prefers).
