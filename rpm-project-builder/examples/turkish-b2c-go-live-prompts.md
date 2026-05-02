# Claude Code Prompts — Turkish B2C Go-Live Finishes

## 1. פתיחת הפרויקט והבנת המצב

Read the RPM project document:

`projects/rpm/turkish-b2c-go-live-finishes.md`

Then scan the codebase and create a practical go-live technical checklist for the Turkish B2C site.

Focus on these areas:
1. Performance / Speed
2. SSR / SEO
3. Commercial UX
4. Design / Brand Trust
5. Analytics / Tracking
6. Backend / Ops

For each item, classify it as:
- Blocker
- Important but not blocker
- After launch

Do not change code yet. First give me the checklist and your recommended execution order.

---

## 2. למצוא בעיות SSR / SEO

Audit the Turkish B2C product and category pages for SSR and SEO readiness.

Please check:
- Product page HTML is available on initial server render
- Category page HTML is available on initial server render
- Metadata exists and is dynamic where needed
- title / description are correct
- canonical exists
- Open Graph fields exist
- no important content depends only on client-side rendering
- internal navigation does not cause missing content or delayed content

Return:
1. Findings
2. Blockers
3. Recommended fixes
4. Files that need changes

Do not change code until I approve.

---

## 3. לבדוק את בעיית המעבר הפנימי

We noticed that direct entry to a product page shows content, but internal navigation sometimes shows a delay or missing content before the page appears.

Investigate this behavior in the Turkish B2C site.

Check:
- Link navigation
- loading.tsx / suspense boundaries
- client components that fetch data after navigation
- cache behavior
- route segment structure
- hydration issues
- whether product data is fetched server-side or client-side

Return the likely root cause and a minimal fix plan.

Do not change code yet.

---

## 4. בדיקת Performance

Run a performance-oriented audit of the Turkish B2C storefront.

Focus on:
- bundle size
- unnecessary client components
- large images
- image optimization
- fonts
- blocking scripts
- repeated API calls
- cache usage
- server data fetching
- route-level loading behavior

Give me:
1. Top 10 performance issues
2. Estimated impact
3. Quick wins
4. Deeper fixes
5. Which changes are safe before go-live

Do not change code yet.

---

## 5. להפוך את זה לצ׳קליסט אמיתי בקובץ

Create a launch checklist file based on the RPM project.

File path:
`projects/go-live/turkish-b2c-launch-checklist.md`

Use this structure:

# Turkish B2C Go-Live Checklist

## Performance / Speed
| Item | Owner | Status | Blocker? | Due Date | Notes |

## SSR / SEO
| Item | Owner | Status | Blocker? | Due Date | Notes |

## Commercial UX
| Item | Owner | Status | Blocker? | Due Date | Notes |

## Design / Brand Trust
| Item | Owner | Status | Blocker? | Due Date | Notes |

## Analytics / Tracking
| Item | Owner | Status | Blocker? | Due Date | Notes |

## Backend / Ops
| Item | Owner | Status | Blocker? | Due Date | Notes |

Use the owners from the RPM:
- Project Owner: Shirel
- Approval: Morad
- Frontend / UX: Sarit
- SEO / Performance: Shirel
- Backend / Ops: Amir
- QA / Demo Readiness: Shimi, Sarit, Shirel

Do not invent too many items. Create a practical checklist for go-live by May 10, 2026.

---

## 6. להריץ QA תרחישי לקוח

Create a manual QA test plan for the Turkish B2C go-live.

The QA should simulate a real customer journey:

1. Homepage
2. Category page
3. Search / filters
4. Product page
5. Add to cart
6. Cart
7. Checkout
8. Order confirmation
9. Email / notification
10. Back office order verification

For each step include:
- What to test
- Expected result
- Possible failure signs
- Blocker or not
- Owner

Save it as:
`projects/go-live/turkish-b2c-qa-plan.md`

---

## 7. לבדוק Analytics / Tracking

Audit the Turkish B2C site for analytics and tracking readiness.

