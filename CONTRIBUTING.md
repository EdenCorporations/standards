# Contributing to EdenCORP Standards

Thank you for contributing to the EdenCORP engineering standards repository. These documents define how we build software at scale, so changes must be deliberate, well-reasoned, and clearly communicated.

---

## Who Can Contribute

- **All EdenCORP engineers** may propose changes via pull request.
- **External contributors** are not accepted. This is an internal repository.
- **Final approval** on all changes rests with `@edencorp/platform` and `@edencorp/leadership`.

---

## Types of Contributions

| Type | Process |
|---|---|
| Typo / formatting fix | Open a PR directly |
| Clarification or wording improvement | Open a PR with rationale in the description |
| New standard or significant change | Open an issue first, discuss, then PR |
| Removal or deprecation of a standard | Requires ADR and leadership sign-off |

---

## Process

### 1. Check Existing Issues

Before opening a new issue or PR, search existing issues to avoid duplicates. If your proposal is already being discussed, add your perspective there.

### 2. Open an Issue (for significant changes)

For any non-trivial change, open an issue describing:

- What you want to change and why.
- The impact on existing projects.
- Any alternatives you considered.

Label the issue `standards-proposal`.

### 3. Write Your Changes

- Follow the [Markdown formatting standards](DOCUMENTATION.md).
- Be specific. Avoid vague language like "should consider" — use "must", "should", or "may" consistently.
- Use `must` for non-negotiable requirements.
- Use `should` for strong recommendations.
- Use `may` for optional guidance.

### 4. Open a Pull Request

Use the [PR template](templates/pull_request_template.md). Your PR must include:

- A clear summary of what changed and why.
- A link to the related issue (if applicable).
- Confirmation that you have read the affected standards document in full.
- An assessment of which existing projects will be impacted.

### 5. Review

- All PRs require at least **one approval** from `@edencorp/platform`.
- Changes to security, AI, or architecture standards additionally require approval from `@edencorp/leadership`.
- Reviews are expected within **3 business days**.

### 6. Merge

Approved PRs are merged by `@edencorp/platform` using squash merge.

---

## Compliance Checklist

Use this checklist to assess whether your repository complies with EdenCORP standards:

### Development
- [ ] TypeScript strict mode enabled
- [ ] ESLint and Prettier configured and enforced in CI
- [ ] Environment variables documented in `.env.example`
- [ ] No secrets committed to the repository
- [ ] Dependencies audited and pinned

### Testing
- [ ] Unit test coverage ≥ 80%
- [ ] Integration tests exist for all critical paths
- [ ] CI fails on test failure

### Git & Branching
- [ ] `main` branch is protected
- [ ] PR required for all merges to `main`
- [ ] Commits follow Conventional Commits format

### Security
- [ ] Dependency scanning enabled (Dependabot or equivalent)
- [ ] Secret scanning enabled
- [ ] SECURITY.md present

### Documentation
- [ ] README.md present and up to date
- [ ] API documentation complete
- [ ] ADRs written for significant architectural decisions

---

## Questions

Open an issue or contact `@edencorp/platform` in Slack (`#platform-engineering`).
