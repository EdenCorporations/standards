# Commit Conventions

EdenCORP uses the [Conventional Commits](https://www.conventionalcommits.org/) specification for all commit messages. This enables automated changelog generation, semantic versioning, and a readable project history.

---

## Commit Message Format

```
<type>(<scope>): <short description>

[optional body]

[optional footer(s)]
```

### Rules

- The header (first line) must not exceed **72 characters**.
- The type and description are **required**. The scope, body, and footer are optional.
- Use the **imperative mood** in the description: "add feature", not "added feature" or "adds feature".
- Do not capitalise the first letter of the description.
- Do not end the description with a period.

---

## Types

| Type | When to use |
|---|---|
| `feat` | A new feature for the user |
| `fix` | A bug fix for the user |
| `docs` | Documentation changes only |
| `style` | Formatting, whitespace — no logic change |
| `refactor` | Code change that is neither a feature nor a bug fix |
| `perf` | A code change that improves performance |
| `test` | Adding or updating tests |
| `build` | Changes to the build system or dependencies |
| `ci` | Changes to CI/CD configuration |
| `chore` | Other changes that don't modify src or test files |
| `revert` | Reverts a previous commit |

---

## Scope

The scope is the area of the codebase affected. Use a short, consistent noun:

- Service or module name: `auth`, `payments`, `model-gateway`
- Layer name: `api`, `db`, `ui`
- Cross-cutting: `deps`, `config`, `infra`

---

## Examples

### Feature

```
feat(auth): add refresh token rotation

Implements automatic token rotation on refresh. The old refresh token
is invalidated when a new one is issued.

Closes ENG-142
```

### Bug Fix

```
fix(payments): correct currency rounding for EUR amounts

EUR amounts were being rounded using the wrong number of decimal places,
causing off-by-one errors in invoice totals.

Fixes ENG-198
```

### Breaking Change

```
feat(api)!: remove deprecated v1 user endpoint

The /v1/users endpoint has been deprecated since v2.0.0 and is now
removed. Consumers must migrate to /v2/users.

BREAKING CHANGE: /v1/users is no longer available. See migration guide
in docs/migrations/v2.md.
```

### Documentation

```
docs(ai-engineering): add hallucination mitigation examples
```

### Dependency Update

```
build(deps): update next.js to 14.2.3
```

### CI Change

```
ci: add accessibility testing step to PR workflow
```

### Revert

```
revert: feat(auth): add refresh token rotation

Reverts commit a1b2c3d because it caused session invalidation issues
under high concurrency. See ENG-201.
```

---

## Breaking Change Syntax

A breaking change is indicated by:

1. An exclamation mark after the type/scope: `feat(api)!: ...`
2. A `BREAKING CHANGE:` footer.

Both the `!` and the `BREAKING CHANGE:` footer should be included for clarity. A breaking change automatically bumps the **major** version.

```
feat(model-gateway)!: change inference API response schema

The response envelope has changed from `{ result: string }` to
`{ data: { content: string }, meta: { model: string, tokens: number } }`.

BREAKING CHANGE: All consumers of the model-gateway inference endpoint
must update their response parsing. See ENG-205 for the migration guide.
```

---

## Relationship to Semantic Versioning

Conventional Commits maps directly to semantic version increments:

| Commit type | Version impact |
|---|---|
| `fix` | Patch increment (`1.2.0` → `1.2.1`) |
| `feat` | Minor increment (`1.2.0` → `1.3.0`) |
| `BREAKING CHANGE` | Major increment (`1.2.0` → `2.0.0`) |
| `docs`, `style`, `chore`, `test`, `ci` | No version increment |

Automated tools (`release-please` or `semantic-release`) parse commit history to determine the next version and generate the `CHANGELOG.md`. This only works if commits follow the Conventional Commits format strictly.

See [VERSIONING.md](VERSIONING.md) for the full versioning policy.

---

## Enforcement

- Commit message format is validated in CI using `commitlint` with `@commitlint/config-conventional`.
- A `commit-msg` git hook validates the format locally before commit.
- Non-conforming commits fail the CI check and cannot be merged.
- PR titles must also follow the Conventional Commits format (the PR title becomes the squash commit message).
