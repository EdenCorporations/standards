# Development Standards

This document defines the development baseline for all EdenCORP projects. Every repository must comply with these standards before reaching production.

---

## Required Technology Stack

### Core

| Layer | Technology | Version |
|---|---|---|
| Runtime | Node.js | LTS (current: 20.x) |
| Language | TypeScript | 5.x, strict mode |
| Web framework | Next.js | 14.x or latest stable |
| API framework | Fastify or Next.js API routes | Latest stable |
| ORM / Query Builder | Prisma or Drizzle | Latest stable |
| Package manager | pnpm | 8.x or latest |

**TypeScript `strict` mode is non-negotiable.** All new projects must have the following in `tsconfig.json`:

```json
{
  "compilerOptions": {
    "strict": true,
    "noUncheckedIndexedAccess": true,
    "exactOptionalPropertyTypes": true,
    "noImplicitReturns": true,
    "noFallthroughCasesInSwitch": true
  }
}
```

### Permitted Additions

Teams may add libraries to this baseline with justification. New dependencies must be evaluated against:

1. Active maintenance (last commit < 6 months, or a well-known foundation-backed project).
2. No critical CVEs in the advisory database.
3. Licence compatibility (MIT, Apache 2.0, ISC preferred).
4. Bundle size impact (for front-end dependencies).

---

## Linting and Formatting

### ESLint

All repositories must use ESLint with the EdenCORP shared config (published as `@edencorp/eslint-config`). Key rules enforced:

- `@typescript-eslint/no-explicit-any` — error
- `@typescript-eslint/no-unused-vars` — error
- `@typescript-eslint/explicit-function-return-type` — warn
- `no-console` — warn (use structured logger instead)
- `no-process-env` — warn (access env through a validated config module)
- `import/order` — error (enforced import ordering)

### Prettier

Formatting is handled by Prettier with the EdenCORP shared config:

```json
{
  "semi": true,
  "singleQuote": true,
  "printWidth": 100,
  "tabWidth": 2,
  "trailingComma": "all",
  "arrowParens": "always"
}
```

**Never mix ESLint and Prettier for formatting.** Prettier handles formatting; ESLint handles code quality.

### Enforcement

- Linting and formatting must run in CI as a required check.
- Pre-commit hooks (via `lefthook` or `husky`) must run `lint-staged` to prevent unformatted code from being committed.
- CI must fail if there are lint errors or formatting violations.

---

## Folder Structure Conventions

### Next.js Applications

```
src/
  app/               # Next.js App Router pages and layouts
  components/        # Shared UI components
    ui/              # Primitive, unstyled components
    features/        # Feature-specific components
  lib/               # Utility functions and shared logic
  hooks/             # Custom React hooks
  types/             # Shared TypeScript types and interfaces
  config/            # Environment config and constants
  server/            # Server-only code (API handlers, db queries)
    db/              # Database schema and query functions
    services/        # Business logic services
  middleware/        # Next.js middleware
public/              # Static assets
docs/
  adr/               # Architecture Decision Records
```

### Node.js Services

```
src/
  routes/            # API route handlers
  services/          # Business logic
  repositories/      # Data access layer
  models/            # Domain models and types
  middleware/        # HTTP middleware
  config/            # Configuration and environment
  utils/             # Utility functions
  events/            # Event publishers and handlers
tests/
  unit/
  integration/
docs/
  adr/
```

---

## Naming Conventions

| Element | Convention | Example |
|---|---|---|
| Files (components) | PascalCase | `UserProfile.tsx` |
| Files (utilities/services) | camelCase | `userService.ts` |
| Files (routes/pages) | kebab-case | `user-profile/page.tsx` |
| Variables and functions | camelCase | `getUserById` |
| Constants | SCREAMING_SNAKE_CASE | `MAX_RETRY_COUNT` |
| TypeScript interfaces | PascalCase with `I` prefix avoided | `User`, `ApiResponse` |
| TypeScript types | PascalCase | `UserId`, `Pagination` |
| Enums | PascalCase with PascalCase members | `UserRole.Admin` |
| Database tables | snake_case | `user_sessions` |
| Database columns | snake_case | `created_at` |
| Environment variables | SCREAMING_SNAKE_CASE | `DATABASE_URL` |
| CSS classes (Tailwind) | Use utility classes directly; avoid custom class names except for component-level semantics |

