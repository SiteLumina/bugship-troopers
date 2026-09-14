# Bugship Troopers

**The only good bug is a dead bug.**

A review-and-gate harness for AI-assisted codebases. It installs as a Claude Code plugin and gives a project four things it usually lacks the day a coding agent starts writing to it:

1. **A gate that cannot be argued with.** Every lane (typecheck, lint, regression, reversed-order regression, security, build, frontend tests, repo guards) prints one verdict line. The verdict line is the truth; exit codes are routing hints. A red that is "probably flaky" gets read, not re-run.
2. **Guards that are proven to fail.** A fix ships with a test that goes red when the fix is reverted, in the same commit, with the proof written down. A guard nobody has seen fail is decoration.
3. **An invariant ledger.** Every fix records the rule it protects, where the rule is enforced, how a break shows, and which guard catches it. Numbers are allocated at merge, never guessed; duplicates and dead citations fail the gate.
4. **A first-pass reviewer with standing questions.** Before a human or a paid reviewer looks, an AI reviewer reads the branch through git objects with a brief that always asks the questions that have caught real defects: does every claimed limit have code that enforces it, does every guard assert what the ledger row says, is every sibling path covered, is every fail-closed condition's truth table tested.

And one thing most harnesses cannot do: it gets stricter the longer a project runs it. Accepted review findings and gate reds become guard skeletons and new standing questions, under an add-only rule (the harness may add or strengthen a guard on its own; weakening one needs a human-signed ledger row and a mutation proof).

## Who it is for

Anyone building with a coding agent who wants the agent's mistakes caught by machinery rather than by luck: solo builders, small teams, and people adopting a codebase they did not write.

## How it knows what kind of project you have

It asks, then verifies. `init` scans the repository for signals (tenant or organisation columns on most tables, a scoping helper, host-based routing, role tables, a payments SDK, a migrations directory, monorepo layout, frameworks), proposes a profile, and you confirm it once. The profile is written to `gate.config.json` and every other part reads it:

| Profile | Extra guard packs |
|---|---|
| `multi-tenant-saas` | tenant isolation, tenant config in the database never in env |
| `single-tenant-app` | core only, plus `money` and `auth` packs when flagged |
| `library` | core only |
| `cli` | core only |

A pack whose precondition is missing reports `NOT-APPLICABLE` on its verdict line. It never passes silently.

## Status

Scaffold. The harness is being extracted from a production monorepo where it retired a paid code-review service. See `docs/ROADMAP.md` for the phases and `docs/PRINCIPLES.md` for the rules the harness enforces. The coupling inventory that drives the extraction is in `docs/COUPLING-INVENTORY.md`.

## Layout (target)

```
.claude-plugin/plugin.json   plugin manifest
skills/                      init, gate, review, ledger, merge-desk
hooks/                       secret scan, placeholder check, no-test-no-merge
bin/                         lane runner, ledger tool, reviewers (node 20+, bash)
guards/                      guard packs: core, tenant, money, auth, api-contract, concurrency
docs/                        principles, roadmap, manifest schema, coupling inventory
```

## Where it runs

Local by default: the gate runs wherever your coding agent runs. Database isolation is a manifest choice, `none` (your existing test database) or `postgres-template-clone` (a local Postgres; `init` provisions the template). Nothing is installed into your GitHub unless you opt in: `init --ci github` writes a workflow into your own repository that runs the same lane runner on GitHub's runners with a Postgres service container and reports the verdict lines as a check. Reviewer keys go in as repository secrets under the names in your manifest.

## Requirements

Node 20 or newer, bash, git. Postgres only if you choose the `postgres-template-clone` isolation provider. Reviewer API keys are read from environment variables you name in the manifest; the harness never stores them.

## Licence

MIT. See `LICENSE`. Copyright 2026 Site Lumina.

Authored and maintained by Site Lumina.
