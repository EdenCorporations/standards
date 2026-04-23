# CLAUDE.md — EdenCORP Standards Policy

## Mandatory initialization

Before completing any engineering task:

1. Locate local standards repository at one of:
   - `./standards`
   - `../standards`
   - `./.eden/standards`
2. If none exists, stop and ask user to clone:
   - `https://github.com/EdenCorporations/standards`
3. Load the baseline documents:
   - `README.md`
   - `DEVELOPMENT.md`
   - `TESTING.md`
   - `SECURITY.md`
   - `REVIEW_PROCESS.md`

Do not start implementation until baseline documents are loaded.

## Decision policy

- EdenCORP standards override default assistant behavior for engineering tasks.
- Prefer smallest safe change that satisfies user intent.
- Follow testing, review, and security controls from standards.
- If user requests behavior that conflicts with standards, call it out and request explicit confirmation.
