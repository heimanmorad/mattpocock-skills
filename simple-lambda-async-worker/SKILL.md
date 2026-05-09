# simple-lambda-async-worker — Claude Code Skill

## Purpose

Use this skill when the user wants to create a simple AWS Lambda flow where:

1. An API Gateway endpoint receives a request.
2. A Dispatcher Lambda validates authentication and input.
3. The Dispatcher returns an immediate response.
4. The Dispatcher invokes a Worker Lambda asynchronously.
5. The Worker performs one clearly defined task.
6. The Worker optionally sends a webhook callback when the task finishes.

This skill is designed for small, focused, single-tenant serverless jobs.

The default pattern is:

Small authenticated API Gateway endpoint  
→ Dispatcher Lambda  
→ immediate `202 Accepted` response  
→ async Worker Lambda  
→ one clear task  
→ optional webhook callback  
→ least-privilege IAM  
→ deployed with Serverless Framework.

---

## Skill name

The skill name is:

`simple-lambda-async-worker`

Recommended local path:

`~/.claude/skills/simple-lambda-async-worker/SKILL.md`

---

## When to use this skill

Use this skill when the user asks to build, scaffold, design, or implement a Lambda that:

- Receives an HTTP request.
- Returns immediately.
- Starts background work.
- Runs a task that may take seconds or a few minutes.
- Optionally reports completion to a webhook.
- Is deployed with Serverless Framework.
- Uses AWS Lambda + API Gateway.
- Should stay simple and stateless.

Example user requests:

- “Create a Lambda that receives an S3 key and scans the file in the background.”
- “Build a simple async Lambda worker.”
- “Create a Serverless Framework project with an endpoint that starts a background job.”
- “I want a Lambda that returns immediately and calls another Lambda.”
- “Build a stateless Lambda that processes a file and sends a webhook when done.”

---

## Hard requirements

These rules are mandatory unless the user explicitly changes the architecture.

### Runtime and language

- Always use JavaScript.
- Always use CommonJS.
- Always use Node.js Lambda runtime:

`nodejs22.x`

Handler style:

    module.exports.handler = async (event) => {
      // ...
    };

Do not use TypeScript by default.  
Do not use ESM by default.  
Do not set `"type": "module"` in `package.json`.

---

### AWS region

Always deploy to Ireland:

`eu-west-1`

In `serverless.yml`, set this as a hardcoded provider region:

    region: eu-west-1

Do not make region configurable unless the user explicitly asks.

---

### Deployment framework

Always use Serverless Framework.

Target Serverless Framework v4 by default.

Use:

    npx serverless deploy

For production:

    npx serverless deploy --stage production

---

### API Gateway

The external endpoint must be API Gateway.

Use Serverless Framework `httpApi`, not REST API, unless the user explicitly asks otherwise.

Default endpoint:

`POST /run`

Example:

    functions:
      someTaskDispatcher:
        handler: src/dispatcher.handler
        events:
          - httpApi:
              path: /run
              method: post

---

### Architecture

Always create two separate Lambda handlers:

1. Dispatcher Lambda
2. Worker Lambda

Use two separate files:

    src/dispatcher.js
    src/worker.js

Do not put both handlers in one file by default.

---

### Single tenant

The generated Lambda project is single tenant.

That means:

- One deployment serves one tenant/customer/use case.
- Environment variables are specific to that tenant.
- IAM permissions are scoped to that tenant.
- If S3 is used, access is limited to one specific bucket.
- Avoid dynamic cross-tenant behavior inside the Lambda.
- Do not allow the caller to choose arbitrary buckets.

---

## Required project structure

Always create this project structure:

    serverless.yml
    package.json
    .env.example
    README.md
    src/dispatcher.js
    src/worker.js

Do not add tests, linting, extra folders, or frameworks unless the user asks.

---

## Naming rules

### Skill name

The skill is named:

`simple-lambda-async-worker`

### Serverless service name

The Serverless `service` name should be derived from the user’s specific task in kebab-case.

Examples:

- PDF Security Scanner → `pdf-security-scanner`
- Product Index Builder → `product-index-builder`
- File Validator → `file-validator`

Do not use `simple-lambda-async-worker` as the service name unless the user is only creating a generic scaffold.

### Function names

Function names in `serverless.yml` should be descriptive and derived from the task.

Always use the suffixes:

- `Dispatcher`
- `Worker`

Examples:

    pdfScannerDispatcher
    pdfScannerWorker

    productIndexDispatcher
    productIndexWorker

    fileValidatorDispatcher
    fileValidatorWorker

---

## Stage rules

Use this default stage configuration:

    stage: ${opt:stage, 'dev'}

The official production stage name is:

`production`

README examples must use:

    npx serverless deploy --stage dev
    npx serverless deploy --stage production

Do not use `prod` as the production stage name.

---

## Endpoint rule

Never hardcode the final API Gateway endpoint URL in code.

