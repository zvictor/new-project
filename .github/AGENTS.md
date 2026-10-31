# GitHub automation

## Purpose

Run development checks and the reusable Bonsai allowance check.

## Ownership

- `workflows/ci.yml`: formatting, applying formatting patches, type checking, and tests.
- `workflows/bonsai.yml`: the sole Bonsai CI invocation and its report artifact.

## Local Contracts

- Run Bonsai for every pull request and merge-group candidate, using the default
  merge checkout, full history, read-only permissions, no persisted credentials,
  and an ephemeral GitHub-hosted runner.
- Pin `jiggy/bonsai` to a reviewed full commit SHA. Let the action select the
  reviewed base policy and its own checker; never override its verdict with
  `continue-on-error` or a candidate package script.
- Install application dependencies with scripts disabled. Use the committed Bun
  lockfile when available; this scaffold currently has none.
- Preserve Bonsai reports after rejected or incomplete checks. Missing handlers
  and contracts remain failures until real application behavior is connected.
- Keep general checks in CI. Do not restore Bonsai's copied root-report,
  package-verification, or demo steps there.

## Work Guidance

The initial scaffold binding imports nonexistent `features/support/actions.ts`.
Connect actual application handlers and executable contracts before claiming a
passing Bonsai baseline; do not copy Bonsai's own contracts or relax policy.

A failing workflow does not by itself block merging. Follow the pinned action's
[protected-workflow guidance](https://github.com/jiggy/bonsai/blob/ab35db95df1ded9b562c0062d2cafb8f4ac4e6bd/docs/github-action.md#add-it-to-a-repository)
to enforce a trusted workflow definition. A required job name alone can be
replaced by a candidate no-op. Where independent required workflows are
unavailable, enforced trusted review of workflow changes is a weaker alternative.
Repository rules and code-owner enforcement are separate setup, not configured
by these files.

## Verification

- `CI` runs formatting checks, `bun run check`, and `bun run test`.
- `Bonsai allowance` invokes the pinned action and uploads its JSON report.
- Only a complete passing action result establishes acceptance. Existing
  scaffold setup failures remain visible.

## Child DOX Index

None.
