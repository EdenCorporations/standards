# GitHub Copilot Instructions — EdenCORP Standards

## Mandatory startup sequence

Before performing implementation, refactoring, or review tasks:

1. Check whether the EdenCORP standards repository is available locally at one of:
   - `./standards`
   - `../standards`
   - `./.eden/standards`
2. If none exists, stop and ask the user:
   - "I need the EdenCORP standards repository to proceed. Please clone `https://github.com/EdenCorporations/standards` and tell me the local path."
3. After a valid path is provided, read these files before continuing:
   - `README.md`
   - `DEVELOPMENT.md`
   - `TESTING.md`
   - `SECURITY.md`
   - `REVIEW_PROCESS.md`

Do not continue with code changes until this sequence is complete.

## Working policy

- Treat EdenCORP standards as governing policy.
- Prefer minimal, targeted changes.
- Follow security and testing requirements from the standards docs.
- If a user request conflicts with standards, explain the conflict and ask for explicit override approval.
- Include a short compliance summary in responses (what standards were applied).
