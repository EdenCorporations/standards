# Security Policy

EdenCORP takes security seriously across all systems, repositories, and data pipelines. This document defines our vulnerability disclosure policy, security baseline, and engineering requirements for all projects.

---

## Reporting a Vulnerability

**Do not open a public GitHub issue for security vulnerabilities.**

Report security vulnerabilities by emailing **security@edencorp.ai** with:

- A clear description of the vulnerability.
- Steps to reproduce (proof of concept if available).
- The affected repository, version, or system.
- Your assessment of impact and severity.

### Response SLAs

| Severity | Acknowledgement | Resolution Target |
|---|---|---|
| Critical (P0) | 4 hours | 24 hours |
| High (P1) | 24 hours | 7 days |
| Medium (P2) | 48 hours | 30 days |
| Low (P3) | 5 business days | 90 days |

We will keep you informed of progress throughout the resolution process.

---

## Disclosure Policy

- EdenCORP follows **responsible disclosure**. We ask that reporters allow us to address the issue before any public disclosure.
- We target a **90-day disclosure window** from the date of report.
- If we cannot resolve a critical issue within 90 days, we will notify the reporter and coordinate a joint disclosure.
- We do not pursue legal action against security researchers who act in good faith.

---

## Dependency Scanning

All EdenCORP repositories must:

- Enable **Dependabot** (or equivalent) for automated dependency vulnerability alerts.
- Address critical and high severity dependency alerts within the SLAs above.
- Pin all production dependencies to exact versions (avoid `^` and `~` in `package.json` for production deps).
- Conduct a full dependency audit (`npm audit`, `pip-audit`, or equivalent) as part of every release.

---

## Secret Scanning

- **GitHub Secret Scanning** must be enabled on all repositories.
- Pre-commit hooks (`detect-secrets` or `gitleaks`) are required in all repositories.
- Any committed secret is treated as compromised immediately. The credential must be rotated before any other action.
- Post-rotation, a postmortem must be completed for any secret that was exposed beyond the committing engineer's machine.

---

## OWASP Baseline

All web applications and APIs must address the [OWASP Top 10](https://owasp.org/www-project-top-ten/) before reaching production:

| OWASP Risk | EdenCORP Requirement |
|---|---|
| A01 Broken Access Control | RBAC enforced at API layer; deny-by-default |
| A02 Cryptographic Failures | TLS 1.2+ required; no plaintext sensitive data |
| A03 Injection | Parameterised queries; input validation on all endpoints |
| A04 Insecure Design | Threat modelling required for new services |
| A05 Security Misconfiguration | Hardened defaults; no debug endpoints in production |
| A06 Vulnerable Components | Dependency scanning in CI (see above) |
| A07 Identification & Auth Failures | MFA required; short-lived tokens; no long-lived API keys |
| A08 Software & Data Integrity | Signed releases; provenance tracking for AI models |
| A09 Logging & Monitoring Failures | Centralised logging; anomaly alerting |
| A10 SSRF | Allowlist-based outbound request validation |

---

## Data Encryption

- **In transit:** TLS 1.2 minimum, TLS 1.3 preferred, for all network communication including internal services.
- **At rest:** AES-256 encryption for all persistent data stores containing PII, credentials, or business-sensitive data.
- **Key management:** Encryption keys must be managed via a dedicated secrets manager (AWS KMS, HashiCorp Vault, or equivalent). No hardcoded keys in any codebase.
- **AI data:** All data used for model training or inference must follow the same encryption requirements as production data.

---

## Access Control

- **Principle of least privilege:** Every system, service account, and human account must have only the permissions required for its function.
- **MFA:** Required for all EdenCORP GitHub organisation members, cloud console access, and production system access.
- **Service accounts:** Must use short-lived credentials (OIDC tokens preferred over long-lived keys).
- **Access reviews:** Conducted quarterly by `@EdenCorporations/platform`. Unused access is revoked within 30 days.
- **Production access:** Requires explicit approval. All production access is logged and auditable.

---

## Security Review Triggers

A dedicated security review is required before deploying:

- Any new external-facing API endpoint.
- Any feature that handles PII, payment data, or health data.
- Any change to authentication or authorisation logic.
- Any new AI model integration or data pipeline.
- Any infrastructure change that modifies network security groups, IAM policies, or firewall rules.

Security reviews are conducted by `@EdenCorporations/platform` with escalation to external auditors for P0-class systems.
