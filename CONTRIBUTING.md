# Contributing

Thanks for helping improve Operational QA Auditor. Contributions should make audits more useful, safe, reproducible, and honest about evidence and coverage.

## Good contributions

- Domain playbooks in `playbooks/` (for example payments, healthcare, e-commerce, multi-tenancy, or IoT), with explicit assumptions and safety boundaries.
- Failure-mode patterns that explain symptom, evidence to collect, applicable checks, and likely remediation.
- Small helper tools for repository inventory, test-environment preflight, or detection of misleading hardcoded metrics.
- Clarifications that reduce overclaiming or distinguish universal principles from project-specific policy.

Domain rules must be presented as conditional guidance, not requirements for unrelated systems. Avoid encoding a compliance claim as a pass/fail rule without a defined authoritative standard and scope.

## Pull request guidance

1. Keep instructions concise, actionable, and evidence-based.
2. State which projects or situations a new check applies to and what prerequisites it needs.
3. Do not assume universal routes, environment variables, database engines, frameworks, audit-log designs, or transaction architecture.
4. For scripts, document side effects and default to read-only or dry-run behavior. Never target production by default.
5. Include examples that use synthetic data and do not expose credentials or personal data.
6. Review Markdown rendering and links. If behavior changes, explain how it was checked; do not claim cross-agent compatibility without trying the supported setup.

## Code of conduct

Be respectful and constructive. Focus discussion on improving reliability and clarity for users and contributors.
