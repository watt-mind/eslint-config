# @watt-mind/eslint-config

Shared ESLint 9 flat-config presets for Watt Mind repos. Part of the code-security
baseline (hdkiller `docs/guides/code-security.md`, tier 2); build-out tracked in
Linear OPS-164 (blocked on OPS-166 — claiming the `watt-mind` npm scope).

Planned exports:

- `@watt-mind/eslint-config/security` — eslint-plugin-security baseline, `no-floating-promises`, unsafe-`any` guards
- `@watt-mind/eslint-config/react` — react-hooks rules + `react/no-danger`
- `@watt-mind/eslint-config/tenant-isolation` — configurable `no-restricted-imports` seam guards for multi-tenant apps

Published to npm as a public scoped package; plugin versions are pinned as
`dependencies` so consumers inherit them from one place.
