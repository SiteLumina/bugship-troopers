# Status

Updated 2026-09-15. Parked after Phase 0 while the source project's own queue takes priority; resumes at Phase 1.

## Done
- Public repository created, MIT licence, scaffold: README, principles (12 rules), roadmap (phases 0 to 4), draft `gate.config.json` schema, plugin manifest, contributing note.

## In progress
- Nothing. Phase 1 has not started.

## To do (in order)
1. Phase 0 coupling scout (not yet run): inventory the source harness's hard-coded assumptions, write `docs/COUPLING-INVENTORY.md`, check it for anything that must not be public, refine `docs/MANIFEST.md` from it.
2. Phase 1 extraction slices (disjoint file ownership, one verifier per slice, every guard with its mutation proof):
   - `bin/gate`: lane runner, verdict-line contract, reversed-order run, capacity lock, failure-block-only contention classifier.
   - `bin/ledger`: placeholder allocation at merge, duplicate and dead-citation checks.
   - `bin/review`: Gemini and OpenAI reviewers over git objects, brief generator with the standing questions.
   - `hooks/`: secret scan, placeholder check, no-test-no-merge.
   - `guards/core`: no-SIGPIPE sweep, spelling-pin detector, sibling-path census.
   - `guards/api-contract`, `guards/concurrency`, `guards/tenant`, `guards/money`, `guards/auth`.
   - `skills/`: init (repo scan + profile interview + manifest), gate, review, ledger, merge-desk.
   - CI adapter (`init --ci github`) and a devcontainer option.
3. Phase 2: first adopter with a three-merge parity proof.
4. Phase 3: add-only learn loops.
5. Phase 4: second adopter on `single-tenant-app`, marketplace listing.

## Decisions taken
- Name and tagline; public; MIT; authored by Site Lumina; "installs as a Claude Code plugin" wording accepted.
- Node 20 and bash only at runtime; Postgres optional.
- The multi-agent runtime stays out of scope.