The final endpoint is taken from Serverless deploy output.

The README should explain:

After running:

    npx serverless deploy --stage production

Serverless will print the endpoint, for example:

    POST https://xxxx.execute-api.eu-west-1.amazonaws.com/run

Use that URL in the integration.

---

## Dispatcher Lambda responsibilities

The Dispatcher Lambda must:

1. Receive the HTTP request from API Gateway.
2. Validate the auth token from the request header.
3. Parse and validate the JSON body.
4. Validate the generic wrapper payload.
5. Validate the task-specific payload.
6. Generate a `job_id`.
7. Invoke the Worker Lambda asynchronously.
8. Return immediately with `202 Accepted`.

The Dispatcher must not perform the heavy task.

The Dispatcher is only responsible for validation, orchestration, and async dispatch.

---

## Worker Lambda responsibilities

The Worker Lambda must:

1. Receive the async event from the Dispatcher.
2. Extract `job_id`, `task_payload`, `callback_url`, and `callback_token`.
3. Run the task by calling:

    runTask(taskPayload)

4. Send webhook callback if `callback_url` exists.
5. Send success callback if the task succeeds.
6. Send failure callback if the task fails.
7. Log clearly with `job_id`.
8. Avoid logging secrets.

The Worker is the only Lambda that performs the actual task.

---

## Request authentication

Every external request to the Dispatcher must include:

    x-api-token: <token>

The expected token is stored in the Lambda environment variable:

    AUTH_TOKEN

The Dispatcher compares:

    event.headers["x-api-token"]

against:

    process.env.AUTH_TOKEN

The skill must not use `Authorization: Bearer` by default.

Reason:

- `x-api-token` is simpler for service-to-service calls.
- It avoids implying JWT/OAuth behavior.
- It keeps the pattern simple.

---

## Secret management

Do not write real secrets into code.

Do not commit real tokens.

In `serverless.yml`, use:

    AUTH_TOKEN: ${env:AUTH_TOKEN}

The real value is provided at deploy time from:

- Local shell environment.
- Local `.env` file that is not committed.
- CI/CD secret, such as GitHub Actions Secrets.

Example local deploy:

    export AUTH_TOKEN="very-secret-token"
    npx serverless deploy --stage dev

`.env.example` should include only placeholders.

---

## External request body format

The external HTTP request body to the Dispatcher must use this standard structure:

    {
      "task_payload": {},
      "callback_url": "https://example.com/webhook",
      "callback_token": "optional-callback-token"
    }

Rules:

- `task_payload` is required.
- `task_payload` must be a plain JSON object.
- `task_payload` must not be `null`.
- `task_payload` must not be an array.
- `task_payload` must not be a string, number, or boolean.
- `callback_url` is optional.
- `callback_token` is optional.
- `callback_token` is allowed only if `callback_url` exists.
- `job_id` is always generated by the Dispatcher.
- The caller does not provide `job_id`.

---

## Body parsing rules

The Dispatcher must parse the API Gateway body.

If the body is empty or invalid JSON, return:

HTTP status:

`400 Bad Request`

Response:

    {
      "ok": false,
      "error": "Invalid JSON body"
    }

Do not silently accept an empty body.

---

## Generic validation rules

The Dispatcher must validate:

1. Auth token.
2. JSON body.
3. `task_payload`.
4. `callback_url`.
5. `callback_token`.

### `task_payload`

Required.

Must be a plain JSON object.

Invalid examples:

    {
      "task_payload": null
    }

    {
      "task_payload": []
    }

    {
      "task_payload": "abc"
    }

### `callback_url`

Optional.

If provided, it must be a valid HTTPS URL.

It must start with:

`https://`

Do not allow `http://` by default.

**SSRF note:**

`callback_url` is caller-controlled. If the Lambda runs in a VPC or can reach internal services, a malicious caller could send an internal URL as the callback target. For untrusted callers, warn the user to consider an allowlist of permitted callback domains. Do not implement an allowlist by default, but raise this as a concern if the caller base is not fully trusted.

### `callback_token`

Optional.

If provided:

- Must be a string.
- Must not be empty.
- Must be 500 characters or fewer.
- Must only be allowed when `callback_url` is also provided.

If `callback_token` is sent without `callback_url`, return:

HTTP status:

`400 Bad Request`

---

## Task-specific validation

Always create a task-specific validation function in `src/dispatcher.js`:

    function validateTaskPayload(taskPayload) {
      // Validate task-specific fields here.
      return { ok: true };
    }

Examples:

For a PDF scanner:

    function validateTaskPayload(taskPayload) {
      if (!taskPayload.s3_key || typeof taskPayload.s3_key !== "string") {
        return { ok: false, error: "task_payload.s3_key is required" };
      }

      if (!taskPayload.s3_key.toLowerCase().endsWith(".pdf")) {
        return { ok: false, error: "task_payload.s3_key must point to a PDF file" };
      }

      return { ok: true };
    }

