# Versioning Standards

EdenCORP uses [Semantic Versioning 2.0.0](https://semver.org/) for all libraries, services, and APIs. Versioning is automated based on [Conventional Commits](COMMIT_CONVENTIONS.md).

---

## Semantic Versioning Rules

Version numbers follow the format `MAJOR.MINOR.PATCH`:

| Segment | Increments when |
|---|---|
| `MAJOR` | A breaking change is introduced. Consumers must update. |
| `MINOR` | A new, backward-compatible feature is added. |
| `PATCH` | A backward-compatible bug fix is applied. |

### Rules

- `MAJOR` version 0 (`0.x.y`) is pre-stable. Breaking changes may occur at any minor version.
- Once `MAJOR` reaches 1, breaking changes require a major version bump.
- Pre-release identifiers are appended with a hyphen: `1.3.0-beta.1`, `1.3.0-rc.2`.
- Build metadata may be appended with a plus sign: `1.3.0+build.20251015`.

### What constitutes a breaking change

- Removing or renaming a public API endpoint, function, or field.
- Changing the type or shape of an existing response or parameter.
- Removing or changing the semantics of a configuration option.
- Changing the required Node.js version or other runtime requirement.
- Removing support for a previously supported input format.

### What does not constitute a breaking change

- Adding a new optional parameter.
- Adding a new response field that was previously absent.
- Internal refactoring with no public interface changes.
- Performance improvements with no behaviour change.
- Bug fixes that correct unintentional behaviour (even if consumers relied on the bug — document clearly).

---

## Release Tagging Format

- All releases are tagged on the `main` branch using annotated git tags.
- Tag format: `v<MAJOR>.<MINOR>.<PATCH>` (e.g., `v1.3.0`).
- Pre-release tags: `v1.3.0-beta.1`, `v1.3.0-rc.1`.
- Tags are created after a successful production deployment, not before.
- Tags are created by `@EdenCorporations/platform`.

### Tagging Command (for reference)

```bash
git tag -a v1.3.0 -m "Release v1.3.0

Adds refresh token rotation and fixes EUR rounding bug.
See CHANGELOG.md for full details."
```

---

## Changelog Requirements

Every repository must maintain a `CHANGELOG.md` in the root directory.

### Format

EdenCORP follows the [Keep a Changelog](https://keepachangelog.com/) format:

```markdown
# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
and this project adheres to [Semantic Versioning](https://semver.org/).

## [Unreleased]

### Added
- ...

### Changed
- ...

### Fixed
- ...

## [1.3.0] - 2025-10-15

### Added
- Refresh token rotation (ENG-142)
- Support for EUR currency in invoice totals (ENG-189)

### Fixed
- EUR amounts rounded incorrectly for amounts > €1,000 (ENG-198)

### Changed
- Model gateway response envelope now includes `meta.tokens` field

## [1.2.1] - 2025-09-28

### Fixed
- Token expiry off-by-one error causing premature session invalidation (ENG-198)

## [1.2.0] - 2025-09-15

### Added
- User authentication via OAuth 2.0 (ENG-105)
```

### Rules

- The `[Unreleased]` section accumulates changes since the last release.
- On release, rename `[Unreleased]` to `[<version>] - <date>` and add a new empty `[Unreleased]` section.
- Changes are categorised as: `Added`, `Changed`, `Deprecated`, `Removed`, `Fixed`, `Security`.
- Every entry includes the associated ticket number.
- `CHANGELOG.md` is generated automatically by `release-please` based on Conventional Commits, then reviewed before release.

---

## Deprecation Policy

Deprecation is the process of marking a feature, API, or behaviour as scheduled for removal. EdenCORP follows a structured deprecation process to avoid breaking consumers without warning.

### Process

1. **Mark as deprecated.** In the release where the deprecation begins, mark the feature as `@deprecated` in code and add a `Deprecated` entry to `CHANGELOG.md`. The deprecation notice must include the version in which the feature will be removed and the recommended alternative.

2. **Minimum deprecation period:**
   - Public APIs: 90 days minimum, or 2 minor versions, whichever is longer.
   - Internal libraries: 30 days minimum, or 1 minor version.
   - CLI flags or config options: 60 days minimum.

3. **Communication.** Deprecation notices are:
   - Added to the API documentation.
   - Communicated to affected teams via Slack and email.
   - Tracked in a deprecation register maintained by `@EdenCorporations/platform`.

4. **Remove.** After the deprecation period, the feature is removed in a `MAJOR` version bump. The removal is documented as a breaking change in `CHANGELOG.md`.

### Example Deprecation Notice in Code

```typescript
/**
 * @deprecated Since v1.3.0. Use `getUserById` instead.
 * Will be removed in v2.0.0.
 */
export function fetchUser(id: string): Promise<User> {
  console.warn('fetchUser is deprecated. Use getUserById instead.');
  return getUserById(id);
}
```
