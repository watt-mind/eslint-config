# @watt-mind/eslint-config

Shared ESLint 9 flat-config presets for Watt Mind repos. Part of the code-security
baseline (hdkiller `docs/guides/code-security.md`, tier 2); build-out tracked in
Linear OPS-164 (blocked on OPS-166 — adding `NPM_TOKEN` for tag-triggered publish).

## Tooling Decision (ESLint 9 vs. Oxlint)

**ESLint 9 flat config** is the universal cross-repo baseline because Watt Mind
repositories span Nuxt/Vue (`coach-wattz`), React, and Node/Bun services, and
require `eslint-plugin-security` plus custom AST rules (`tenant-isolation`).
Repos that want faster inner-loop linting may optionally layer Oxlint (`oxlint` +
`tsrs`) alongside ESLint, without making Oxlint a hard requirement for the shared
baseline.

## Planned Exports

- `@watt-mind/eslint-config/base` — core correctness & hygiene (`eqeqeq`,
  `no-eval`, `import/no-self-import`, `@typescript-eslint/consistent-type-imports`)
  with scoped test-file overrides (`**/*.{test,spec,integration,e2e}.{ts,tsx,mts,cts}`,
  `**/__tests__/**`) that relax `no-explicit-any`, `no-unsafe-*`,
  `no-non-null-assertion`, `only-throw-error`, and `unbound-method` in tests
  only so production code stays strict as `error`.
- `@watt-mind/eslint-config/security` — `eslint-plugin-security` baseline,
  type-aware promise/error rules (`no-floating-promises`, `no-misused-promises`,
  `no-promise-executor-return`, `only-throw-error`,
  `use-unknown-in-catch-callback-variable`, `switch-exhaustiveness-check`,
  unsafe-`any` guards), and logging hygiene (`no-console` outside CLI/script
  entrypoints + `no-log-exception-with-throw`).
- `@watt-mind/eslint-config/react` — `eslint-plugin-react-hooks` +
  `react/no-danger` (with `checksVoidReturn: { attributes: false }` guidance for
  async JSX handlers).
- `@watt-mind/eslint-config/tenant-isolation` — opt-in multi-tenant & API
  guardrails: configurable `no-restricted-imports` seam guards plus custom AST
  rules (unscoped multi-tenant Prisma query guard and required route middleware),
  each documented with a rule `README.md` so coding agents know how to fix
  violations.

## Agent Migration Skill

The package will also ship `skills/migrate-eslint-config/SKILL.md` so coding
agents can adopt the preset incrementally in any repo:

1. Adopt the preset in one passing baseline commit, temporarily disabling newly
   failing rules.
2. Enable and fix one rule per commit (or document a scoped override with a
   reason).
3. Squash and merge once all rules are evaluated.

## Distribution

Published to npm as a public scoped package; plugin versions are pinned as
`dependencies` so consumers inherit them from one place.