Do not use external validation libraries such as:

- `zod`
- `joi`
- `yup`

Use simple manual validation to keep dependencies minimal.

---

## Dispatcher to Worker payload

The Dispatcher must invoke the Worker with this structure:

    {
      "job_id": "job_...",
      "task_payload": {},
      "callback_url": "https://example.com/webhook",
      "callback_token": "optional-callback-token"
    }

Rules:

- `job_id` is generated by the Dispatcher.
- `task_payload` is the cleaned, validated task payload.
- `callback_url` is included only if provided.
- `callback_token` is included only if provided.
- Do not pass headers.
- Do not pass the auth token.
- Do not pass the full API Gateway event.
- Do not pass unvalidated raw body.

---

## Job ID rules

The Dispatcher always generates `job_id`.

Do not depend on the caller to send it.

Use a simple format:

`job_<timestamp>_<random>`

Example:

`job_20260509_8f3a2c`

The `job_id` must be:

- Returned immediately to the caller.
- Logged by the Dispatcher.
- Passed to the Worker.
- Included in webhook callbacks.

---

## Immediate response rules

On successful dispatch, the Dispatcher returns immediately with:

HTTP status:

`202 Accepted`

Response:

    {
      "ok": true,
      "status": "accepted",
      "job_id": "job_..."
    }

This response means:

- The request was valid.
- The Worker was invoked.
- The task is running asynchronously.
- The final result may arrive via webhook.

The Dispatcher must not wait for the Worker result.

---

## HTTP response standards

All responses must be JSON.

Always include:

    Content-Type: application/json

Use these HTTP status codes:

- `202` — request accepted and Worker invoked.
- `400` — invalid input.
- `401` — missing or invalid auth token.
- `500` — internal failure, such as failure to invoke Worker.

---

## Worker invocation

The Dispatcher must invoke the Worker asynchronously using AWS SDK v3:

    InvocationType: "Event"

Use:

    @aws-sdk/client-lambda

The Worker function name must come from environment variable:

    WORKER_FUNCTION_NAME

Do not hardcode the Worker function name inside `dispatcher.js`.

---

## IAM scoping

Each function gets its own IAM role.

By default Serverless Framework gives every function in a service the same role. That violates least-privilege: if the Worker needs `s3:GetObject`, the Dispatcher inherits it too.

Use the `serverless-iam-roles-per-function` plugin to give each function its own role. Declare permissions per function with `iamRoleStatements`.

### Plugin setup

Add to `package.json` devDependencies:

    "serverless-iam-roles-per-function": "^3.2.0"

Add to `serverless.yml`:

    plugins:
      - serverless-iam-roles-per-function

### Dispatcher permissions

The Dispatcher gets only `lambda:InvokeFunction`, scoped to the specific Worker function of the same service/stage:

    pdfScannerDispatcher:
      iamRoleStatements:
        - Effect: Allow
          Action:
            - lambda:InvokeFunction
          Resource:
            - arn:aws:lambda:${aws:region}:${aws:accountId}:function:${self:service}-${sls:stage}-pdfScannerWorker

Do not give the Dispatcher:

- `lambda:*`
- access to all functions
- account-wide Lambda permissions
- task-specific permissions such as S3, DynamoDB, SQS

### Worker permissions

The Worker gets only the task-specific permissions it needs.

If the task needs no AWS resources, use:

    pdfScannerWorker:
      iamRoleStatements: []

If the task needs S3, see the S3 IAM permissions section.

Do not give the Worker `lambda:InvokeFunction`. Only the Dispatcher invokes the Worker.

---

## Worker async retry behavior

Disable automatic async retries for the Worker by default.

Set:

    maximumRetryAttempts: 0

Reason:

- Prevent duplicate processing.
- Prevent duplicate writes.
- Prevent duplicate webhooks.
- Keep the simple worker model predictable.

If a task requires retries, the user must explicitly request that behavior and the task must be designed to be idempotent.

---

## Optional reliability and operational notes

These are not added by default. Mention them to the user when the task warrants the tradeoff.

### Dead letter queue

With `maximumRetryAttempts: 0`, failed Worker invocations are silently dropped.

If task loss is unacceptable, add an SQS DLQ to the Worker via the `onFailure` destination, and ensure the task is idempotent before reprocessing.

### Reserved concurrency

For long-running single-tenant Workers (timeout near 5 minutes), set reserved concurrency on the Worker to prevent one runaway invocation from starving the rest of the AWS account:

    exampleTaskWorker:
      handler: src/worker.handler
      reservedConcurrency: 5

Pick a value that matches expected peak load for the tenant.

### Base64-encoded request bodies

API Gateway HTTP API v2 may set `event.isBase64Encoded: true` for some content types. For `application/json` it usually does not, but if the caller sends an unexpected content type the body may be base64.

The default `parseJsonBody` does not decode base64. If the user expects base64 bodies, decode first:

    if (event.isBase64Encoded) {
      return JSON.parse(Buffer.from(event.body, "base64").toString("utf8"));
    }

