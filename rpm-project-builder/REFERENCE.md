# RPM Reference

Per-section questions, suggestion banks, the full output template, and a worked example. Load this file when you need any of them.

## Per-Section Questions and Recommendations

Use these as a question pool — pick one at a time, react to the answer, then move on. Don't dump them all at once.

### 1. What do I want?

**Questions to draw from**

- What exactly do you want to achieve with this topic?
- How will success look in practice?
- Is this a business, personal, technical, operational, or management outcome?
- If we succeed, what will be different in reality?

**Goal**: define a measurable result, not an activity.

**Suggestion bank**

- Increase revenue
- Reduce costs
- Improve speed
- Improve customer experience
- Ship a working MVP
- Improve team ownership
- Reduce manual work
- Build a repeatable process
- Improve quality
- Launch a new product or service
- Create clarity around a decision
- Reduce dependency on specific people

**Bad** → "Work on marketing."
**Good** → "Increase qualified Youleap leads by 30% within 90 days."

---

### 2. Why do I want it?

**Questions to draw from**

- Why is this important now?
- What pain does this solve?
- What opportunity does this open?
- What happens if we don't do this?
- How does this connect to your bigger personal or business goals?

**Goal**: create urgency, clarity, motivation.

**Suggestion bank**

- Removes an operational bottleneck
- Drives business growth
- Reduces dependency on specific people
- Improves customer satisfaction
- Creates competitive advantage
- Supports the company's strategic direction
- Prevents future risk
- Increases focus and clarity
- Saves management time
- Improves execution quality
- Builds a repeatable operating system

---

### 3. How can that be done?

**Questions to draw from**

- What are the possible ways to execute this?
- What are the main steps?
- What is the simplest way to start?
- What can be the MVP?
- What resources are needed?
- What blockers, risks, or open questions exist?
- What should be checked before starting?

**Goal**: turn the goal into a practical execution path.

**Suggestion bank**

- Start with a simple MVP
- Run a 7-day validation sprint
- Assign one clear owner
- Build a small working prototype
- Document the process first
- Automate only after the manual process is clear
- Break the project into phases
- Create a checklist or SOP
- Use existing tools before building custom
- Start with one team or one customer segment
- Ship a first draft before perfecting it
- Define success metrics before execution

Prefer practical execution over theoretical planning.

---

### 4. Who will do it?

**Questions to draw from**

- Who is the main owner?
- Who should be involved?
- Who makes the decisions?
- Who executes the work?
- Are there suppliers, employees, managers, or teams involved?
- Who needs to approve?
- Who needs to be updated?

**Goal**: create ownership and accountability. Push for named people.

**Suggested ownership structures**

- One clear project owner
- One business owner
- One technical owner
- One execution owner
- One approval owner
- Weekly review with stakeholders
- Clear separation between decision maker and executor

**Bad** → "The team will handle it."
**Good** → Owner: `{Your Name}` / Technical owner: `___` / Execution owner: `___` / Approval: `___`

**Adaptive role rows** for the Participants table — always keep `Owner` and `Approval`; choose the rest based on project type:

- **Software / product project** → Product, Development, Design, Operations
- **Business / go-to-market project** → Sales, Marketing, Customer Success, Finance
- **Customer-service / ops project** → Service Lead, Operations, QA, Training
- **Personal project** → Owner only, plus an optional Accountability Partner
- **Mixed / unclear** → propose roles based on the answers so far and ask the user to confirm

---

### 5. When will it be done?

**Questions to draw from**

- When do we start?
- What is the deadline?
- What is the first milestone?
- What can be completed within 7 days?
- What should happen within 30 days?
- When should we do a review meeting?

**Goal**: turn the project into a real timeline with concrete dates.

**Suggested timeline anchors** (compute actual dates from today)

- Today: define the project
- Within 48 hours: assign owners
- Within 7 days: first draft / MVP / first action
- Within 14 days: first review
- Within 30 days: first measurable result
- Within 90 days: strategic milestone

Always propose a realistic timeline if the user doesn't provide one. Use today's date as the anchor and write actual dates into the timeline table.

---

## Output Template

For Hebrew bodies, wrap the entire content below in `<div dir="rtl" align="right">` ... `</div>` (with blank lines after the opening tag and before the closing tag). For English bodies, use the template as-is. See SKILL.md → "RTL wrapping for Hebrew output."

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
| {Role 2} | {Name} | {Responsibility} |
| {Role 3} | {Name} | {Responsibility} |
| {Role 4} | {Name} | {Responsibility} |
| Approval | {Name} | {Responsibility} |

