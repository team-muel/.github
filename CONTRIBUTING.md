# Team Muel contribution policy

## Source of truth

- Linear owns scope, priority, dependencies, and delivery state.
- GitHub owns implementation evidence: commits, pull requests, reviews, CI, releases, and deployments.
- Slack owns synchronous coordination and conflict resolution.
- Notion owns durable architecture, runbooks, and long-lived knowledge.

Do not duplicate planning state in GitHub Issues or GitHub Projects when a Linear issue already exists.

## Pull requests

Every non-trivial change should be delivered through a pull request. Link the relevant Linear issue when one exists.

Prefer small, single-purpose pull requests. A pull request should make one coherent change and should be independently reviewable and revertible.

Before requesting review:

1. Run the repository's required tests and static checks.
2. Describe the failure modes and regression surface.
3. Identify high-risk files or areas in the review map.
4. Confirm that credentials, tokens, private data, and generated artifacts were not committed.
5. Keep implementation notes in the pull request; keep long-term architecture in Notion.

## Review order

Review in this order:

1. Correctness and invariant preservation.
2. Failure behavior and recovery.
3. Security and authority boundaries.
4. Tests and regression coverage.
5. Maintainability and clarity.

Resolve or explicitly disposition every material review thread before merge. Do not treat an AI review comment as informational-only when it identifies a correctness, security, data-integrity, or governance risk.

## Review completion gate

For a normal pull request, `review pending` means **do not merge**. The expected sequence is:

1. Deterministic CI passes.
2. Automated review has completed, including Codex when configured for the repository.
3. Material findings are fixed or explicitly dispositioned.
4. Review threads are resolved.
5. If fixes materially change the diff, request/re-run review and wait for a clean result.
6. Merge only after the final review state and required checks are known.

A PR must not be merged merely because no finding has arrived yet. Absence of a finding while an automated review is still running is not approval.

### Emergency hotfix exception

An emergency hotfix may bypass the normal review-completion wait only when delaying the merge would materially worsen an active production/security incident.

When using the exception:

- state `Emergency hotfix` and the incident/risk in the PR body;
- run every deterministic check that can complete without worsening the incident;
- keep the change minimal and independently revertible;
- request the normal automated review before or immediately after merge;
- review late findings as mandatory remediation work, not informational comments;
- create/link a Linear remediation issue for every material late finding that is not fixed immediately.

The emergency exception is not a convenience path for ordinary urgent work.

## Late-finding remediation

If a material automated-review finding arrives after merge:

1. Re-evaluate it against the current default branch.
2. Classify it as already fixed, no longer applicable, or still reproducible.
3. For reproducible correctness/security/data-integrity/governance findings, open or update a Linear remediation issue and fix it with regression coverage.
4. Link the fixing PR/commit back to the Linear issue and resolve the original review thread when possible.

## CI and merge policy

Deterministic checks remain authoritative. AI review augments CI; it does not replace tests, linting, type checking, builds, security scanning, or deployment gates.

Busy or high-risk repositories may use merge queues. Repositories using merge queues must run their required CI on both `pull_request` and `merge_group` events.

Repositories should protect the default branch so normal merges require a pull request, required deterministic checks, and conversation resolution. Direct pushes to the protected default branch should be restricted. Repository rulesets may strengthen these defaults.

## Automation and agents

Repository-local `AGENTS.md`, `.github/instructions/*.instructions.md`, and task-specific skills may strengthen these organization defaults. Repository-local rules take precedence when they are more restrictive.

Agents should prefer draft pull requests for incomplete work, preserve a reviewable audit trail, and avoid direct pushes to protected default branches.
