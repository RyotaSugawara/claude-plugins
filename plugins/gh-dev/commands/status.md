---
description: Show current development progress and next tasks
---

# Development Status

現在の開発進捗を表示します。

## Milestones

!`gh api repos/:owner/:repo/milestones --jq '.[] | "### \(.title) (\(.open_issues) open / \(.closed_issues) closed)\n\(.description // "No description")\nDue: \(.due_on // "No due date")\n"'`

## Project Items Status

!`[ -n "$PROJECT_NUMBER" ] && gh project item-list "$PROJECT_NUMBER" --owner "${PROJECT_OWNER:-@me}" --limit 200 --format json | jq -r '.items | group_by(.status) | map({status: .[0].status, count: length}) | .[] | "\(.status): \(.count)"' || echo "PROJECT_NUMBER not set, skipping project overview"`

## Next Tasks (Todo Items)

!`[ -n "$PROJECT_NUMBER" ] && gh project item-list "$PROJECT_NUMBER" --owner "${PROJECT_OWNER:-@me}" --limit 200 --format json | jq -r '.items[] | select(.status == "Todo") | "#\(.content.number) - \(.content.title)"' | head -20 || gh issue list --state open --limit 20`

## Recently Completed (Done Items)

!`[ -n "$PROJECT_NUMBER" ] && gh project item-list "$PROJECT_NUMBER" --owner "${PROJECT_OWNER:-@me}" --limit 200 --format json | jq -r '.items[] | select(.status == "Done") | "#\(.content.number) - \(.content.title)"' | head -10 || echo "PROJECT_NUMBER not set, skipping completed items"`

## Development Rules

- GitHub Project is the single source of truth for all tasks
- GitHub Milestones group tasks into deliverable units
- Prioritize Issues in the **earliest open Milestone** first
- Issues without a Milestone are lower priority
- One branch per Issue, one PR per Issue
- PR body must contain `Closes #<issue>` for auto-close
- Follow CLAUDE.md for all code style and project conventions

## Your Task

上記の情報を元に、以下をサマリーとして報告:

1. **Milestone進捗**: 各Milestoneのopen/closed比率を表示
2. **現在のMilestone**: 最も早い未完了Milestoneを特定
3. **進捗率**: Done/Total の割合を計算して表示
4. **次のタスク**: 現在のMilestoneのTodoアイテムを優先して提示
5. **最近の成果**: 直近で完了したタスクをハイライト
