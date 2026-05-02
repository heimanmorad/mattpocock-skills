# Run the Business Weekly Planner Reference

Use this reference before writing the final weekly operating plan.

This skill is for weekly operational control, not strategic transformation.

Run the Business asks:

> How do we keep the business healthy, stable, responsive, and under control this week?

RPM asks:

> What important change are we leading that will move the company forward?

Keep this distinction visible throughout the conversation.

---

## Final Output Template

For Hebrew output, wrap the full file in:

```md
<div dir="rtl" align="right">

...content...

</div>
```

Use this structure:

```md
# Run the Business Weekly Plan: {Area / Manager / Week}

## 1. Area of Responsibility

### Manager
{Name}

### Area
{Area}

### What this manager owns this week
{Clear description}

### Assumptions
{Only include if assumptions were made}

---

## 2. Current Operating Health

| Area | Status | Reason |
|---|---|---|
| {Area} | Green / Yellow / Red | {Reason} |

### Main Operating Concern
{Main concern}

---

## 3. Key Metrics for the Week

| Metric | Current State | Weekly Target | Owner | Risk |
|---|---|---|---|---|
| {Metric} | {Current} | {Target} | {Owner} | Green / Yellow / Red |

---

## 4. Must-Win Priorities

| Priority | Why It Matters | Expected Result | Owner | Deadline | Status |
|---|---|---|---|---|---|
| {Priority} | {Reason} | {Result} | {Owner} | {Date / Time} | Green / Yellow / Red |

---

## 5. Risks and Escalations

| Risk | Impact | Escalation Trigger | Owner | Action |
|---|---|---|---|---|
| {Risk} | {Impact} | {Trigger} | {Owner} | {Action} |

---

## 6. Team Focus

### Focus This Week
{What the team should focus on}

### Stop / Avoid This Week
{What should not distract the team}

### People Who Need Support
{Names / roles}

---

## 7. Daily Check Rhythm

### Daily Check Time
{Time}

### Daily Questions

1. What is abnormal today?
2. What is stuck?
3. Which customer / issue is at risk?
4. What must be closed today?
5. Who needs help?

---

## 8. End-of-Week Review

At the end of the week, review:

1. What improved?
2. What got worse?
3. What was closed?
4. What is still stuck?
5. What risk remains?
6. What should change next week?

---

## 9. Potential RPM Candidates

| Recurring Issue | Why It May Need RPM | Possible RPM |
|---|---|---|
| {Issue} | {Reason} | {Possible RPM} |

---

## 10. Summary

This week will be considered successful if:

{Clear success definition}

Main focus:

{Main focus}

Main risk:

{Main risk}

First action:

{Immediate next action}
```

---

## Opening Question

Start with this question when the area is not already clear:

### Hebrew

איזה מנהל / תחום אתה רוצה לתכנן עבורו את השבוע?

למה זה חשוב:  
כדי לבנות תוכנית Run the Business צריך קודם לדעת מה גבולות האחריות של המנהל — שירות, מכירות, פיתוח, תפעול, DevOps, SEO, לקוחות או תחום אחר.

אפשרויות לדוגמה:

1. מנהל שירות לקוחות
2. מנהל מכירות
3. מנהל פיתוח
4. מנהל תפעול לקוחות
5. מנהל DevOps / תשתיות
6. מנהל SEO / ביצועים
7. אחר — תכתוב לי מי ומה התחום

תבחר אחד, תערוך, או תכתוב תחום אחר.

### English

Which manager or operating area do you want to plan the week for?

Why it matters:  
A useful Run the Business plan starts by defining the manager's operating responsibility: service, sales, development, operations, DevOps, SEO, customer success, or another area.

Options:

1. Customer Service Manager
2. Sales Manager
3. Development Manager
4. Customer Operations Manager
5. DevOps / Infrastructure Manager
6. SEO / Performance Manager
7. Other — tell me the manager and area

Pick one, edit, or replace.

---

## Conversation Flow

