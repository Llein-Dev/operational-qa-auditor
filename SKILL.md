---
name: operational-qa-auditor
description: >-
  Evidence-driven operational quality assurance and codebase auditing for software projects.
  Use when planning or performing scoped UI, API, workflow, data-integrity, security-boundary,
  reliability, or release-readiness audits. Adapts checks to the repository, requirements,
  available tools, and explicitly verified environment; reports verified, failed, blocked,
  and unverified areas without inventing expected behavior.
---

# Operational QA Auditor

A practical, framework-agnostic method for auditing software. Use the parts that fit the task and system. The goal is a reproducible, evidence-based assessment—not maximum test count.

## 1. Operating principles

### Establish scope and evidence

Before testing, identify:
- The question to answer, requested depth, in-scope routes/components/endpoints/workflows, and deliverable.
- Applicable acceptance criteria, product requirements, API contracts, invariants, security expectations, and known limitations.
- The repository architecture, available test commands, required services, and tools actually available.

Treat requirements, documented contracts, and established invariants as the basis for expected behavior. Do not invent product rules. If expected behavior is unclear, record a **Business Observation** or ask a focused question; continue independent checks where possible.

Make coverage visible. List the relevant scope and mark each item **Pass**, **Fail**, **Blocked**, **Not tested**, or **Not applicable**, with a brief reason/evidence. Do not imply exhaustive coverage from a sample. Use a route/endpoint inventory or a count (such as 24) only when it was actually derived from the project and the audited scope.

### Protect systems and data

- Identify the actual target environment and data source before any operation that could write data, run migrations/seeds, clear caches, dispatch jobs, send messages, charge money, or control hardware.
- Never knowingly run mutations or tests against production. If the target is unknown or cannot be verified, do not perform potentially destructive or externally visible actions; report **Blocked** and state what must be confirmed.
- Prefer isolated test data, sandbox credentials, mocks, simulators, dry-run modes, and reversible operations. Verify they are active rather than assuming a variable or flag is supported.
- Read-only inspection can proceed when safe even if an integration environment is unavailable. Distinguish static analysis from behavior exercised at runtime.
- Do not expose secrets or unnecessary personal data in notes, logs, screenshots, or defect reports.

### Be precise about conclusions

- Separate observed facts, inferred causes, and recommendations.
- A symptom is not a root cause. Call a cause confirmed only when evidence supports it; otherwise label it a hypothesis and name the next check.
- A missing feature is a defect only when a requirement or contract says it should exist.
- Report limitations and confidence. “Not tested” and “Blocked” are not passes.

## 2. Choose a proportionate audit

Use the two dimensions below as a planning framework, not a requirement to test every possible item every time.

### Horizontal: pages, components, APIs, jobs

Build an inventory appropriate to the request. For each in-scope item, select applicable checks:

1. **Reachability and access:** expected navigation/direct access, authentication, role/tenant guards, and denied access where relevant.
2. **Purpose and usability:** labels, units, status meaning, primary action, keyboard/accessibility basics when in scope.
3. **Data states:** representative populated, empty, loading, and error/retry behavior where supported and observable.
4. **Input boundaries:** required/optional fields, invalid and boundary values, Unicode, whitespace, date/time and numeric precision, server-side validation, and safe error messages.
5. **Runtime and contract behavior:** browser/server errors, request/response shape, status codes, nullability, serialization, and compatibility with documented contracts.
6. **State changes:** create/update/delete/toggle only when supported, authorized, relevant, and safe in the isolated environment. Check confirmation and resulting persisted state where applicable.
7. **Auditability:** verify audit records only where required by policy, specification, or existing system design. Do not assume every mutation must create an immutable log.
8. **Navigation and session continuity:** redirects, refresh behavior, preserved state, and session expiry when relevant.

For APIs, adapt the checks to authentication, authorization, schema validation, idempotency, pagination, rate limits, concurrency, and error contracts as applicable. Do not force UI-only checks onto a backend endpoint.

### Select test techniques by risk

Choose a small set of techniques that can expose the risks in scope; these are options, not mandatory steps for every audit:

- **Equivalence partitioning and boundary-value analysis:** sample valid/invalid classes and values at limits (including just below/at/above a boundary).
- **Decision tables and pairwise testing:** cover combinations of roles, flags, inputs, and configuration when exhaustive combinations are impractical.
- **State-transition testing:** exercise allowed and forbidden transitions, including retries and terminal states.
- **Contract testing:** compare producers and consumers against documented API, event, or schema contracts.
- **Exploratory testing:** probe unexpected but plausible use, following observations while recording the path and evidence.
- **Concurrency and fault injection:** use only when relevant and safely isolated to examine races, duplicate delivery, timeouts, and partial failures.

State why selected cases represent the important risk. A small risk-based sample is valid when disclosed; it is not proof of exhaustive correctness.

### Recognize architecture and adapt the audit

Identify the architecture from repository evidence before choosing architecture-specific checks. Architecture names are clues, not quality judgments. Focus on boundaries, ownership, failure modes, and observable guarantees:

