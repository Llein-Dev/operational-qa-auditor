---
name: operational-qa-auditor
description: >-
  Universal operational quality assurance and deep codebase audit workflow for any software project.
  Use this skill when auditing enterprise web applications, APIs, distributed systems, or IoT platforms;
  executing systematic page-by-page UI/UX inspections; validating end-to-end persona workflows;
  diagnosing root-cause database, concurrency, and serialization defects; and enforcing production-readiness standards.
---

# Universal Operational QA & Deep Codebase Auditor Skill

A production-grade, framework-agnostic Quality Assurance & Deep Codebase Auditing specification designed for AI agents and engineering teams. This skill teaches agents to think and audit like seasoned Principal QA & Reliability Engineers across any software project (SaaS, FinTech, E-Commerce, Smart Site, IoT, Enterprise Portals).

---

## 🧭 I. CORE PHILOSOPHY & MENTAL MODELS

### 1. Zero Production Risk & Complete Isolation
* **Never execute tests or mutations against Production Databases:** Always require an isolated, dedicated test database instance or sandbox schema.
* **Never trigger unmocked external side-effects:** Hardware actuators, external billing webhooks, payment gateways, and third-party SMS/Email dispatchers must be replaced with test stubs, simulators, or dry-run modes.
* **Awareness of Implicit Side-Effects:** Starting a backend server or test runner can trigger migrations, automated seeders, or cache flushes. Always verify environment variables and target connection strings before execution.

### 2. Two-Dimensional Quality Assurance (The Audit Matrix)
* **Horizontal Dimension (Page-by-Page & Endpoint-by-Endpoint Audit):**
  Systematically inspect every route, view, and API endpoint for layout integrity, responsive behavior, form validation edge-cases, error/loading/empty states, and runtime console errors.
* **Vertical Dimension (Persona-Based End-to-End Operational Workflows):**
  Walk through complete lifecycles from the perspective of real personas (e.g., Operator, End-User, Auditor, Administrator). Test full journeys: authentication -> entity onboarding -> state transitions -> payment/processing -> reporting & reconciliation.

### 3. Deep Root-Cause Tracing (Beyond Surface Symptoms)
* When encountering an HTTP 500 error: Never stop at reporting "API failed". Inspect server logs, database query plans, broken foreign keys, transaction rollbacks, or missing migrations.
* When encountering serialization defects: Trace data contracts end-to-end (e.g., JSON unmarshaling, base64 byte encoding, date-time format mismatches).
* When encountering UI crashes: Inspect frontend browser console warnings (e.g., missing dependencies, unhandled empty values, hydration mismatches).

### 4. Radical Data Honesty & Objective Classification
* Never accept hardcoded dummy metrics or fake indicators masquerading as real telemetry. If a metric has no data, demand an explicit "No Data Available" state.
* Strictly classify all findings into standard severity levels:
  * **Blocker:** Halts core operations, deployment, or creates an unrecoverable system crash.
  * **Critical:** Corrupts financial transactions, data integrity, security boundaries, or permissions.
  * **Major:** Breaks primary operational workflow; workaround exists but is unacceptable for release.
  * **Minor:** Visual defect, UX inconsistency, or non-blocking usability issue.
  * **Business Observation:** Undocumented capability or business rule ambiguity requiring Product Owner clarification before declaring a defect.

---

## 🛠️ II. STEP-BY-STEP AUDITING RUNBOOK

### Phase 1: Environment Isolation & Preflight Verification
1. Verify backend, frontend, database, and background workers are isolated.
2. Confirm test configuration (e.g., `APP_ENV=test`, sandbox API keys, mock endpoints).
3. Prepare controlled test datasets (clean entities, valid records, invalid edge-case records).
4. Snapshot or establish a baseline to ensure repeatable database resets.

### Phase 2: Horizontal Inspection (Page / View / Endpoint Audit)
Apply this 8-point checklist to every route or screen:
1. **Access Control & Navigation:** Direct URL access, navigation menu, breadcrumbs, and permission guards for unauthorized roles.
2. **User Cognitive Clarity:** Is the purpose of the screen, metric units, status indicators, and primary Call-To-Action immediately obvious?
3. **Data State Quad:** Test (1) Normal data, (2) Empty state with actionable guidance, (3) Loading state (skeletons/spinners), and (4) Error state with a retry mechanism.
4. **Form Validation & Boundary Testing:** Submit empty forms; test special characters, Unicode, leading/trailing whitespace, and date-time boundaries. Ensure server validation errors map back to user-friendly UI messages.
5. **Runtime Error Snooping:** Check browser console / server logs for uncaught exceptions, missing imports, unhandled null references, or component crashes.
6. **Data Mutation & Confirmation:** Test Create, Update, Status Toggle, and Delete. Verify that destructive actions display unambiguous confirmation dialogs.
7. **Audit Trail Recording:** Confirm that state-changing actions (POST, PUT, DELETE) generate immutable audit log records containing performer, timestamp, and target entity.
8. **Navigation & Session Continuity:** Ensure actions transition smoothly without requiring manual page reloads or losing application state.

### Phase 3: Vertical Inspection (End-to-End Operational Workflows)
1. **Onboarding & Configuration:** Setup master entities, rules, schedules, and pricing/quotas.
2. **Standard Transaction Flow:** Execute regular lifecycle actions and verify proper persistence across all related tables.
3. **Exception & Recovery Handling:**
   * Disconnected networks / timeout simulation.
   * Duplicate input / idempotency validation.
   * Partial failure recovery: Multi-step operations must be atomic (e.g., wrapped in a database transaction). If step B fails, step A must roll back.
4. **Closing, Reporting & Reconciliation:** Compare operational actions against financial summaries, history logs, and exported reports.

### Phase 4: Production-Readiness & Release Packaging Audit
1. **Standalone & Bundle Integrity:** Verify build artifacts contain all runtime dependencies, assets, and statically traced modules.
2. **Fail-Fast Configuration Guard:** The application must immediately terminate on startup with a descriptive error code if critical production variables (e.g., `DB_URL`, `JWT_SECRET`) are missing, rather than falling back to weak defaults.
3. **Zero Secret Leakage:** Ensure distribution packages (`dist/`, `release/`) never bundle developer `.env` files, private keys, or credentials.
4. **Automated Verification:** Execute the full test suite and production build:
   * Backend tests: `go test -v ./...` (or equivalent test runner).
   * Frontend production build: `npm run build` / `pnpm build`.

---

## 📝 III. DEFECT LOGGING SPECIFICATION

When logging findings, use this structured format:

```markdown
### [DEFECT-ID] Concise Defect Title
- **Target Area / Route:** `/api/v1/resource` | `UI /settings`
- **Severity:** Blocker | Critical | Major | Minor | Business Observation
- **Preconditions & Test Data:** Specific state, inputs, or role used.
- **Steps to Reproduce:** Minimal, reproducible sequence.
- **Expected Behavior:** What a reliable system should do.
- **Observed Behavior:** Actual outcome with raw console/server logs.
- **Technical Root Cause:** Exact code file, line number, SQL constraint, or logic gap.
- **Remediation Recommendation:** Proposed architectural or code-level fix.
- **Status:** Open | In Progress | Verified Pass ✅ | Blocked
```
