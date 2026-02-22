# Testing Standards

This document defines the testing requirements, coverage targets, and quality gates for all EdenCORP projects.

---

## Testing Philosophy

Testing at EdenCORP is a first-class engineering discipline. Tests are not optional, not cosmetic, and not the last thing you write. They define the contract that your code must satisfy.

Every feature ships with tests. Every bug fix ships with a regression test. CI blocks merge if tests fail.

---

## Test Types and Coverage Targets

### Unit Tests

- **Target coverage:** ≥ 80% line coverage across the codebase.
- **Target coverage (critical paths):** ≥ 95% line coverage for payment, auth, and AI pipeline code.
- Test individual functions, classes, and modules in isolation.
- Mock all external dependencies (databases, APIs, queues).
- Unit tests must be fast: the full unit test suite must complete in < 60 seconds.

### Integration Tests

- Test interactions between components: service ↔ database, service ↔ cache, service ↔ message queue.
- Use real (containerised) infrastructure via Docker Compose or Testcontainers — not mocks.
- Every API endpoint must have at least one integration test covering the happy path and one covering the primary error case.
- Integration tests run in CI on every PR.

### End-to-End Tests

- Test complete user flows from the UI through to the database.
- Written in Playwright.
- Must cover all critical user journeys defined in the product specification.
- E2E tests run in CI on the `staging` environment as part of the deployment pipeline (not on every PR, due to execution time).
- E2E tests are tagged by criticality. `@critical` tests also run as synthetic monitors in production.

### Test Pyramid

```
         /\
        /  \        E2E (few, high value)
       /----\
      /      \      Integration (moderate)
     /--------\
    /          \    Unit (many, fast)
   /____________\
```

---

## Performance Testing

- **Load tests** must be run before every major release for Tier 1 services.
- Load tests simulate at least 2× expected peak traffic.
- Use `k6` for load testing. Load test scripts are stored in `tests/load/`.
- Define pass/fail criteria in the load test script (e.g., p99 latency < 500ms, error rate < 0.1%).
- Results are published to the observability dashboard.
- **Stress tests** are run quarterly on Tier 1 services to identify breaking points.

---

## Accessibility Testing

- Automated accessibility checks run in CI using `axe-core` via Playwright.
- All WCAG 2.1 AA violations detected by automated tooling are treated as failing tests.
- Manual accessibility audits are conducted before major UI releases using screen reader testing (NVDA + Chrome, VoiceOver + Safari).
- Accessibility test results are reported alongside other test results in CI.

---

## Security Testing

- **Dependency scanning:** `pnpm audit` (or equivalent) runs in CI. Critical vulnerabilities block merge.
- **Static application security testing (SAST):** CodeQL or Semgrep runs on every PR via GitHub Actions.
- **Secret scanning:** GitHub Secret Scanning and `gitleaks` pre-commit hook are enabled on all repositories.
- **DAST:** OWASP ZAP scans run against the staging environment as part of the release pipeline.
- Penetration testing is conducted annually by an external firm for all Tier 1 services. Findings are tracked in the security issue tracker.

---

## AI Evaluation Tests

AI features require evaluation tests in addition to standard software tests. See [AI_ENGINEERING.md](AI_ENGINEERING.md) for the full evaluation pipeline specification.

### Minimum Requirements

- **Golden dataset:** A set of representative input/expected output pairs for the feature.
- **Regression suite:** Evaluation against the golden dataset runs in CI on every prompt template or model version change.
- **Guardrail tests:** Tests that verify each guardrail correctly triggers on adversarial or out-of-scope inputs.
- **Refusal tests:** Tests that verify the model correctly refuses harmful or out-of-scope requests.
- **Latency tests:** Verify that the AI pipeline meets the defined latency budget at the expected percentile.

---

## Test Tooling

| Purpose | Tool |
|---|---|
| Unit and integration testing (Node.js) | Vitest |
| E2E testing | Playwright |
| Load testing | k6 |
| Accessibility testing | axe-core (via Playwright) |
| Coverage reporting | v8 / Istanbul (via Vitest) |
| Mocking | Vitest built-in mocking + MSW (for HTTP) |
| Snapshot testing | Vitest snapshots |

---

## CI Requirements Before Merge

The following checks must pass on every PR before merge is permitted:

- [ ] All unit tests pass.
- [ ] All integration tests pass.
- [ ] Code coverage meets minimum thresholds (80% overall, 95% for critical paths).
- [ ] No new ESLint errors or warnings (unless suppressed with justification).
- [ ] No new TypeScript errors.
- [ ] Dependency audit passes (no new critical CVEs).
- [ ] Secret scanning passes.
- [ ] SAST scan passes (no new high or critical findings).
- [ ] Accessibility tests pass (for front-end changes).

---

## Writing Good Tests

- **Arrange, Act, Assert:** Structure every test clearly.
- **One assertion per test (where practical).** Tests that assert one thing are easier to diagnose when they fail.
- **Test behaviour, not implementation.** Tests should survive refactoring. Avoid testing internal implementation details.
- **Descriptive test names:** `it('returns 404 when user does not exist')`, not `it('test 3')`.
- **Deterministic:** Tests must produce the same result on every run. No random data, no time-dependent assertions without mocking.
- **Independent:** Tests must not depend on execution order or shared mutable state.

---

## Test Data Management

- Use factories (e.g., `@faker-js/faker` + factory functions) to generate test data.
- Test data must not be copied from production. All test data is synthetic.
- Database state is reset between integration test runs using database transactions (rollback after each test) or by re-running migrations.
- Shared fixture data (e.g., seed data required for every test) is defined in `tests/fixtures/` and applied once per test suite.