Check whether we have:
- GA4 installed
- page_view tracking
- product_view event
- category_view event
- add_to_cart event
- begin_checkout event
- purchase event
- error tracking
- consent / privacy considerations if relevant

Return:
1. What exists
2. What is missing
3. What must be fixed before go-live
4. Suggested implementation plan

Do not change code yet.

---

## 8. לבדוק Backend / Ops

Audit the Turkish B2C backend and operational readiness.

Check:
- Products are loading correctly
- Prices are correct
- Inventory is available
- Variants work
- Shipping options work
- Payment flow works
- Order creation works
- Emails / notifications work
- Back office can see and process orders
- Errors are logged clearly

Return:
1. Current status
2. Blockers
3. Risks
4. Recommended fixes
5. What needs manual confirmation from Amir

Do not change code yet.

---

## 9. לבקש ממנו לתקן בפועל אחרי אישור

Based on the checklist you created, start with the highest-impact blocker in SSR / SEO or Performance.

Before changing code:
1. Explain the exact issue
2. Explain the minimal fix
3. List the files you will edit
4. Tell me how we will test it

Then wait for my approval.

---

## 10. אחרי שהוא מציע שינוי — לתת אישור מוגבל

Approved.

Make only the minimal change you described.

After changing code:
1. Show me the diff summary
2. Explain how to test locally
3. Mention any risk
4. Do not continue to the next issue without asking me.

---

## 11. בדיקת "האם מביך להראות ללקוח?"

Act as a strict demo readiness reviewer.

Review the Turkish B2C storefront as if we are showing it to a strategic customer tomorrow.

Look for anything that feels:
- unfinished
- slow
- visually weak
- confusing
- technically broken
- bad for trust
- bad for conversion
- embarrassing in a sales demo

Return:
1. Must fix before demo
2. Should fix soon
3. Acceptable for launch
4. Strong points we should highlight in the demo

Be direct and critical.

---

## 12. הכנת הודעת סטטוס לצוות

Based on the current checklist and known blockers, draft a short Hebrew status update for the team.

Include:
- Current goal
- Go-live date: May 10, 2026
- Go / No-Go date: May 7, 2026
- Main blockers
- Owners
- What must happen today
- What decisions are needed from Morad

Make it clear, direct, and action-oriented.

---

## 13. הכנת Daily War Room

Create a daily War Room template for the Turkish B2C go-live week.

The template should include:
1. Yesterday completed
2. Today must close
3. Blockers
4. Owner per blocker
5. Decision needed
6. Risk to May 10 go-live
7. End-of-day commitment

Save as:
`projects/go-live/turkish-b2c-war-room-template.md`

---

## 14. בדיקת Lighthouse / Core Web Vitals

Help me prepare a Lighthouse and Core Web Vitals validation checklist for the Turkish B2C site.

Include:
- Which pages to test
- Mobile vs desktop
- What scores are acceptable before go-live
- What metrics matter most
- How to document results
- What is considered a blocker

Focus on practical go-live readiness, not theoretical perfection.

---

## 15. להפוך את התוכנית למשימות GitHub Issues

Convert the RPM project and launch checklist into GitHub issues.

Group the issues by:
1. Performance / Speed
2. SSR / SEO
3. Commercial UX
4. Design / Brand Trust
5. Analytics / Tracking
6. Backend / Ops
7. QA / Demo Readiness

For each issue include:
- Title
- Description
- Owner
- Acceptance criteria
- Blocker label or not
- Due date

Do not create the issues yet. First show me the proposed list.

---

## 16. פרומפט קצר לפתיחת סשן עבודה כל בוקר

Read:
- `projects/rpm/turkish-b2c-go-live-finishes.md`
- `projects/go-live/turkish-b2c-launch-checklist.md`

Then tell me:
1. What is the highest-risk blocker today?
2. What should we fix first?
3. Who owns it?
4. What is the smallest safe change?
5. What should I ask the team now?

Do not change code yet.
