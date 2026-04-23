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

To use these standards in any coding agent (Copilot, Cursor, Claude Code, Cline, Continue, Windsurf, and similar tools), apply one of these patterns:

1. **Direct URL import (works in most chat-based agents).**
   - Add the repo URL as context: `https://github.com/EdenCorporations/standards`
   - If your agent supports document-level context, also attach the specific files it should enforce (for example `DEVELOPMENT.md`, `TESTING.md`, `SECURITY.md`, `REVIEW_PROCESS.md`).

2. **Repository instruction file (best for repeatable enforcement).**
   - In your project repository, create the agent instruction file your tool supports.
   - In that file, state that all generated code and reviews must comply with this standards repository and link to the required documents.
   - Keep links explicit so the agent can resolve the source of truth:
     - `https://github.com/EdenCorporations/standards/blob/main/DEVELOPMENT.md`
     - `https://github.com/EdenCorporations/standards/blob/main/TESTING.md`
     - `https://github.com/EdenCorporations/standards/blob/main/SECURITY.md`
     - `https://github.com/EdenCorporations/standards/blob/main/REVIEW_PROCESS.md`

3. **Prompt bootstrap (fallback for agents without persistent instructions).**
   - Start each new task/session with: "Use EdenCORP engineering standards as the governing policy for this work."
   - Include direct links to the standards files relevant to the task.

#### Recommended Agent Mapping

| Agent | Where to configure standards |
|---|---|
| GitHub Copilot | Repository custom instructions / chat context |
| Cursor | Project Rules |
| Claude Code | Project instruction file / session context |
| Cline / Roo Code | Workspace instruction/rules file |
| Continue | Assistant config and system message |
| Windsurf | Workspace rules/instructions |
| Any other coding agent | System prompt, project instructions, or attached context documents |

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

---

## Ownership

This repository is maintained by `@EdenCorporations/platform` under the authority of `@EdenCorporations/leadership`.

To propose a change to these standards, open a pull request and follow the process defined in [CONTRIBUTING.md](CONTRIBUTING.md).

---

## License

Copyright © 2026 EdenCORP. All rights reserved. Internal use only.
