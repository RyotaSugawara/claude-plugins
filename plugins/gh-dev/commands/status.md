---
description: Show current development progress and next tasks
---

# Development Status

Display current development progress.

## Milestones

!`gh api repos/:owner/:repo/milestones --jq '.[] | "### \(.title) (\(.open_issues) open / \(.closed_issues) closed)\n\(.description // "No description")\nDue: \(.due_on // "No due date")\n"'`

## Project Items Status

!`if [ -n "$PROJECT_NUMBER" ]; then gh project item-list "$PROJECT_NUMBER" --owner "${PROJECT_OWNER:-@me}" --limit 200 --format json | jq -r '.items | group_by(.status) | map({status: .[0].status, count: length}) | .[] | "\(.status): \(.count)"'; else echo "PROJECT_NUMBER not set, skipping project overview"; fi`

## Next Tasks (Todo Items)

!`if [ -n "$PROJECT_NUMBER" ]; then gh project item-list "$PROJECT_NUMBER" --owner "${PROJECT_OWNER:-@me}" --limit 200 --format json | jq -r '.items[] | select(.status == "Todo") | "#\(.content.number) - \(.content.title)"' | head -20; else gh issue list --state open --limit 20; fi`

## Recently Completed (Done Items)

!`if [ -n "$PROJECT_NUMBER" ]; then gh project item-list "$PROJECT_NUMBER" --owner "${PROJECT_OWNER:-@me}" --limit 200 --format json | jq -r '.items[] | select(.status == "Done") | "#\(.content.number) - \(.content.title)"' | head -10; else echo "PROJECT_NUMBER not set, skipping completed items"; fi`

## Development Rules

- GitHub Project is the single source of truth for all tasks
- GitHub Milestones group tasks into deliverable units
- Prioritize Issues in the **earliest open Milestone** first
- Issues without a Milestone are lower priority
- One branch per Issue, one PR per Issue
- PR body must contain `Closes #<issue>` for auto-close
- Follow CLAUDE.md for all code style and project conventions

## Your Task

Based on the information above, report the following summary:

1. **Milestone progress**: Display the open/closed ratio for each Milestone
2. **Current Milestone**: Identify the earliest incomplete Milestone
3. **Progress rate**: Calculate and display the Done/Total ratio
4. **Next tasks**: Prioritize and present Todo items from the current Milestone
5. **Recent achievements**: Highlight recently completed tasks
