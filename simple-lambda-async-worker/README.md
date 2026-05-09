# simple-lambda-async-worker

A Claude Code skill that scaffolds a small, opinionated AWS Lambda async worker project — one HTTP endpoint that accepts a job, returns immediately, and runs the work in the background with an optional webhook callback when finished.

This README explains **when, why, and how** to use the skill. The full ruleset that Claude Code follows lives in [`SKILL.md`](./SKILL.md).

---

## What it generates

When invoked, the skill produces six files and nothing else:

```
serverless.yml
package.json
.env.example
README.md
src/dispatcher.js
src/worker.js
```

The architecture is fixed:

```
Client
  │  POST /run  (x-api-token header + JSON body)
  ▼
API Gateway (httpApi)
  ▼
Dispatcher Lambda
  ├─ validates auth (constant-time comparison)
  ├─ validates JSON body, task_payload, callback_url, callback_token
  ├─ generates a job_id
  ├─ invokes Worker async (InvocationType: "Event")
  └─ returns 202 immediately
                  │
                  ▼  (fire-and-forget)
            Worker Lambda
              ├─ runs the single task via runTask(taskPayload)
              └─ POSTs result to callback_url (if provided)
```

---

## When to use it

Use this skill when **all** of the following are true:

- You want a single HTTP endpoint that triggers one well-defined background task.
- The task takes seconds to a few minutes (Lambda's 15-minute hard cap applies).
- The caller does not need a synchronous result — `202 Accepted` plus an optional webhook is acceptable.
- The deployment is single-tenant: one customer, one bucket, one use case per stack.
- AWS Lambda + Serverless Framework is an acceptable platform choice.
- You want least-privilege IAM and sensible security defaults out of the box.

### Example tasks that fit the pattern

- Scan a PDF in S3 for embedded JavaScript or other suspicious constructs.
- Generate a thumbnail or image variant from a source file.
- Build a search index from a JSON document.
- Validate a file's structure or contents.
- Call a slow third-party API and forward the result via webhook.
- Transcode a small audio clip.
- Run a report and write the output to S3.

### When NOT to use it

- The task needs to return its result synchronously — use a normal API Lambda.
- The task can run longer than 15 minutes — use ECS, AWS Batch, or Step Functions.
- You need multi-tenant routing or caller-supplied bucket names — the skill explicitly forbids these.
- You need streaming responses, WebSockets, scheduled jobs, or queue-driven processing.
- You need exactly-once processing or guaranteed delivery — there is no DLQ by default; the skill notes where to add one.
- TypeScript, ESM, or a region other than `eu-west-1` is a hard requirement — the skill is opinionated against these.

---

## Why this shape

The skill makes a series of deliberate, opinionated choices to keep the resulting Lambda small, secure, and predictable.

| Choice | Why |
|--------|-----|
| **JavaScript + CommonJS** | No build step. `serverless deploy` packages `src/*.js` directly. Faster cold starts, fewer moving parts. |
| **Node.js 22 runtime** | Built-in `fetch`, modern language features, supported by AWS today. |
| **Hardcoded `eu-west-1`** | Removes a configuration knob. If you need multi-region, this is the wrong skill. |
| **Serverless Framework v4** | Standard, well-documented, supports per-function IAM via plugin. |
| **`httpApi` (HTTP API v2), not REST API** | Cheaper, faster, lowercases header keys consistently. |
| **Two Lambdas, not one** | Separation of concerns: Dispatcher validates and orchestrates; Worker does the actual work. The Dispatcher returns 202 immediately, so the API stays fast even for long tasks. |
| **`x-api-token` header, not `Authorization: Bearer`** | Simpler for service-to-service. Avoids implying JWT/OAuth semantics that aren't there. Constant-time comparison prevents timing attacks. |
| **Per-function IAM (`serverless-iam-roles-per-function`)** | The Dispatcher gets only `lambda:InvokeFunction` for the specific Worker; the Worker gets only the task-specific permissions it needs. Neither role leaks permissions to the other. |
| **`maximumRetryAttempts: 0`** | Async Lambda invocations retry by default, which causes duplicate processing, duplicate writes, and duplicate webhooks. Disabling retries keeps behavior predictable. The skill notes how to add a DLQ if task loss is unacceptable. |
| **Single tenant** | Callers send only object keys (e.g., `s3_key`), never bucket names. IAM is scoped to one bucket. This keeps the blast radius small. |
| **Manual validation, no Zod/Joi/Yup** | The validation surface is small and explicit. A handwritten `validateTaskPayload` is easier to read than a schema library for ~10 fields. |
| **Built-in `fetch`, no axios** | Node 22 has `fetch`. Adding axios is an unnecessary dependency. |
| **No tests, no linters, no extra folders by default** | The skill stays focused on producing a deployable starting point. Add what you need afterwards. |

---

## How to use it

### 1. Install the skill

Place [`SKILL.md`](./SKILL.md) in your Claude Code skills directory:

```
~/.claude/skills/simple-lambda-async-worker/SKILL.md
```

Claude Code will pick it up automatically.

### 2. Ask Claude Code to scaffold a project

Describe the task in plain language. The skill will trigger on requests like:

- *"Build a Lambda that receives an S3 key and scans the PDF in the background."*
- *"Create a Serverless Framework project with an endpoint that starts a background job and returns immediately."*
- *"I want a Lambda that takes a request, kicks off async work, and posts the result to a webhook."*

Claude Code will ask clarifying questions only when necessary — typically what the single Worker task is, and whether S3 access is needed.

### 3. Review the generated project

You will get the six files listed above. The handler files contain a clearly-marked TODO:

```js
async function runTask(taskPayload) {
  // TODO: Implement the single task here.
  // Must return a JSON-serializable object.
  // Throw an Error with a human-readable message on failure.
}
```

Implement `runTask` for your specific task. The wrapper handles `job_id`, callback delivery, error formatting, logging, and timeouts.

### 4. Deploy

```bash
export AUTH_TOKEN="your-secret-token"
# If S3 is used:
export S3_BUCKET_NAME="your-single-tenant-bucket"

npm install
npx serverless deploy --stage dev
```

Serverless prints the API Gateway endpoint URL. Use that URL in the integration; never hardcode it elsewhere.

For production:

```bash
npx serverless deploy --stage production
```

### 5. Test

The skill generates a complete `curl` example in the project's own README. The base shape is:

```bash
curl -X POST "https://xxxx.execute-api.eu-west-1.amazonaws.com/run" \
  -H "Content-Type: application/json" \
  -H "x-api-token: your-secret-token" \
  -d '{
    "task_payload": { "...": "task-specific fields" },
    "callback_url": "https://example.com/webhook",
    "callback_token": "optional-callback-token"
  }'
```

A successful dispatch returns:

```json
{ "ok": true, "status": "accepted", "job_id": "job_20260509_8f3a2c" }
```

The Worker then runs in the background. If `callback_url` was provided, the final result (success or failure) is POSTed there with `x-callback-token` if a `callback_token` was supplied.

---

## Customizing the defaults

The skill is opinionated by design. Most defaults should not be changed without good reason. The ones most commonly worth revisiting:

- **Worker memory and timeout** — heavier tasks (PDF parsing, image processing, index generation) often want more memory and longer timeouts. The skill prompts for this when the task signals it.
- **Reserved concurrency** — for long-running single-tenant Workers, set `reservedConcurrency` on the Worker so one runaway invocation cannot starve the rest of the AWS account. The skill documents this in the "Optional reliability and operational notes" section.
- **DLQ** — if task loss is unacceptable, add an SQS DLQ via the Worker's `onFailure` destination, and ensure the task is idempotent before reprocessing.
- **CORS** — off by default, since the endpoint is for service-to-service traffic. If you need browser access, configure CORS for the specific origin only — never wildcard.

For everything else (region, runtime, framework, language, retry policy, header name, response shapes), the rules in `SKILL.md` are intentional. If you find yourself wanting to change several of them at once, that's usually a signal that this skill is the wrong fit and you want a richer pattern (e.g., Step Functions, a proper microservice, or a queue-driven worker).

---

## Reference

- Full skill rules: [`SKILL.md`](./SKILL.md)
- Serverless Framework: https://www.serverless.com/framework/docs
- `serverless-iam-roles-per-function`: https://github.com/functionalone/serverless-iam-roles-per-function
- AWS Lambda async invocation behavior: https://docs.aws.amazon.com/lambda/latest/dg/invocation-async.html
