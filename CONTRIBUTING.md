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

## CI and merge policy

Deterministic checks remain authoritative. AI review augments CI; it does not replace tests, linting, type checking, builds, security scanning, or deployment gates.

Busy or high-risk repositories may use merge queues. Repositories using merge queues must run their required CI on both `pull_request` and `merge_group` events.

## Automation and agents

Repository-local `AGENTS.md`, `.github/instructions/*.instructions.md`, and task-specific skills may strengthen these organization defaults. Repository-local rules take precedence when they are more restrictive.

Agents should prefer draft pull requests for incomplete work, preserve a reviewable audit trail, and avoid direct pushes to protected default branches.