- **Monolith or modular monolith:** module boundaries, shared database assumptions, transaction scope, and unintended coupling.
- **Microservices or service-oriented systems:** service contracts, data ownership, timeouts, retries, idempotency, version compatibility, and partial availability.
- **Event-driven systems:** duplicate/out-of-order delivery, consumer idempotency, poison messages, dead-letter/replay behavior, and eventual consistency.
- **Serverless or background jobs:** retry semantics, duplicate invocation, execution limits, cold-start/configuration behavior, and partial completion.
- **External, hardware, or payment integrations:** sandbox/mock confirmation, command/result correlation, timeout and reconnect behavior, and prevention of real side effects.

Apply only checks supported by the actual design and task. Do not demand a specific architecture or implementation pattern without a requirement.

For distributed data flows, consider whether outbox/inbox patterns, queues, caches, or asynchronous projections exist. Verify the documented consistency window and recovery behavior; do not assume immediate consistency or require a particular pattern by default.

### Vertical: persona and lifecycle workflows

Select end-to-end journeys relevant to the task. Define persona, preconditions, inputs, actions, expected outcome, and the data/state that should persist. Include important exception paths (timeouts, duplicates, denied access, partial failure, retries) when risk or requirements warrant them.

For multi-step operations, verify the consistency guarantee the system promises. This may be an ACID transaction, saga, idempotent retry, or compensating action. Do not prescribe rollback as universal architecture; check that partial failure is handled, visible, recoverable, and consistent with the contract.

## 3. Runbook

### Phase A: Reconnaissance and preflight

1. Inspect project instructions, architecture, routes/endpoints, schemas, tests, configuration templates, and relevant recent changes.
2. Identify dependencies and side effects of candidate commands (startup hooks, migrations, seeders, workers, outbound calls).
3. Verify target environment and isolation using project-supported configuration. Never infer safety from example variable names.
4. Record baseline state and prepare controlled test data only if authorized and safe.
5. If a prerequisite is missing, continue safe static/read-only work and mark dependent checks **Blocked**.

### Phase B: Execute the scoped matrix

1. Cover each in-scope item with the applicable horizontal checks.
2. Run selected persona workflows through their meaningful state transitions.
3. Capture minimal reproducible evidence: steps, sanitized inputs, response/log excerpts, affected records, and environment.
4. After a mutation, verify the intended state and relevant related records. Clean up test data only when cleanup is safe and unambiguous.
5. Stop a check if it risks production data, real financial activity, external communication, or physical equipment.

### Phase C: Trace defects

Follow the evidence across layers as appropriate:
- UI event/state → network request → API handler → service/domain logic → persistence or external integration.
- HTTP/server error → sanitized logs → query/schema/constraints → migration and transaction behavior.
- Wrong/missing value → source data → domain conversion → serialization contract → client rendering.
- Concurrent or repeated operation → locking/idempotency/retry behavior → final persisted state.

Use logs and database inspection only when available and authorized. Avoid running expensive or invasive diagnostics without need. Distinguish verified root cause from likely cause.

### Phase D: Release readiness (only when requested or in scope)

Assess relevant items rather than applying this phase to every audit:
- Reproducible tests and production build for the target platform.
- Required configuration validation appropriate to each environment; fail clearly for missing production-critical settings without unsafe fallback defaults.
- Package contents and runtime dependencies.
- Secret and sensitive-data exclusion from distributables and logs.
- Migration/rollback or recovery plan, deployment assumptions, and known operational dependencies.

Run tests/builds only when requested or necessary to answer the scoped question, and after checking their side effects and environment. Use the repository's documented commands; examples such as `go test ./...` or `npm run build` are not universal. Report the exact commands and outcomes. Never claim tests passed if they were not run.

## 4. Finding severity

Use the project's severity scheme if one exists; otherwise:

- **Blocker:** prevents a critical operation or safe release with no viable workaround.
- **Critical:** serious security boundary, data integrity, or financial correctness failure.
- **Major:** important intended workflow fails; workaround is materially inadequate.
- **Minor:** limited usability, visual, or non-critical behavior issue.
- **Business Observation:** expected behavior or business rule is unclear or undocumented.

Severity describes impact, not certainty. State uncertainty separately. Use **Blocked** for an unperformed check due to environment, permission, dependency, or safety constraint; use **Not tested** when it was simply outside the executed scope.

## 5. Defect report

```markdown
### [ID] Concise title
- **Area:** route, endpoint, component, job, or subsystem
- **Severity / status:** Blocker|Critical|Major|Minor|Business Observation; Open|Verified|Blocked
- **Environment:** sanitized environment and relevant version/build
- **Evidence basis:** requirement, contract, invariant, or reason expected behavior is established
- **Preconditions and data:** role, state, and sanitized inputs
- **Steps to reproduce:** minimal numbered steps
- **Expected:** behavior supported by the evidence basis
- **Observed:** actual behavior, response, and relevant sanitized logs
- **Impact:** users, data, operations, and scope affected
- **Cause:** confirmed cause or explicitly labeled hypothesis
- **Recommendation:** proportionate next action
- **Verification:** checks performed and remaining limitations
```

## 6. Audit summary

Conclude with:
- Scope inspected and coverage counts or inventory reference.
- Findings by severity, including business observations.
- Commands/actions run and their outcomes, if any.
- Blocked and not-tested items with reasons.
- Overall assessment tied to the requested decision, with confidence and material limitations.

Never summarize a partial or static-only review as a complete operational pass.