Do not add this by default.

---

## Failure to invoke Worker

If the Dispatcher fails to invoke the Worker:

Return:

HTTP status:

`500 Internal Server Error`

In `production`, return a generic error:

    {
      "ok": false,
      "error": "Failed to start worker"
    }

In `dev`, the response may include error details:

    {
      "ok": false,
      "error": "Failed to start worker",
      "details": "Actual AWS/client error message"
    }

Always log the detailed error internally with `job_id`.

Do not expose sensitive information.

---

## Worker structure

The Worker must contain a task wrapper and a task function.

The task function must be named:

    runTask(taskPayload)

The wrapper handles:

- `job_id`
- `callback_url`
- `callback_token`
- try/catch
- logs
- success callback
- failure callback
- webhook timeout

`runTask` handles only the task-specific business logic.

---

## `runTask` rules

The Worker must call:

    const result = await runTask(task_payload);

`runTask` receives one JSON object:

    runTask(taskPayload)

The `taskPayload` contains all task-specific parameters.

`runTask` must not receive:

- API Gateway event.
- Headers.
- Auth token.
- Callback URL.
- Callback token.
- Full Lambda event.

`runTask` should not send the webhook itself.

The Worker wrapper sends the webhook using the result returned by `runTask`.

---

## `runTask` return value

`runTask` must return a JSON-serializable object.

Good:

    return {
      success: true,
      findings: [],
      summary: "Task completed"
    };

Bad:

- Buffer
- Stream
- Class instance
- HTTP response object
- Circular object
- Non-serializable value

If the task produces a large file, save it to S3 and return metadata only.

Example:

    return {
      output_s3_key: "results/job_123/output.json",
      size_bytes: 12345
    };

The Worker wraps the return value into:

    {
      "ok": true,
      "job_id": "job_...",
      "status": "completed",
      "result": {}
    }

---

## `runTask` error handling

If the task fails, `runTask` should throw an error:

    throw new Error("Human readable error message");

Do not return:

    { "ok": false, "error": "..." }

The Worker wrapper catches the error and sends a failed callback if needed.

Failure callback format:

    {
      "ok": false,
      "job_id": "job_...",
      "status": "failed",
      "error": "Human readable error message"
    }

The callback error message should be human-readable.

Do not include stack traces in the callback.

Detailed errors may be logged in CloudWatch, without secrets.

---

## Webhook callback rules

`callback_url` is optional.

If it exists, the Worker sends a webhook when the task finishes.

The Worker sends webhook for both:

- Success
- Failure

### Success callback

    {
      "ok": true,
      "job_id": "job_...",
      "status": "completed",
      "result": {}
    }

### Failure callback

    {
      "ok": false,
      "job_id": "job_...",
      "status": "failed",
      "error": "Human readable error message"
    }

### Callback token

If `callback_token` was provided in the original request, the Worker sends it as a header:

    x-callback-token: <callback_token>

Do not reuse `AUTH_TOKEN` for callback authentication.

### Webhook timeout

Use a fixed timeout in code:

    const CALLBACK_TIMEOUT_MS = 5000;

The Worker attempts the webhook once.

If the webhook call fails:

- Log the error.
- Do not retry.
- Do not fail the whole Lambda because the callback failed.

Reason:

The task already finished. Failed callback delivery should not trigger duplicate task execution.

---

## CORS

Default:

No CORS.

Reason:

This endpoint is intended for system-to-system calls using `x-api-token`.

If the user explicitly needs browser access, ask for the allowed origin and configure CORS for that specific origin only.

Do not set wildcard CORS by default.

Avoid:

    allowedOrigins:
      - "*"

---

## Logging rules

Use simple CloudWatch logs.

Always include `job_id` where available.

Good examples:

    console.log(`[${jobId}] Request accepted`);
    console.log(`[${jobId}] Worker invoked successfully`);
    console.log(`[${jobId}] Worker started`);
    console.log(`[${jobId}] Task completed`);
    console.error(`[${jobId}] Task failed`, error);

Do not log:

- Auth token.
- Callback token.
- Full headers.
- Sensitive payload fields.
- Secrets.
- Full environment variables.

Payload logging should be minimal and task-specific.

---

## S3 rules

The skill must not assume S3 is needed.

If the task clearly does not need S3, do not add S3 permissions, S3 env vars, or S3 dependencies.

If S3 need is unclear, ask the user:

“Does this Lambda need S3 access?”

Offer these options:

1. No S3 access.
2. Read only — `s3:GetObject`.
3. Write only — `s3:PutObject`.
4. Read and write — `s3:GetObject` + `s3:PutObject`.

Recommend the least-privilege option.

---

## S3 bucket rule

If S3 is used, the bucket is single tenant.

The bucket name must come from environment variable:

    S3_BUCKET_NAME

