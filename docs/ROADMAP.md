# Roadmap

| Phase | Version | Deliverable |
|---|---|---|
| 0 | scaffold | This repository, the principles, the manifest schema, and the coupling inventory of the source harness (`docs/COUPLING-INVENTORY.md`). |
| 1 | 0.1 | Extract: lane runner with the verdict-line contract, reversed-order run, capacity lock and a failure-block-only contention classifier; ledger tool (placeholders, allocation, duplicate and citation checks); mutation-proof checklist skill; reviewers (Gemini and OpenAI over git objects with the standing questions); secret-scan, placeholder and no-test-no-merge hooks; guard packs `core`, `tenant`, `money`, `auth`, `api-contract`, `concurrency`; CI adapter (`init --ci github` writes a GitHub Actions workflow that runs the lane runner with a Postgres service container and prints the verdict lines as a check; reviewer keys as repository secrets named in the manifest) and a devcontainer option for local isolation. Every guard ships with its mutation proof. |
| 2 | 0.2 | Adopt: the source monorepo becomes adapter one through its own `gate.config.json`. Parity proof: both harnesses produce identical verdict lines on the same head for three consecutive merges before the in-repo copies are deleted. |
| 3 | 0.3 | Learn: catch-to-guard (a merge hook records the gap, classifies it, drafts the guard and a brief for human approval), pattern store mined into the next brief, reviewer calibration (finding outcome per reviewer and class). Add-only rule enforced by a guard on the guards directory. |
| 4 | 1.0 | Publish: documentation, a second adopter on the `single-tenant-app` profile proving the profile logic, licence, plugin marketplace listing. |

## Non-goals

The multi-agent build runtime that surrounds the harness in its source project (task roles, transport, sandboxing, scheduling) is not part of this repository. Bugship Troopers is the review-and-gate layer only; it works with one human and one coding agent as well as with a fleet.