Ask one question at a time.

Recommended order:

1. Area and manager
2. Current operating health
3. 3–5 key metrics
4. 3–5 must-win priorities
5. Risks and escalation triggers
6. Team focus
7. Daily check rhythm
8. End-of-week review criteria
9. Potential RPM candidates
10. Save the final file

Do not over-question. If the user gives enough signal, make reasonable assumptions and mark them clearly.

---

## Question Bank

### 1. Area of Responsibility

Questions:

- Which area does this manager own this week?
- What is included in their responsibility?
- What is outside their responsibility?
- Which part of the operation must be under control by the end of the week?

Suggestions:

1. Customer service: tickets, response time, repeated issues, strategic customers at risk.
2. Sales: leads, meetings, proposals, stuck deals, expected closes.
3. Development: bugs, releases, blocked tasks, customer-impacting issues.
4. DevOps: uptime, incidents, performance, security, monitoring.
5. Customer operations: onboarding, open customer tasks, SLA, handoffs.

### 2. Current Operating Health

Questions:

- What is the current status: Green, Yellow, or Red?
- What is currently healthy?
- What is under pressure?
- What is already at risk?
- What cannot be ignored this week?

Status definitions:

- Green — healthy and under control.
- Yellow — under pressure but manageable.
- Red — unstable, stuck, overloaded, or risky.

Challenge vague answers:

- "What specific signal makes it Yellow?"
- "Which metric or customer shows the risk?"
- "What would make it Green by the end of the week?"

### 3. Key Metrics

Questions:

- What are the 3–5 most important operating metrics for this area?
- Which metric warns us early that something is wrong?
- What is the target for each metric this week?
- Which metric matters but is not currently measured?

Metric rule:

Choose 3–5 only. More than that usually creates noise.

### 4. Must-Win Priorities

Questions:

- What are the 3–5 operational wins that must happen this week?
- What would make this week a success?
- What must be closed by Thursday?
- What must be closed to prevent pressure next week?

Priority format:

- Priority
- Why it matters
- Expected result
- Owner
- Deadline
- Status

Challenge vague priorities:

- "Close things" → "Which things, how many, and by when?"
- "Improve service" → "Which service metric should improve this week?"
- "Follow up with leads" → "How many leads, which segment, and what result?"

### 5. Risks and Escalations

Questions:

- Which customer is at risk?
- Which SLA may be breached?
- Which task is stuck?
- Which dependency may delay the team?
- What has been stuck for more than 48 hours?
- What needs management escalation?

Escalate when:

- A strategic customer is at risk.
- SLA is breached or likely to be breached.
- An issue affects many customers.
- A blocker lasts more than 48 hours.
- Revenue is at risk.
- There is a production issue.
- The manager cannot resolve it alone.

### 6. Team Focus

Questions:

- What should the team focus on this week?
- What should the team stop doing or avoid this week?
- Who is overloaded?
- Who needs support?
- Who owns each important area?

The goal is focus, not a task dump.

### 7. Daily Check Rhythm

Questions:

- When will the daily check happen?
- Who must participate?
- What five questions will be asked every day?
- What must come out of each daily check?

Recommended daily questions:

1. What is abnormal today?
2. What is stuck?
3. Which customer / issue is at risk?
4. What must be closed today?
5. Who needs help?

Keep it to 5–10 minutes.

### 8. End-of-Week Review

Questions:

- What will we check at the end of the week?
- What should improve?
- What should be closed?
- What remaining risk is acceptable?
- What will we learn for next week?

Review questions:

1. What improved?
2. What got worse?
3. What was closed?
4. What is still stuck?
5. What risk remains?
6. What should change next week?

### 9. Potential RPM Candidates

Questions:

- Which problem repeats too often?
- What is wasting too much team time?
- What issue affects many customers?
- What depends too much on one person?
- What process is broken?
- What could be solved with automation, process, training, or system redesign?

A Run the Business issue may become RPM if:

