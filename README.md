# EdenCORP Engineering Standards

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)
[![Standards Version](https://img.shields.io/badge/standards-v1.0.0-green.svg)](VERSIONING.md)
[![Maintained](https://img.shields.io/badge/maintained-yes-brightgreen.svg)](CONTRIBUTING.md)

> The official engineering standards, principles, and conventions for all EdenCORP projects.

---

## Purpose

This repository defines the engineering culture, development practices, and operational standards for every team, product, and system at EdenCORP. It is the single source of truth for how we build, ship, and maintain software.

Standards defined here apply to all repositories under the `EdenCorporations` organization — whether AI systems, web platforms, infrastructure tooling, or automation pipelines.

---

## Engineering Philosophy

EdenCORP builds AI-native, enterprise-grade systems that are designed to scale. Our engineering principles reflect that ambition:

- **Clarity over cleverness.** Code is read far more than it is written. Optimize for human understanding.
- **Security by default.** Every system is designed with security as a first-class concern, not an afterthought.
- **Automation first.** Manual processes are risks. Automate testing, deployment, compliance checks, and reviews wherever possible.
- **AI-augmented, human-accountable.** We embrace AI tooling at every layer, but humans remain accountable for all decisions.
- **Scalability is designed in.** Systems must be architected to handle growth without rework.
- **Observability is non-negotiable.** If you cannot measure it, you cannot operate it.

---

## Standards Index

| Document | Description |
|---|---|
| [ARCHITECTURE.md](ARCHITECTURE.md) | Service design, API standards, microservice boundaries, AI modularity |
| [DEVELOPMENT.md](DEVELOPMENT.md) | Tech stack, linting, naming, environment variables, performance budgets |
| [AI_ENGINEERING.md](AI_ENGINEERING.md) | Model integration, prompt standards, guardrails, responsible AI |
| [INFRASTRUCTURE.md](INFRASTRUCTURE.md) | Cloud, IaC, environment separation, monitoring, SLAs |
| [TESTING.md](TESTING.md) | Coverage targets, test types, CI requirements |
| [BRANCHING_STRATEGY.md](BRANCHING_STRATEGY.md) | Branch naming, PR flow, release and hotfix process |
| [COMMIT_CONVENTIONS.md](COMMIT_CONVENTIONS.md) | Conventional Commits, semantic versioning, breaking changes |
| [REVIEW_PROCESS.md](REVIEW_PROCESS.md) | PR checklists, approval requirements, AI-specific review |
| [INCIDENT_RESPONSE.md](INCIDENT_RESPONSE.md) | Severity levels, response SLAs, postmortem process |
| [SECURITY.md](SECURITY.md) | Vulnerability reporting, OWASP baseline, access control |
| [DOCUMENTATION.md](DOCUMENTATION.md) | Documentation requirements, ADR policy, Markdown standards |
| [VERSIONING.md](VERSIONING.md) | Semantic versioning, release tagging, deprecation policy |
| [CONTRIBUTING.md](CONTRIBUTING.md) | How to contribute to EdenCORP repositories |
| [CODE_OF_CONDUCT.md](CODE_OF_CONDUCT.md) | Expected behaviour for all contributors |

---

## How Teams Adopt These Standards

### New Projects

1. **Bootstrap from the standard template.** Every new repository must be initialized from the EdenCORP project template, which pre-configures linting, CI, branch protection, and required files.
2. **Reference this repository in your `README.md`.** Link back to `eden-standards` so contributors know where to find the rules.
3. **Declare your compliance level** in your repo's README using the standard compliance badge.

### Existing Projects

1. **Audit against the checklist** in [CONTRIBUTING.md](CONTRIBUTING.md).
2. **Open migration issues** for each area of non-compliance.
3. **Assign an owner** from [CODEOWNERS](CODEOWNERS) to each migration issue.
4. **Target compliance within one sprint** for P1/P2 gaps, one quarter for P3 gaps.

### Importing Standards into Coding Agents

Use the ready-made templates in [`templates/agent-configs/`](templates/agent-configs/) and copy them into your project at the correct config path for your coding agent.

All templates enforce the same startup policy:

1. The agent must load EdenCORP standards before beginning implementation.
2. If the standards repository is not available locally, the agent must stop and ask the user to clone `https://github.com/EdenCorporations/standards`.
3. The agent must read and apply at minimum: `README.md`, `DEVELOPMENT.md`, `TESTING.md`, `SECURITY.md`, and `REVIEW_PROCESS.md`.

#### Agent Config Paths and Templates

| Agent | Config location in your project | Template to copy |
|---|---|---|
| Cursor (legacy) | `.cursorrules` | [`templates/agent-configs/cursor/.cursorrules`](templates/agent-configs/cursor/.cursorrules) |
| Cursor (rules directory) | `.cursor/rules/*.mdc` | [`templates/agent-configs/cursor/.cursor/rules/eden-standards.mdc`](templates/agent-configs/cursor/.cursor/rules/eden-standards.mdc) |
| GitHub Copilot | `.github/copilot-instructions.md` | [`templates/agent-configs/copilot/.github/copilot-instructions.md`](templates/agent-configs/copilot/.github/copilot-instructions.md) |
| Windsurf | `.windsurfrules` | [`templates/agent-configs/windsurf/.windsurfrules`](templates/agent-configs/windsurf/.windsurfrules) |
| Claude Code | `CLAUDE.md` | [`templates/agent-configs/claude/CLAUDE.md`](templates/agent-configs/claude/CLAUDE.md) |
| Cline | `.clinerules` | [`templates/agent-configs/cline/.clinerules`](templates/agent-configs/cline/.clinerules) |
| Aider | `.aider.conf.yml` | [`templates/agent-configs/aider/.aider.conf.yml`](templates/agent-configs/aider/.aider.conf.yml) |
| Continue | `.continue/config.yaml` | [`templates/agent-configs/continue/.continue/config.yaml`](templates/agent-configs/continue/.continue/config.yaml) |

#### Setup Steps

1. Pick your agent from the table above.
2. Copy the matching template file to the target config location in your project repository.
3. If needed, update the standards path in the template (for example `../standards`).
4. Commit the config file so all contributors and CI agents use the same policy.
5. On first run, if the agent reports standards are missing, clone the repo and rerun:
   - `git clone https://github.com/EdenCorporations/standards`

### Enforcement

- Branch protection rules enforce PR requirements and required status checks.
- CI pipelines enforce linting, testing, and security scanning automatically.
- Quarterly standards reviews are conducted by `@EdenCorporations/platform`.
- Exceptions require written approval from `@EdenCorporations/leadership` and must be documented in an [ADR](templates/architecture_decision_record.md).

---

## Templates

| Template | Use Case |
|---|---|
| [pull_request_template.md](templates/pull_request_template.md) | Standard PR description |
| [issue_bug.md](templates/issue_bug.md) | Bug reports |
| [issue_feature.md](templates/issue_feature.md) | Feature requests |
| [architecture_decision_record.md](templates/architecture_decision_record.md) | Architectural decisions |
| [agent-configs/](templates/agent-configs/) | Starter templates for coding-agent configuration |

---

## Ownership

This repository is maintained by `@EdenCorporations/platform` under the authority of `@EdenCorporations/leadership`.

To propose a change to these standards, open a pull request and follow the process defined in [CONTRIBUTING.md](CONTRIBUTING.md).

---

## License

Copyright © 2026 EdenCORP. All rights reserved. Internal use only.
