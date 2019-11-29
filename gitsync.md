# Git Integration

Universal Automation can automatically synchronize with a git repository of your choice. The git synchronization works via standard git commands. Every minute the git sync runs a job that pulls from the remote repository and updates the local database cache with scripts, variables, schedules, and other settings within Universal Automation.

## Pulling Changes

UA automatically pulls from the master branch to local. It synchronizes the local files with the local database. Any mistakes within the files that will cause a syntax error will be ignored. Configuration changes are stored as simple PowerShell scripts.

## Pushing Changes

When changes are made through the UA REST API or PowerShell cmdlets, the changes will be automatically written to disk, committed to git and pushed to a remote, if configured. Even if a remote isn't configured, changes will be written to disk and committed to git.

## Merge Conflicts

Universal Automation does not yet deal with merge conflicts.
