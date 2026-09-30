# Contributing to Operational QA Auditor

Thank you for your interest in contributing to **Operational QA Auditor**! 

Our goal is to build the definitive, open-source operational quality assurance cheatsheet and mental framework for AI coding agents and engineering teams worldwide.

---

## 🎯 Contribution Areas & Community Goals

We invite contributions across the following core areas:

### 1. Adding Domain-Specific Runbooks (`references/` or `playbooks/`)
Different software industries have unique failure modes. You can contribute specialized audit playbooks for:
* **Fintech & Payments:** Handling transaction idempotency, floating-point rounding errors, webhook replay attacks.
* **Healthcare & Life Sciences:** PHI/PII data obfuscation, audit trail permanence, compliance logging.
* **Smart Sites & Industrial IoT:** Stream disconnect recovery, hardware command validation, watchdog/heartbeat checking.
* **SaaS & Multi-Tenancy:** Cross-tenant data isolation, quota enforcement, subscription tier boundary testing.

### 2. Enhancing Failure Mode Detection Patterns
If you find edge-cases that AI agents commonly overlook (e.g., subtle hydration mismatches, Radix UI empty string panics, database foreign key orphanings), submit a pattern describing:
* The symptom & why standard testing misses it.
* The inspection checklist step to catch it.
* The recommended remediation pattern.

### 3. Tooling & Helper Scripts (`scripts/`)
Help automate the pre-audit checks:
* Scripts to detect hardcoded demo indicators in frontend code.
* Environment safety checkers (ensuring non-production DB URLs before running tests).

---

## 📋 Pull Request Guidelines

1. **Keep it Universal & Concise:** Instructions must be actionable for LLMs without token bloat. Avoid generic programming advice; focus strictly on auditing, edge-cases, and failure modes.
2. **Use Clear Markdown Hierarchy:** Maintain clean GitHub-flavored markdown with consistent alerts, tables, and code snippets.
3. **Test with AI Agents:** Before submitting changes to `SKILL.md`, test it in a live conversation with an AI agent (Antigravity, Claude, Cursor) to ensure the agent understands and reliably follows the instructions.

---

## ⚖️ Code of Conduct

Be welcoming, respectful, and constructive. We are here to raise the bar for software reliability and AI engineering excellence.
