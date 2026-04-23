# GitHub Copilot Instructions — EdenCORP Standards

## Mandatory startup sequence

Before performing implementation, refactoring, or review tasks:

1. Check whether the EdenCORP standards repository is available locally at one of:
   - `./standards`
   - `../standards`
   - `./.eden/standards`
2. If none exists, clone it automatically when tools allow:
   - `git clone https://github.com/EdenCorporations/standards`
3. If cloning is not possible in this environment, stop and ask the user:
   - "I need the EdenCORP standards repository to proceed, but I cannot clone it from this environment."
   - Ask user to clone: https://github.com/EdenCorporations/standards
   - Ask user to provide the local path.
4. After a valid path exists, read all standards docs before continuing:
   - Read every root-level Markdown standards file in the standards repo.
   - Read templates that affect implementation/review process.
5. Extract relevant requirements and store them in persistent project memory:
   - Preferred: agent-native persistent memory.
   - Fallback: create/update `.eden/agent-memory/standards-memory.md`.
6. At the start of every future task, load that memory before implementation.

Do not continue with code changes until this sequence is complete.

## Working policy

- Treat EdenCORP standards as governing policy.
- Prefer minimal, targeted changes.
- Follow security and testing requirements from the standards docs.
- If a user request conflicts with standards, explain the conflict and ask for explicit override approval.
- Include a short compliance summary in responses (what standards were applied).