- It repeats every week.
- It wastes a lot of team time.
- It affects many customers.
- It creates revenue risk.
- It depends too much on one person.
- It shows that a process is broken.
- It could be solved with a system, automation, training, or process redesign.

---

## Operating Metrics by Area

### Customer Service

Use 3–5:

- Open tickets
- First response time
- Average resolution time
- Overdue tickets
- Repeated ticket topics
- Strategic customers at risk
- Tickets older than 72 hours
- Escalated tickets

### Sales

Use 3–5:

- New leads
- Scheduled meetings
- Proposals sent
- Deals expected to close
- Stuck deals
- Conversion rate
- Revenue forecast
- Follow-ups completed

### Development

Use 3–5:

- Critical bugs
- Open tasks by priority
- Releases planned
- Blocked tasks
- Customer-impacting issues
- Bug resolution time
- Deployment readiness
- Regression issues

### DevOps / Infrastructure

Use 3–5:

- System availability
- Critical incidents
- Performance issues
- Security risks
- Recovery time
- Monitoring gaps
- Error rates
- Slow endpoints / services

### Customer Operations / Onboarding

Use 3–5:

- Open customer tasks
- Customers waiting too long
- SLA breaches
- Blocked onboarding
- Internal handoff issues
- Go-live readiness
- Delayed setups

### SEO / Performance

Use 3–5:

- Performance scores
- Pages with critical issues
- Indexing problems
- Core Web Vitals issues
- SEO blockers
- Slow templates / pages
- Customer-impacting drops

---

## Must-Win Priority Examples

### Customer Service

- Close all strategic customer tickets older than 72 hours.
- Reduce overdue tickets from 40 to 20.
- Identify the top 3 repeated ticket topics.
- Escalate all stuck tickets older than 48 hours.

### Sales

- Follow up with all open proposals from last week.
- Close 3 deals currently in final stage.
- Schedule 10 qualified meetings.
- Identify the top reason deals are stuck.

### Development

- Close 2 critical customer-impacting bugs.
- Release the pending fix by Wednesday.
- Unblock all tasks waiting for product decision.
- Reduce regression bugs in the current release.

### DevOps

- Resolve the highest-risk monitoring gap.
- Review all incidents from last week.
- Fix the main performance bottleneck.
- Define escalation path for production incidents.

### Customer Operations

- Move all blocked onboarding customers to next step.
- Close all setup tasks older than 7 days.
- Identify handoff bottlenecks between sales and operations.
- Prepare go-live checklist for customers launching this week.

---

## Run the Business vs RPM Examples

| Run the Business Issue | Possible RPM |
|---|---|
| Many repeated tickets about the same topic | Reduce repeated tickets by 30% |
| Sales deals stuck after proposal | Improve close rate from 20% to 30% |
| Bugs keep coming back from the same area | Reduce recurring bugs in area X by 50% |
| Onboarding tasks get delayed every week | Build a 7-day onboarding operating system |
| Customer setup depends on one person | Create a repeatable setup SOP and backup owner |
| Performance issues are found manually | Build proactive performance monitoring |

---

## Quality Checklist

A good Run the Business weekly plan is:

- Specific
- Measurable
- Realistic for one week
- Owned by clear people
- Focused on operational health
- Not overloaded
- Connected to customer, revenue, SLA, or team health
- Clear about risks
- Clear about escalation
- Clear about the first action

Reject or challenge plans that are:

- Too vague
- Too broad
- Not measurable
- Just a task dump
- Missing owners
- Missing deadlines
- Missing risks
- Disconnected from actual business health

---

## Suggested File Naming

Use lowercase English slugs.

Format:

```txt
projects/run-the-business/{area}-{yyyy-mm-dd}.md
```

Examples:

- `projects/run-the-business/customer-service-2026-05-03.md`
- `projects/run-the-business/sales-2026-05-03.md`
- `projects/run-the-business/devops-2026-05-03.md`
- `projects/run-the-business/customer-operations-2026-05-03.md`
