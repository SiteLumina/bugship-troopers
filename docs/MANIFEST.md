# gate.config.json

One manifest per adopting project, written by the `init` skill and read by every other part of the harness. Draft schema; the coupling inventory refines it.

```jsonc
{
  "profile": {
    "kind": "multi-tenant-saas",          // multi-tenant-saas | single-tenant-app | library | cli
    "money": true,                        // enables the money pack
    "auth": true                          // enables the auth pack
  },
  "stacks": [
    {
      "name": "api",
      "dir": "api",
      "typecheck": "npm run typecheck",   // must cover test files too
      "lint": "npm run lint",
      "lanes": {
        "regression": "npm run test:regression",
        "security": "npm run test:security"
      },
      "build": "npm run build"
    },
    {
      "name": "web",
      "dir": "web",
      "typecheck": "npx tsc --noEmit",
      "lanes": { "unit": "npx jest" },
      "build": "npm run build"
    }
  ],
  "db": {
    "isolation": "postgres-template-clone", // none | postgres-template-clone
    "template": "app_test",
    "migrationsDir": "api/migrations",
    "runner": "api/dist/migration-runner.js"
  },
  "guards": {
    "packs": ["core", "tenant", "money", "auth", "api-contract", "concurrency"],
    "dir": "guards"
  },
  "reviewers": {
    "first-pass": { "provider": "gemini", "model": "gemini-3.1-pro-preview", "keyEnv": "GEMINI_API_KEY" },
    "security":   { "provider": "openai", "model": "gpt-6-astra",          "keyEnv": "OPENAI_API_KEY" }
  },
  "ledger": {
    "file": "docs/INVARIANTS.md",
    "idPrefix": "INV",
    "migrationPattern": "^[0-9]{3}-"
  },
  "conventions": {
    "currencyUnit": "minor-integer",      // pence / cents as integers
    "tenantColumn": "site_id",
    "scopingHelper": "siteFilter"
  }
}
```

Rules:
- Reviewer keys are named by environment variable, never stored.
- A pack listed in `guards.packs` whose precondition (for example `conventions.tenantColumn`) is absent reports `NOT-APPLICABLE` on its verdict line rather than passing.
- `stacks[].typecheck` must include test files; a typecheck that excludes tests is a known way to ship a test that does not parse.