The caller should usually send only object keys, such as:

    {
      "task_payload": {
        "s3_key": "uploads/file.pdf"
      }
    }

Do not let the external caller choose arbitrary bucket names.

Do not accept `s3_bucket` from external input unless the user explicitly asks and understands the security tradeoff.

---

## S3 IAM permissions

Add S3 permissions only when needed, and attach them to the Worker function only — never the Dispatcher.

### Read only

    pdfScannerWorker:
      iamRoleStatements:
        - Effect: Allow
          Action:
            - s3:GetObject
          Resource:
            - arn:aws:s3:::${env:S3_BUCKET_NAME}/*

### Write only

    pdfScannerWorker:
      iamRoleStatements:
        - Effect: Allow
          Action:
            - s3:PutObject
          Resource:
            - arn:aws:s3:::${env:S3_BUCKET_NAME}/*

### Read and write

    pdfScannerWorker:
      iamRoleStatements:
        - Effect: Allow
          Action:
            - s3:GetObject
            - s3:PutObject
          Resource:
            - arn:aws:s3:::${env:S3_BUCKET_NAME}/*

Do not use broad S3 permissions.

Avoid:

    s3:*
    arn:aws:s3:::*

---

## S3 ACL rules

If the task writes to S3, ask whether uploaded objects need ACL.

Ask:

“Should uploaded S3 objects use an ACL?”

Options:

1. No ACL — recommended default.
2. `private`
3. `public-read`

Recommendation:

Use no ACL by default.

Reason:

Many modern S3 buckets use Object Ownership settings that disable ACLs. Public access is often better handled with CloudFront or signed URLs.

If the user chooses `public-read`, warn:

`public-read` works only if the bucket allows ACLs and Block Public Access does not block public ACLs. Prefer CloudFront or signed URLs when possible.

If ACL is required, add the necessary S3 permission only when needed, for example:

    s3:PutObjectAcl

Do not add ACL permissions unless the task actually needs ACL.

---

## Dependencies

Use minimum dependencies.

Always include:

    @aws-sdk/client-lambda

Include only if S3 is needed:

    @aws-sdk/client-s3

Use built-in `fetch` in Node.js 22 for webhook calls.

Do not add `axios` by default.

Do not add validation libraries by default.

Do not rely on AWS SDK being preinstalled in the Lambda runtime. Declare required AWS SDK v3 clients explicitly in `package.json`.

---

## package.json rules

Create a minimal `package.json`.

Rules:

- Do not set `"type": "module"`.
- Keep CommonJS.
- Include deploy/remove scripts.
- Include `serverless` as dev dependency.
- Include `serverless-iam-roles-per-function` as dev dependency.
- Include AWS SDK clients based on need.

Example without S3:

    {
      "name": "pdf-security-scanner",
      "version": "1.0.0",
      "private": true,
      "scripts": {
        "deploy": "serverless deploy",
        "remove": "serverless remove"
      },
      "dependencies": {
        "@aws-sdk/client-lambda": "^3.0.0"
      },
      "devDependencies": {
        "serverless": "^4.0.0",
        "serverless-iam-roles-per-function": "^3.2.0"
      }
    }

Example with S3:

    {
      "name": "pdf-security-scanner",
      "version": "1.0.0",
      "private": true,
      "scripts": {
        "deploy": "serverless deploy",
        "remove": "serverless remove"
      },
      "dependencies": {
        "@aws-sdk/client-lambda": "^3.0.0",
        "@aws-sdk/client-s3": "^3.0.0"
      },
      "devDependencies": {
        "serverless": "^4.0.0",
        "serverless-iam-roles-per-function": "^3.2.0"
      }
    }

---

## .env.example rules

Always create `.env.example`.

Keep it minimal.

Without S3:

    AUTH_TOKEN=replace-with-secret-token

With S3:

    AUTH_TOKEN=replace-with-secret-token
    S3_BUCKET_NAME=your-single-tenant-bucket

Do not add `CALLBACK_TIMEOUT_MS` by default.

Do not add `AWS_REGION` because region is hardcoded to `eu-west-1`.

Do not add real secrets.

---

## Timeout defaults

Use these defaults:

Dispatcher:

    timeout: 10

Worker:

    timeout: 300

AWS Lambda maximum is 900 seconds.

Only increase Worker timeout if the user clearly needs a longer task.

---

## Memory defaults

Use these defaults:

Dispatcher:

    memorySize: 128

Worker:

    memorySize: 512

Only increase Worker memory if the task is heavy, such as large file processing, PDF parsing, image processing, or index generation.

---

## Serverless Framework template rules

`serverless.yml` must include:

- `service`
- `frameworkVersion`
- `plugins` with `serverless-iam-roles-per-function`
- provider name
- runtime
- region
- stage
- environment
- per-function `iamRoleStatements` (Dispatcher: `lambda:InvokeFunction` only; Worker: task-specific only)
- Dispatcher function
- Worker function
- API Gateway `httpApi`
- Worker async retry config
- function-specific timeout and memory

Use `WORKER_FUNCTION_NAME` env var for the Dispatcher.

Use `AUTH_TOKEN: ${env:AUTH_TOKEN}`.

Use `S3_BUCKET_NAME: ${env:S3_BUCKET_NAME}` only if S3 is needed.

---

## Example serverless.yml skeleton

Adapt names to the specific task.

    service: example-task-service

    frameworkVersion: "4"

    plugins:
      - serverless-iam-roles-per-function

    provider:
      name: aws
      runtime: nodejs22.x
      region: eu-west-1
      stage: ${opt:stage, 'dev'}
      environment:
        AUTH_TOKEN: ${env:AUTH_TOKEN}
        STAGE: ${sls:stage}

    functions:
      exampleTaskDispatcher:
        handler: src/dispatcher.handler
        timeout: 10
        memorySize: 128
        environment:
          WORKER_FUNCTION_NAME: ${self:service}-${sls:stage}-exampleTaskWorker
        iamRoleStatements:
          - Effect: Allow
            Action:
              - lambda:InvokeFunction
            Resource:
              - arn:aws:lambda:${aws:region}:${aws:accountId}:function:${self:service}-${sls:stage}-exampleTaskWorker
        events:
          - httpApi:
              path: /run
              method: post

      exampleTaskWorker:
        handler: src/worker.handler
        timeout: 300
        memorySize: 512
        maximumRetryAttempts: 0
        iamRoleStatements: []

When S3 is needed, add `S3_BUCKET_NAME` to provider env:

    provider:
      environment:
        AUTH_TOKEN: ${env:AUTH_TOKEN}
        STAGE: ${sls:stage}
        S3_BUCKET_NAME: ${env:S3_BUCKET_NAME}

And replace the Worker's empty `iamRoleStatements` with the relevant S3 statements (see "S3 IAM permissions"). The Dispatcher's `iamRoleStatements` does not change.

---

## dispatcher.js template

Generate task-specific validation inside `validateTaskPayload`.

    const { LambdaClient, InvokeCommand } = require("@aws-sdk/client-lambda");
    const crypto = require("crypto");

    const lambdaClient = new LambdaClient({});

    module.exports.handler = async (event) => {
      const stage = process.env.STAGE || "dev";

      const authResult = validateAuth(event);
      if (!authResult.ok) {
        console.warn("Auth rejected");
        return jsonResponse(401, {
          ok: false,
          error: "Unauthorized"
        });
      }

      let body;

      try {
        body = parseJsonBody(event);
      } catch (error) {
        return jsonResponse(400, {
          ok: false,
          error: "Invalid JSON body"
        });
      }

      const wrapperValidation = validateWrapperPayload(body);
      if (!wrapperValidation.ok) {
        return jsonResponse(400, {
          ok: false,
          error: wrapperValidation.error
        });
      }

      const taskValidation = validateTaskPayload(body.task_payload);
      if (!taskValidation.ok) {
        return jsonResponse(400, {
          ok: false,
          error: taskValidation.error
        });
      }

      const jobId = createJobId();

      const workerPayload = {
        job_id: jobId,
        task_payload: body.task_payload
      };

      if (body.callback_url) {
        workerPayload.callback_url = body.callback_url;
      }

      if (body.callback_token) {
        workerPayload.callback_token = body.callback_token;
      }

      try {
        await lambdaClient.send(new InvokeCommand({
          FunctionName: process.env.WORKER_FUNCTION_NAME,
          InvocationType: "Event",
          Payload: Buffer.from(JSON.stringify(workerPayload))
        }));

        console.log(`[${jobId}] Worker invoked successfully`);

        return jsonResponse(202, {
          ok: true,
          status: "accepted",
          job_id: jobId
        });
      } catch (error) {
        console.error(`[${jobId}] Failed to start worker`, error);

        const responseBody = {
          ok: false,
          error: "Failed to start worker"
        };

        if (stage !== "production") {
          responseBody.details = error.message;
        }

        return jsonResponse(500, responseBody);
      }
    };

    function parseJsonBody(event) {
      if (!event.body || typeof event.body !== "string") {
        throw new Error("Missing body");
      }

      return JSON.parse(event.body);
    }

    function validateAuth(event) {
      // API Gateway HTTP API v2 lowercases all header keys.
      const incomingToken = (event.headers || {})["x-api-token"];

      if (!incomingToken || !process.env.AUTH_TOKEN) {
        return { ok: false };
      }

      // Constant-time comparison to prevent timing attacks on the token.
      // Note: the length pre-check leaks token length, which is acceptable
      // because timingSafeEqual requires equal-length inputs.
      const incoming = Buffer.from(incomingToken);
      const expected = Buffer.from(process.env.AUTH_TOKEN);
      if (
        incoming.length !== expected.length ||
        !crypto.timingSafeEqual(incoming, expected)
      ) {
        return { ok: false };
      }

      return { ok: true };
    }

    function validateWrapperPayload(body) {
      if (!isPlainObject(body)) {
        return { ok: false, error: "Body must be a JSON object" };
      }

      if (!isPlainObject(body.task_payload)) {
        return { ok: false, error: "task_payload must be a JSON object" };
      }

      if (body.callback_url !== undefined) {
        if (typeof body.callback_url !== "string" || !isValidHttpsUrl(body.callback_url)) {
          return { ok: false, error: "callback_url must be a valid https URL" };
        }
      }

      if (body.callback_token !== undefined) {
        if (!body.callback_url) {
          return { ok: false, error: "callback_token requires callback_url" };
        }

        if (
          typeof body.callback_token !== "string" ||
          body.callback_token.trim().length === 0 ||
          body.callback_token.length > 500
        ) {
          return {
            ok: false,
            error: "callback_token must be a non-empty string up to 500 characters"
          };
        }
      }

      return { ok: true };
    }

    function validateTaskPayload(taskPayload) {
      // TODO: Replace this with task-specific validation.
      // Example:
      // if (!taskPayload.s3_key || typeof taskPayload.s3_key !== "string") {
      //   return { ok: false, error: "task_payload.s3_key is required" };
      // }

      return { ok: true };
    }

    function isPlainObject(value) {
      return (
        value !== null &&
        typeof value === "object" &&
        !Array.isArray(value)
      );
    }

    function isValidHttpsUrl(value) {
      try {
        const url = new URL(value);
        return url.protocol === "https:";
      } catch {
        return false;
      }
    }

    function createJobId() {
      const now = new Date();
      const datePart = now.toISOString().slice(0, 10).replace(/-/g, "");
      const randomPart = Math.random().toString(16).slice(2, 8);
      return `job_${datePart}_${randomPart}`;
    }

    function jsonResponse(statusCode, body) {
      return {
        statusCode,
        headers: {
          "Content-Type": "application/json"
        },
        body: JSON.stringify(body)
      };
    }

---

## worker.js template

    const CALLBACK_TIMEOUT_MS = 5000;

    module.exports.handler = async (event) => {
      const jobId = event.job_id || "unknown_job";

      console.log(`[${jobId}] Worker started`);

      try {
        const taskPayload = event.task_payload;

        if (!taskPayload || typeof taskPayload !== "object" || Array.isArray(taskPayload)) {
          throw new Error("Invalid task_payload");
        }

        const result = await runTask(taskPayload);

        console.log(`[${jobId}] Task completed`);

        if (event.callback_url) {
          await sendCallback({
            jobId,
            callbackUrl: event.callback_url,
            callbackToken: event.callback_token,
            payload: {
              ok: true,
              job_id: jobId,
              status: "completed",
              result
            }
          });
        }

        // Return value is discarded by AWS for async invocations; useful for local testing only.
        return {
          ok: true,
          job_id: jobId,
          status: "completed",
          result
        };
      } catch (error) {
        console.error(`[${jobId}] Task failed`, error);

        if (event.callback_url) {
          await sendCallback({
            jobId,
            callbackUrl: event.callback_url,
            callbackToken: event.callback_token,
            payload: {
              ok: false,
              job_id: jobId,
              status: "failed",
              error: error.message || "Task failed"
            }
          });
        }

        // Return value is discarded by AWS for async invocations; useful for local testing only.
        return {
          ok: false,
          job_id: jobId,
          status: "failed",
          error: error.message || "Task failed"
        };
      }
    };

    async function runTask(taskPayload) {
      // TODO: Implement the single task here.
      // This function must return a JSON-serializable object.
      // If the task fails, throw an Error with a human-readable message.

      return {
        success: true,
        summary: "Task completed"
      };
    }

    async function sendCallback({ jobId, callbackUrl, callbackToken, payload }) {
      const controller = new AbortController();
      const timeout = setTimeout(() => controller.abort(), CALLBACK_TIMEOUT_MS);

      try {
        const headers = {
          "Content-Type": "application/json"
        };

        if (callbackToken) {
          headers["x-callback-token"] = callbackToken;
        }

        const response = await fetch(callbackUrl, {
          method: "POST",
          headers,
          body: JSON.stringify(payload),
          signal: controller.signal
        });

        if (!response.ok) {
          console.error(`[${jobId}] Callback failed with status ${response.status}`);
        }
      } catch (error) {
        console.error(`[${jobId}] Callback request failed`, error);
      } finally {
        clearTimeout(timeout);
      }
    }

---

## README requirements

Always create a short, practical `README.md`.

It must include:

1. What the Lambda project does.
2. Architecture summary.
3. Files created.
4. Environment variables.
5. Installation command.
6. Deploy command for `dev`.
7. Deploy command for `production`.
8. How to find the endpoint after deploy.
9. Example `curl`.
10. Example request payload.
11. Explanation of optional callback.
12. S3 permissions, if relevant.
13. Security notes.

---

## README example structure

    # Example Task Service

    This project creates a simple async Lambda worker flow:

    API Gateway `POST /run`
    → Dispatcher Lambda
    → immediate `202 Accepted`
    → async Worker Lambda
    → optional webhook callback.

    ## Requirements

    - Node.js
    - Serverless Framework v4
    - AWS credentials configured locally or in CI

    ## Environment variables

    Create your local env:

        export AUTH_TOKEN="your-secret-token"

    If S3 is used:

        export S3_BUCKET_NAME="your-single-tenant-bucket"

    ## Install

        npm install

    ## Deploy to dev

        npx serverless deploy --stage dev

    ## Deploy to production

        npx serverless deploy --stage production

    ## Endpoint

    The API Gateway endpoint is printed by Serverless after deploy.

    Use the printed `POST /run` endpoint.

    ## Example request

        curl -X POST "https://xxxx.execute-api.eu-west-1.amazonaws.com/run" \
          -H "Content-Type: application/json" \
          -H "x-api-token: your-secret-token" \
          -d '{
            "task_payload": {
              "example": "value"
            },
            "callback_url": "https://example.com/webhook",
            "callback_token": "optional-callback-token"
          }'

    ## Successful response

        {
          "ok": true,
          "status": "accepted",
          "job_id": "job_20260509_8f3a2c"
        }

    ## Callback success payload

        {
          "ok": true,
          "job_id": "job_20260509_8f3a2c",
          "status": "completed",
          "result": {}
        }

    ## Callback failure payload

        {
          "ok": false,
          "job_id": "job_20260509_8f3a2c",
          "status": "failed",
          "error": "Human readable error message"
        }

---

## Testing rules

Do not add a test framework by default.

No Jest.  
No Vitest.  
No complex test setup.

The README must include a complete `curl` example for testing the deployed API Gateway endpoint.

---

## Questions Claude Code should ask

Ask only when required for correctness.

Ask one question at a time.

### If the worker task is unclear

Ask:

“What is the single task this Worker Lambda should perform?”

Reason:

The entire pattern depends on one Lambda doing one clearly defined job.

### If S3 need is unclear

Ask:

“Does this Lambda need S3 access?”

Options:

1. No S3 access — recommended default.
2. Read only — `s3:GetObject`.
3. Write only — `s3:PutObject`.
4. Read and write — `s3:GetObject` + `s3:PutObject`.

### If S3 write is needed and ACL is unclear

Ask:

“Should uploaded S3 objects use an ACL?”

Options:

1. No ACL — recommended default.
2. `private`
3. `public-read`

If `public-read` is selected, warn that it requires bucket ACL support and compatible Block Public Access settings.

---

## Final Claude Code output

After creating or updating the project, Claude Code must end with a short practical summary:

- Files created.
- Service name.
- Default stage.
- Production stage.
- How to deploy to dev.
- How to deploy to production.
- Reminder that endpoint comes from Serverless deploy output.
- Reminder to set `AUTH_TOKEN` before deploy.
- Mention S3 permissions if added.

Example:

    Created the async Lambda worker project.

    Files:
    - serverless.yml
    - package.json
    - .env.example
    - README.md
    - src/dispatcher.js
    - src/worker.js

    Service:
    pdf-security-scanner

    Default stage:
    dev

    Production stage:
    production

    Deploy dev:
    export AUTH_TOKEN="..."
    npx serverless deploy --stage dev

    Deploy production:
    export AUTH_TOKEN="..."
    npx serverless deploy --stage production

    Use the API Gateway endpoint printed by Serverless after deploy.

---

## What not to do

Do not:

- Run the long task inside the Dispatcher.
- Wait for Worker completion before returning HTTP response.
- Use REST API unless explicitly requested.
- Use a region other than `eu-west-1`.
- Use TypeScript by default.
- Use ESM by default.
- Add unnecessary dependencies.
- Add Axios by default.
- Add Zod/Joi by default.
- Add test frameworks by default.
- Add S3 permissions unless needed.
- Give broad IAM permissions.
- Use `lambda:*`.
- Use `s3:*`.
- Give the Dispatcher task-specific permissions (S3, DynamoDB, etc.).
- Share one IAM role between the Dispatcher and Worker.
- Let the caller choose arbitrary S3 buckets.
- Log secrets.
- Log auth tokens.
- Log callback tokens.
- Log full headers.
- Commit real secrets.
- Hardcode final API Gateway endpoint.
- Reuse `AUTH_TOKEN` as callback token.
- Enable CORS by default.
- Use `prod` as the production stage name.

---

## Core principle

Keep the Lambda boring, small, secure, and predictable.

One API Gateway endpoint.  
One Dispatcher Lambda.  
One Worker Lambda.  
One task.  
One tenant.  
One region: `eu-west-1`.  
One deployment framework: Serverless Framework.  
One language: JavaScript.  
One simple async pattern.