---

## Environment Variable Handling

- All environment variables must be defined in a `.env.example` file committed to the repository.
- **Never commit `.env` or any file containing real values.**
- Environment variables must be validated at startup using a schema validation library (e.g., `zod`).

```typescript
// src/config/env.ts
import { z } from 'zod';

const envSchema = z.object({
  DATABASE_URL: z.string().url(),
  API_KEY: z.string().min(32),
  NODE_ENV: z.enum(['development', 'staging', 'production']),
  PORT: z.coerce.number().default(3000),
});

export const env = envSchema.parse(process.env);
```

- Applications must fail fast at startup if required environment variables are missing or invalid.
- Document the purpose of every variable in `.env.example` using comments.

---

## Secrets Management

- Secrets (API keys, database credentials, signing keys) must never be stored in environment variable files on production infrastructure.
- Use the designated secrets manager for your environment:
  - **AWS:** AWS Secrets Manager or Parameter Store.
  - **Local development:** `.env` files (never committed).
- Secrets must be rotated on a schedule defined by their sensitivity (see [SECURITY.md](SECURITY.md)).
- Service accounts must use short-lived credentials (OIDC tokens) rather than long-lived keys wherever possible.

---

## Dependency Management

- Use `pnpm` for all Node.js projects.
- Lock files (`pnpm-lock.yaml`) must be committed and kept in sync.
- Dependencies are separated into `dependencies` (runtime) and `devDependencies` (build/test).
- Production dependencies are pinned to exact versions. Development dependencies may use patch-level ranges.
- `npm audit` / `pnpm audit` must run in CI. Builds fail on critical vulnerabilities.
- Dependabot is configured for weekly dependency updates on all repositories.
- Unused dependencies must be removed. Use `depcheck` or equivalent in CI.

---

## Accessibility Requirements

All user-facing interfaces must meet **WCAG 2.1 Level AA** as a minimum.

| Requirement | Detail |
|---|---|
| Colour contrast | Text contrast ratio ≥ 4.5:1 (normal), ≥ 3:1 (large text) |
| Keyboard navigation | All interactive elements reachable and operable via keyboard |
| Focus management | Visible focus indicators on all interactive elements |
| Screen reader support | Semantic HTML; ARIA attributes where semantics are insufficient |
| Images | All meaningful images have descriptive `alt` text |
| Forms | All inputs have associated labels |
| Error messages | Errors are programmatically associated with the relevant field |

Accessibility testing must run in CI using `axe-core` or `@playwright/test` with accessibility assertions.

---

## Performance Budgets

### Web (Front-End)

| Metric | Budget |
|---|---|
| JavaScript bundle (initial load) | < 200 KB gzipped |
| Total page weight | < 1 MB gzipped |
| Lighthouse Performance score | ≥ 90 |
| LCP | < 2.5 seconds |
| CLS | < 0.1 |

Performance budgets are enforced via Lighthouse CI in the build pipeline.

### API

| Metric | Target |
|---|---|
| p50 response time | < 100ms |
| p95 response time | < 300ms |
| p99 response time | < 500ms |

---

## Logging Standards

- Use a structured JSON logger (`pino` for Node.js services).
- Never log to `console.log` directly in application code.
- Log levels: `fatal`, `error`, `warn`, `info`, `debug`, `trace`. Default to `info` in production.
- Every log entry must include: `timestamp`, `level`, `service`, `requestId` (if in request context), `message`.
- **Never log sensitive data:** passwords, tokens, PII, or full request/response bodies containing user data.
- Log all inbound requests (method, path, status, duration) at `info` level.
- Log all errors with a full stack trace at `error` or `fatal` level.
- Logs are shipped to the centralised logging platform (see [INFRASTRUCTURE.md](INFRASTRUCTURE.md)) and retained per the data retention policy.

```typescript
// Example structured log
logger.info({
  requestId: ctx.requestId,
  userId: ctx.userId,
  method: 'GET',
  path: '/api/v1/users',
  status: 200,
  durationMs: 45,
}, 'Request completed');
```
