# Operational QA Auditor

An evidence-driven, framework-agnostic workflow for auditing user interfaces, APIs, operational journeys, data integrity, and release readiness. It helps AI agents and engineering teams produce reproducible findings without inventing expected behavior or overstating coverage.

## What it covers

- **Scoped horizontal checks:** inspect the relevant pages, endpoints, components, or jobs using only applicable checks.
- **Persona workflows:** follow meaningful user journeys and exception paths through their state changes.
- **Root-cause tracing:** connect UI, API, domain logic, serialization, persistence, and integrations when evidence calls for it.
- **Safe, honest reporting:** verify environment before risky actions; distinguish Pass, Fail, Blocked, Not tested, and Business Observation.
- **Release readiness:** optional phase for requests that include build, packaging, configuration, or deployment assessment.
- **Risk-selected techniques:** boundary and state-transition checks, contract testing, pairwise sampling, and safe fault/concurrency probes where relevant.
- **Architecture-aware inspection:** adapt checks for monoliths, services, event-driven flows, jobs, caches, and external integrations based on repository evidence.

The full instructions are in [`SKILL.md`](SKILL.md). Industry-specific playbooks can extend the core without making every domain check mandatory for every project.

Keep architecture-specific or regulated-domain detail in focused playbooks under `playbooks/`; the core skill treats patterns as conditional prompts, not required designs.

## Install

Clone this repository into the agent's supported skills directory. For Antigravity, for example:

```bash
git clone https://github.com/llein-dev/operational-qa-auditor.git .agents/skills/operational-qa-auditor
```

For other tools, follow their current custom-instructions or skills documentation and include `SKILL.md`. The Cursor and Claude examples below are minimal manual approaches; support and discovery behavior can differ by product/version.

### Cursor

Add the contents or a reference to `SKILL.md` in a project rule under `.cursor/rules/`, using the rule format supported by your installed Cursor version.

### Claude Code or custom agents

Add `SKILL.md` to the project's supported instructions/skills location or provide it as context. Verify the tool actually loads the file before relying on it.

## Use

Ask for a scoped audit, for example:

> Audit the password reset workflow against the repository's documented requirements. Use only the local test environment, and report what you verified, what failed, and what was blocked.

The auditor should discover the project's actual routes, commands, environment configuration, and contracts. It must not assume that example commands, variables, audit logs, CRUD operations, or industry rules apply universally.

## Contributing

See [`CONTRIBUTING.md`](CONTRIBUTING.md) for contribution areas and review guidance.

## License

MIT. See [`LICENSE`](LICENSE).
