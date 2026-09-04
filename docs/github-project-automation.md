# GitHub Project automation

This repository contains two workflows for the Out of Office backlog.

## What is already prepared

- Issues 1 to 39 are the backlog tickets imported from Notion.
- Issues 40 to 48 are the epics.
- `setup-epic-sub-issues.yml` converts the epic checklists into native GitHub sub-issue relationships.
- `sync-epic-statuses.yml` adds the 48 issues to Project 8 and synchronizes statuses.

## Status rules

The synchronization workflow applies these rules:

1. When an epic is moved to `Ready`, children in `Todo`, `Backlog`, `Pas commencé` or `Not started` move to `Ready`.
2. When at least one child is `In Progress` or `Done`, the epic moves to `In Progress`.
3. When every child is `Done` or closed, the epic moves to `Done`.
4. Completed children are never moved backwards.

The workflow runs every five minutes after activation. It can also be launched manually.

## One-time setup

### 1. Check the project statuses

Open [Project 8](https://github.com/users/PerrineLV/projects/8/views/1) and check that its `Status` field contains exactly:

- `Ready`
- `In Progress`
- `Done`

The project may also contain `Todo` or `Backlog`.

If your names differ, edit these values in `.github/workflows/sync-epic-statuses.yml`:

```yaml
READY_STATUS: Ready
IN_PROGRESS_STATUS: In Progress
DONE_STATUS: Done
```

### 2. Create a token for the personal project

The normal repository `GITHUB_TOKEN` cannot edit a personal GitHub Project.

1. Open GitHub Settings.
2. Open Developer settings.
3. Create a personal access token.
4. Give it read and write access to Projects.
5. Give it access to the `PerrineLV/OutOfOffice` repository.
6. Copy the token once.

Use the smallest possible permissions. Never commit this token to the repository.

### 3. Save the token as a repository secret

1. Open the repository settings.
2. Open Secrets and variables, then Actions.
3. Create a repository secret named `PROJECT_TOKEN`.
4. Paste the token as its value.

### 4. Create the native parent-child relationships

1. Open the Actions tab.
2. Select `Setup epic sub-issues`.
3. Select `Run workflow`.
4. Wait for the workflow to finish successfully.

The workflow is idempotent. It can be launched again if a previous run was interrupted.

### 5. Test the status synchronization

1. Open the Actions tab.
2. Select `Sync epic statuses`.
3. Select `Run workflow`.
4. Confirm that issues 1 to 48 appear in Project 8.
5. Move one epic to `Ready`.
6. Launch the workflow again.
7. Confirm that its unstarted children move to `Ready`.
8. Move one child to `In Progress`, launch the workflow, and confirm that the epic follows.
9. Move every child to `Done`, launch the workflow, and confirm that the epic moves to `Done`.

### 6. Enable the schedule

After the manual test succeeds:

1. Return to Secrets and variables, then Actions.
2. Open the Variables tab.
3. Create a repository variable named `EPIC_SYNC_ENABLED`.
4. Set its value to `true`.

The scheduled synchronization is skipped until this variable exists.

## Troubleshooting

- `The PROJECT_TOKEN repository secret is missing`: create the secret described above.
- `Project 8 or its Status field could not be found`: verify the token permissions and the project number.
- `The Status field must contain Ready, In Progress and Done options`: rename the project options or update the workflow variables.
- An issue is missing from the project: launch the synchronization workflow again.
- A child is not linked to its epic: launch the sub-issue setup workflow again.