### Approval
{Who approves the final decision}

---

## 5. When will it be done?

### Timeline

| Milestone | Date | Owner | Output |
|---|---|---|---|
| Start | {YYYY-MM-DD} | {Owner} | {Output} |
| First milestone | {YYYY-MM-DD} | {Owner} | {Output} |
| MVP | {YYYY-MM-DD} | {Owner} | {Output} |
| Review | {YYYY-MM-DD} | {Owner} | {Output} |
| Final target | {YYYY-MM-DD} | {Owner} | {Output} |

### Next Action
{The first concrete action that should happen immediately}

---

## Summary
{Short summary of the entire RPM project}
```

---

## Worked Example

Topic: `rpm Youleap launch`. Slug: `youleap-launch`. Saved to `projects/rpm/youleap-launch.md`. (Dates assume today is 2026-05-01.)

```md
# RPM Project: Youleap Launch

## 1. What do I want?

### Desired Outcome
Launch Youleap publicly and acquire 100 paying customers within 90 days.

### Success Definition
- 100 paying customers by 2026-07-30
- < 5% week-1 churn
- At least 20 inbound qualified leads per week by week 6

### Current Situation
MVP is functional in private beta with 12 design partners. Pricing is set. Landing page is draft only.

---

## 2. Why do I want it?

### Main Reason
Validate Youleap's commercial viability before committing further capital.

### Business / Personal Importance
This is the gating decision for the next funding round and for whether the team scales.

### Cost of Not Doing It
Burn continues without market signal; team morale drops; competitor closes the window.

### Opportunity
First-mover position in a category that has no clear leader yet.

---

## 3. How can that be done?

### Strategy
Public launch in two waves: a soft launch to the existing waitlist, then a Product Hunt + content push two weeks later.

### Execution Steps
1. Finalize landing page and pricing copy.
2. Soft-launch to waitlist (2,400 people) with a 14-day intro offer.
3. Instrument analytics — signup, activation, paid conversion.
4. Publish 3 launch posts (founder story, technical deep-dive, customer case study).
5. Product Hunt launch + paid acquisition test ($2k cap).

### MVP
Public signup with Stripe checkout, onboarding email sequence, and one core workflow working end-to-end. No team plan, no SSO, no integrations beyond Slack.

### Resources Needed
- Landing page rewrite (1 designer, 1 copywriter, 3 days)
- Analytics setup (1 engineer, 2 days)
- Launch content (3 posts, 1 week)
- $2k paid acquisition budget

### Risks / Open Questions
- Will Stripe approve the account in time? (Open — apply this week.)
- Is the onboarding flow good enough for cold signups, or only warm waitlist?
- Do we need a refund policy before launch?

---

## 4. Who will do it?

### Owner
{Your Name}

### Participants

| Role | Person | Responsibility |
|---|---|---|
| Owner | {Your Name} | Overall delivery and go/no-go calls |
| Product | ___ | Onboarding flow + pricing page |
| Development | ___ | Stripe + analytics instrumentation |
| Design | ___ | Landing page + launch assets |
| Marketing | ___ | Launch content + Product Hunt |
| Approval | {Your Name} | Final go/no-go on launch day |

### Approval
{Your Name} approves the launch-day go/no-go.

---

## 5. When will it be done?

### Timeline

| Milestone | Date | Owner | Output |
|---|---|---|---|
| Start | 2026-05-01 | {Your Name} | RPM signed off, owners assigned |
| Stripe + analytics live | 2026-05-08 | Dev | Working checkout, dashboards |
| Landing page v2 live | 2026-05-12 | Design | Public landing page |
| Soft launch to waitlist | 2026-05-15 | {Your Name} | 14-day intro live |
| Product Hunt launch | 2026-05-29 | Marketing | PH page + content posts |
| 90-day review | 2026-07-30 | {Your Name} | Customers report, churn, learnings |

### Next Action
Apply for the Stripe production account today and assign the Product owner by end of day tomorrow.

---

## Summary
Launch Youleap in two waves over 30 days, targeting 100 paying customers in 90 days. Soft launch to waitlist on 2026-05-15, Product Hunt on 2026-05-29. Owner: {Your Name}. Biggest risk: Stripe approval timing.
```
