# Principles

These are the rules the harness enforces. Each one was bought with a real defect that reached a gate, a reviewer, or production in the codebase the harness was extracted from. The incidents stay private; the rules are the product.

## 1. Verdict lines are the truth; exit codes are routing hints

Every lane prints exactly one line of the form `LANE-EXIT=<n>` plus the tool's own summary (for a test runner: the suites and tests counts). A coordinator reads the summary lines before believing the exit code. Three things exit codes get wrong in practice: a runner that hangs after printing a full pass, a harness that is missing (`command not found`), and a classifier that relabels a real red as "infrastructure". None of those survive reading the summary line.

## 2. A guard is proven to fail before it is trusted

A fix and its guard land in the same commit. The commit message records the mutation: which line was reverted or altered, and the red that produced. A guard that has never been seen red is decoration, and the harness treats an unproven guard as missing.

## 3. Never weaken a guard silently

Changing a guard's assertion, its fixture, or its allowlist is a ledger event. The harness's own learning loop may add or strengthen guards; weakening requires a human-signed ledger row and a fresh mutation proof.

## 4. Guards assert intent, never spelling

A guard that matches the exact text of an implementation (`$2` in a SQL string, a specific variable name, a log message) breaks on the next correct change. Assert the property: which parameter carries the site id, which rows a query can return, which branch a request takes.

## 5. Sibling paths are covered together

The dominant defect class in agent-written code: a fix lands on the path the author was looking at and its sibling stays broken. Every guard scenario for an invariant runs on every path that implements it, or the ledger row names why a path is exempt.

## 6. Partition coverage

For every fail-closed condition and every sentinel (zero, null, missing, not-completed), the truth table is enumerated and the untested cell is named. "Handles the error case" is not a test plan.

## 7. Claims the code keeps

Every user-facing claim of a limit, expiry, or validation names the code that enforces it. Copy that promises what nothing enforces is a finding.

## 8. Contract existence

Every literal API path a client calls resolves end to end to a route registration for that verb. Mocked fetch plus a green typecheck will not tell you the path returns 404.

## 9. Concurrency is provable

Every select-then-insert find-or-create is either protected in code (a transaction-scoped advisory lock, `ON CONFLICT`, `FOR UPDATE`) or registered with the unique index that already covers it. A mocked pool has one imaginary connection and will never show you the race.

## 10. Numbers are allocated at merge

Invariant ids and migration numbers are placeholders on a branch (`INV-XXX`, `NNN-XXX`). The merge step allocates from the authority (the ledger tail and the migrations directory on the integration branch), never from a remembered value. Duplicate numbers and citations of files that do not exist fail the gate.

## 11. The reviewer reads the branch, not the diff

A first-pass reviewer works over git objects at the head ref with read, grep and list tools, so it can follow a call to its siblings and a client to its route. It is briefed with the standing questions (principles 4 to 9) on every run, and its findings are verified at the desk before code moves.

## 12. The harness learns add-only

Accepted findings and gate reds are recorded with their commit and classified against the known gap classes. New classes become standing questions; instances become guard skeletons. Every learned pattern cites its evidence so a human can audit why the harness believes it.
