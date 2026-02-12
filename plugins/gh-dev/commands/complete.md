---
description: Create PR to complete a task and close the linked Issue
argument-hint: <issue-number>
---

# Complete Task

Complete the task, create a PR, and auto-close the linked Issue.

## Arguments

- `$ARGUMENTS`: Issue number (e.g., 35, 36)

## Current Open Issues

!`gh issue list --state open`

## Your Task

1. **Commit changes:**
   - Commit any uncommitted changes
   - Use conventional commits format: `feat:`, `fix:`, etc.

2. **Create a PR:**
   ```bash
   gh pr create --title "feat: description" --body "Closes #$ARGUMENTS"
   ```

3. **Verify:**
   - Report the PR URL
   - Confirm the Issue is linked

## Notes

- The Issue will auto-close when the PR is merged
- The GitHub Project will also update automatically
- Make sure CI passes before merging
