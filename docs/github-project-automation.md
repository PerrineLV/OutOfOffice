# GitHub Project and Gitflow automation

This repository automates the Out of Office backlog and its lightweight Gitflow.

## Branch flow

```text
ticket/<ticket>-<slug> -> epic/<epic>-<slug> -> dev -> main
```

- Each epic has its own `epic/<issue>-<slug>` branch, created from `dev`.
- Each ticket has its own `ticket/<issue>-<slug>` branch, created from its epic branch.
- `chore/<slug>` branches may target `dev` for repository maintenance.
- Only `dev` may target `main`.

## Automated statuses

| Event | Project status |
| --- | --- |
| Create a ticket branch with the workflow | Ticket -> `In Progress` |
| Open or reopen a ticket PR toward its epic | Ticket -> `In Review` |
| Merge a ticket PR into its epic | Ticket -> `Done` |
| Close a ticket PR without merging | Ticket -> `In Progress` |
| Open or reopen an epic PR toward `dev` | Epic -> `In Review` |
| Merge an epic PR into `dev` | Epic -> `Done` |
| Close an epic PR without merging | Epic -> `In Progress` |

The periodic epic synchronization still applies these complementary rules:

1. Moving an epic to `Ready` moves its unstarted children to `Ready`.
2. Starting a child moves the epic to `In Progress`.
3. An epic in `In Review` or `Done` is never moved backwards by the periodic synchronization.
4. Completing all tickets does not mark the epic `Done`; only merging its PR into `dev` does.

## Required Project statuses

Project 8 must contain these exact Status options:

- `Ready`
- `In Progress`
- `In Review`
- `Done`

## Repository secret and variable

The workflows use the existing repository secret `PROJECT_TOKEN`. It needs read and write access to the personal Project and access to `PerrineLV/OutOfOffice`.

The scheduled epic synchronization runs when the repository variable `EPIC_SYNC_ENABLED` is set to `true`. It can always be launched manually.

## Create a ticket branch

1. Open the repository Actions tab.
2. Select `Create ticket branch`.
3. Select `Run workflow`.
4. Enter the ticket issue number, without `#`.

The workflow finds the ticket in its epic checklist, finds the matching epic branch, creates the ticket branch from it, and moves the ticket to `In Progress`.

To work locally afterward:

```bash
git fetch origin
git switch ticket/12-example-slug
```

## Pull requests

The `Validate Gitflow` check verifies:

- a ticket PR targets the correct epic branch;
- the ticket is listed as a child of that epic;
- an epic PR targets `dev`;
- only `dev` targets `main`;
- branch names follow the documented convention.

The status synchronization is a separate check. This keeps a temporary Project API problem from weakening the structural Gitflow validation.

## Recommended rulesets

Create the following rulesets in `Settings > Rules > Rulesets`.

### Protect main

Target: default branch.

- Restrict deletions
- Block force pushes
- Require a pull request before merging
- Required approvals: 0
- Require conversation resolution
- Require status check: `Validate Gitflow`

### Protect dev

Target: branch name `dev`.

- Restrict deletions
- Block force pushes
- Require a pull request before merging
- Required approvals: 0
- Require conversation resolution
- Require status check: `Validate Gitflow`

### Protect epic branches

Target: branch name pattern `epic/*`.

- Block force pushes
- Require a pull request before merging
- Required approvals: 0
- Require conversation resolution
- Require status check: `Validate Gitflow`

Do not restrict deletion for `epic/*`, because an epic branch should be deleted after it is merged into `dev`.

In `Settings > General > Pull Requests`, enable automatic deletion of head branches. This deletes ticket branches after their merge into an epic and epic branches after their merge into `dev`.

## Initial epic branches

- `epic/40-fondamentaux-node`
- `epic/41-exclusion-hmac`
- `epic/42-socle-applicatif`
- `epic/43-decouverte-filtree`
- `epic/44-like-match`
- `epic/45-chat-temps-reel`
- `epic/46-blocage`
- `epic/47-livraison`
- `epic/48-v2`

## Troubleshooting

- `Add a 'In Review' option`: add the missing status to Project 8.
- `PROJECT_TOKEN repository secret is missing`: recreate or restore the secret.
- `Issue is not a child of epic`: check the native sub-issue relationship and the epic checklist.
- `No epic branch exists`: create the documented `epic/<issue>-<slug>` branch from the current `dev` branch.
