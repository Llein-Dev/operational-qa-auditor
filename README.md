# 🛡️ Operational QA Auditor (Open-Source Skill for AI Agents)

> **Universal Operational Quality Assurance & Deep Codebase Auditing Workflow for AI Coding Assistants & Engineering Teams.**

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](https://opensource.org/licenses/MIT)
[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg)](CONTRIBUTING.md)
[![Compatible with Antigravity](https://img.shields.io/badge/Compatible-Google%20Antigravity-orange.svg)](#)
[![Compatible with Cursor & Claude](https://img.shields.io/badge/Compatible-Cursor%20%7C%20Claude%20%7C%20Copilot-purple.svg)](#)

---

## 💡 The Problem This Skill Solves

When AI coding agents (Claude, Cursor, Copilot, Antigravity) are asked to test or review a software project, they frequently make critical mistakes:
1. **Shallow Testing:** They check if the page loads (HTTP 200) and immediately declare "All features working perfectly!", ignoring subtle database integrity issues, broken foreign keys, and empty state crashes.
2. **Accepting Fake / Hardcoded Data:** When telemetry or metrics are hardcoded (e.g. `uptime 99.98%`, `+12% traffic` on an empty database), standard agents don't notice or call it out.
3. **Endangering Production Environments:** Testing directly on live databases or triggering external actuators without sandbox isolation.
4. **Ignoring Cascading Transaction Failures:** Missing bugs where Step 1 creates a record but Step 2 fails, leaving orphaned corrupted data without rollback.

**Operational QA Auditor** is an open-source, prompt-engineered skill and operational cheatsheet that teaches AI agents to think, inspect, debug, and report like **Principal QA Engineers and Reliability Architects**.

---

## 🧭 The 4 Pillars of the Auditor Mindset

```
┌────────────────────────────────────────────────────────────────────────┐
│                   OPERATIONAL QA AUDITOR MINDSET                       │
├────────────────────┬────────────────────┬──────────────────────────────┤
│ 1. Zero Production │ 2. Two-Dimensional │ 3. Deep Root-Cause           │
│    Impact          │    Audit Matrix    │    Tracing                   │
│                    │                    │                              │
│ • Isolated DB only │ • Horizontal:      │ • Beyond HTTP 500            │
│ • Stubs for IoT/   │   24-point route   │ • Trace SQL constraints & FK │
│   Hardware/Webhooks│   & UI/UX audit    │ • Trace Go/JSON serialization│
│ • No side-effect   │ • Vertical:        │ • Catch unhandled React nulls│
│   bleed-through    │   Real Persona E2E │ • Radical Data Honesty (No   │
│                    │   workflows        │   fake hardcoded metrics)    │
└────────────────────┴────────────────────┴──────────────────────────────┘
```

---

## 🚀 Quickstart & Installation

### Option 1: Using with Google Antigravity
Clone directly into your project's `.agents/skills/` directory:
```bash
git clone https://github.com/llein-dev/operational-qa-auditor.git .agents/skills/operational-qa-auditor
```
In your Antigravity chat, simply ask:
> *"Act as an operational-qa-auditor and audit the authentication and payment flows."*

### Option 2: Using with Cursor IDE
Create `.cursorrules` or add to `.cursor/rules/`:
```markdown
# Include the instructions from SKILL.md in your Cursor rules file.
```

### Option 3: Using with Claude Code / Claude Desktop / Custom Agents
Attach or paste `SKILL.md` into your project's custom instructions or system prompt.

---

## 🎯 Community Contribution Goals (Roadmap)

We welcome the global developer and QA community to expand this universal skill! Here are our primary community objectives:

- [x] **Universal Enterprise Core (`SKILL.md`):** Complete two-dimensional horizontal/vertical audit framework.
- [ ] **Domain-Specific Audit Playbooks (`playbooks/`):**
  - [ ] **Fintech & Banking:** Double-entry ledger audit, idempotency keys, currency precision.
  - [ ] **E-Commerce & Logistics:** Cart abandonment edge-cases, warehouse inventory concurrency locks, discount stacking.
  - [ ] **Healthcare & HIPAA:** PII/PHI data leakage prevention, access audit trails, encryption at rest.
  - [ ] **IoT & Smart Site Systems:** Hardware simulator harnesses, reconnect resilience, barrier/gate fail-safe states.
- [ ] **Automated Test Runners & Scripts (`scripts/`):**
  - [ ] Script to scan repositories for hardcoded fake metrics.
  - [ ] Script to verify database isolation and preflight safety flags.

---

## 🤝 Contributing

Contributions are warmly welcomed! Please read [CONTRIBUTING.md](CONTRIBUTING.md) for details on code of conduct and the pull request submission process.

---

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.
