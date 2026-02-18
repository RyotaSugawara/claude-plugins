---
description: Resume development from where we left off
argument-hint: [issue-number (optional)]
---

# Resume Development

Resume development from where you left off.

## Current Context

**Milestones:**
!`gh api repos/:owner/:repo/milestones --jq '.[] | "\(.title): \(.open_issues) open / \(.closed_issues) closed"'`

**Project Progress:**
!`if [ -n "$PROJECT_NUMBER" ]; then gh project item-list "$PROJECT_NUMBER" --owner "${PROJECT_OWNER:-@me}" --limit 200 --format json | jq -r '.items | group_by(.status) | map({status: .[0].status, count: length}) | .[] | "\(.status): \(.count) items"'; else echo "PROJECT_NUMBER not set, skipping project overview"; fi`

**Next Available Tasks (Todo):**
!`if [ -n "$PROJECT_NUMBER" ]; then gh project item-list "$PROJECT_NUMBER" --owner "${PROJECT_OWNER:-@me}" --limit 200 --format json | jq -r '.items[] | select(.status == "Todo") | "#\(.content.number) - \(.content.title)"' | head -30; else gh issue list --state open --limit 30; fi`

**Recently Completed:**
!`if [ -n "$PROJECT_NUMBER" ]; then gh project item-list "$PROJECT_NUMBER" --owner "${PROJECT_OWNER:-@me}" --limit 200 --format json | jq -r '.items[] | select(.status == "Done") | "#\(.content.number) - \(.content.title)"' | head -5; else echo "PROJECT_NUMBER not set, skipping completed items"; fi`

**Current Git Status:**
!`git status --short`

## Development Rules

- GitHub Project is the single source of truth for all tasks
- GitHub Milestones group tasks into deliverable units
- Prioritize Issues in the **earliest open Milestone** first
- Issues without a Milestone are lower priority
- One branch per Issue, one PR per Issue
- PR body must contain `Closes #<issue>` for auto-close
- Follow CLAUDE.md for all code style and project conventions

## Your Task

1. **Assess the situation:**
   - Check Milestone progress and identify the current Milestone
   - Prioritize Todo tasks belonging to the current Milestone
   - Identify the next task to work on

2. **Process arguments:**
   - If an Issue number is specified (e.g., `/gh-dev:resume 69`), prioritize that Issue
   - If not specified, select from Todo items in the current Milestone

3. **Start implementation:**
   - Review the selected task details: `gh issue view <number>`
   - Create a corresponding branch: `git checkout -b feat/<issue>-<description>`
   - Implement following CLAUDE.md conventions
   - Include tests

4. **Wrap up:**
   - Run checks following the "Pre-Push Checklist" in CLAUDE.md
   - Create a PR with auto-close:
     ```bash
     gh pr create --body "Closes #<issue>"
     ```

## Implementation Guidelines

- Follow all rules in CLAUDE.md
- Commit in small units (one feature per commit)
- Create a PR for each completed task
