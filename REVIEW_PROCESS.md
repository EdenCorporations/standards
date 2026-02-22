# Review Process

This document defines the code review standards, expectations, and requirements for all EdenCORP pull requests.

---

## Principles

- **Reviews are a collaborative act, not a gatekeeping act.** The goal is to ship better software together.
- **Review the code, not the person.** Feedback is directed at the change, not the author.
- **Be specific and actionable.** "This is wrong" is not useful. "This function could panic if `user` is nil — add a nil check before line 42" is useful.
- **Acknowledge good work.** If something is well-done, say so.
- **Respond promptly.** Reviews are expected within one business day. PRs should not sit unreviewed.

---

## PR Review Checklist

Reviewers must evaluate every PR against the following checklist:

### Correctness
- [ ] The change does what the PR description says it does.
- [ ] Edge cases are handled (empty inputs, nulls, boundary conditions).
- [ ] Error handling is complete and appropriate.
- [ ] No obvious logic bugs.

### Code Quality
- [ ] Code is readable and self-documenting.
- [ ] Functions are appropriately sized (a function doing one thing).
- [ ] No unnecessary duplication (DRY where it makes the code clearer, not at the cost of clarity).
- [ ] Naming is clear and consistent with the rest of the codebase.
- [ ] No dead code, commented-out blocks, or TODO comments without an accompanying ticket.

### Testing
- [ ] New behaviour is covered by tests.
- [ ] Bug fixes include a regression test.
- [ ] Tests are meaningful (not just coverage padding).
- [ ] Tests pass in CI.

### Security
- [ ] No secrets, credentials, or PII in the code or tests.
- [ ] User inputs are validated and sanitised.
- [ ] No new SQL injection, XSS, or SSRF vectors introduced.
- [ ] Access control is checked at the correct layer.

### Performance
- [ ] No obvious N+1 queries or unbounded loops over large datasets.
- [ ] Database queries are indexed appropriately.
- [ ] No memory leaks from unclosed resources.

### Documentation
- [ ] Public APIs are documented (JSDoc or equivalent).
- [ ] `README.md` updated if the change affects setup or usage.
- [ ] ADR written if the change involves a significant architectural decision.
- [ ] `CHANGELOG.md` updated for user-facing changes.

---

## Approval Requirements

| Change type | Approvals required |
|---|---|
| Standard feature or fix | 1 approval from any engineer on the team |
| Changes to auth, payments, or PII handling | 1 approval from `@edencorp/platform` |
| Changes to AI pipeline, prompts, or model config | 1 approval from `@edencorp/platform` + review against [AI_ENGINEERING.md](AI_ENGINEERING.md) |
| Changes to infrastructure or IaC | 1 approval from `@edencorp/platform` |
| Changes to these standards documents | 1 approval from `@edencorp/platform` + 1 from `@edencorp/leadership` |

Self-approval is never permitted.

---

## Code Quality Expectations

### What reviewers should comment on

- **Blocking issues (`must change`):** Correctness bugs, security vulnerabilities, missing tests, violations of EdenCORP standards.
- **Strong suggestions (`should change`):** Code clarity improvements, better naming, more appropriate patterns.
- **Optional suggestions (`nit`):** Minor style preferences, cosmetic suggestions. Mark with `nit:` prefix. Authors may choose to address or decline these.

### What reviewers should not do

- Block PRs over personal style preferences when the code complies with linting rules.
- Request unnecessary refactoring that is out of scope of the PR.
- Leave vague or non-actionable comments.

---

## Review Response Expectations

| Role | Expectation |
|---|---|
| Reviewer | Provide initial review within 1 business day of PR opening |
| Author | Respond to comments within 1 business day of receiving review |
| Reviewer (follow-up) | Re-review within 1 business day after author response |

If a PR has been blocked by review comments for > 3 business days with no response from either party, escalate to the team lead.

---

## Security Review Triggers

A dedicated security review by `@edencorp/platform` is required (in addition to the standard approval) when the PR:

- Adds or modifies authentication or authorisation logic.
- Changes how credentials or tokens are stored, transmitted, or validated.
- Adds a new external-facing API endpoint.
- Introduces a new dependency with network access.
- Modifies infrastructure security groups, IAM policies, or firewall rules.
- Processes, stores, or transmits PII or payment data.
- Adds or modifies AI data pipelines or model integrations.

---

## AI-Specific Review Criteria

PRs that touch AI features (prompts, model integration, evaluation, guardrails) must be reviewed against:

- [ ] Prompt templates are versioned and stored in `prompts/`.
- [ ] No PII or sensitive data in prompt templates.
- [ ] Guardrails are in place and tested.
- [ ] Evaluation tests cover the new or modified behaviour.
- [ ] Model version is pinned to a specific release.
- [ ] Observability metrics are emitted correctly.
- [ ] Human-in-the-loop requirements are satisfied where applicable.
- [ ] The change is compliant with [AI_ENGINEERING.md](AI_ENGINEERING.md) responsible AI principles.

---

## Merge Criteria

A PR may be merged when:

1. All required approvals are in place.
2. All CI checks pass.
3. All blocking review comments are resolved.
4. The branch is up to date with the target branch.
5. The PR title follows the Conventional Commits format.

Merge is performed by the **PR author** after all criteria are satisfied, unless a `@edencorp/platform` team member is performing the merge as part of a release or hotfix process.
