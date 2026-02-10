---
description: Resume development from where we left off
argument-hint: [issue-number (optional)]
---

# Resume Development

開発を再開します。

## Current Context

**Milestones:**
!`gh api repos/:owner/:repo/milestones --jq '.[] | "\(.title): \(.open_issues) open / \(.closed_issues) closed"'`

**Project Progress:**
!`[ -n "$PROJECT_NUMBER" ] && gh project item-list "$PROJECT_NUMBER" --owner "${PROJECT_OWNER:-@me}" --limit 200 --format json | jq -r '.items | group_by(.status) | map({status: .[0].status, count: length}) | .[] | "\(.status): \(.count) items"' || echo "PROJECT_NUMBER not set, skipping project overview"`

**Next Available Tasks (Todo):**
!`[ -n "$PROJECT_NUMBER" ] && gh project item-list "$PROJECT_NUMBER" --owner "${PROJECT_OWNER:-@me}" --limit 200 --format json | jq -r '.items[] | select(.status == "Todo") | "#\(.content.number) - \(.content.title)"' | head -30 || gh issue list --state open --limit 30`

**Recently Completed:**
!`[ -n "$PROJECT_NUMBER" ] && gh project item-list "$PROJECT_NUMBER" --owner "${PROJECT_OWNER:-@me}" --limit 200 --format json | jq -r '.items[] | select(.status == "Done") | "#\(.content.number) - \(.content.title)"' | head -5 || echo "PROJECT_NUMBER not set, skipping completed items"`

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

1. **状況把握:**
   - Milestone進捗を確認し、現在のMilestoneを特定
   - 現在のMilestoneに属するTodoタスクを優先
   - 次に実行すべきタスクを特定

2. **引数処理:**
   - Issue番号が指定された場合（例: `/gh-dev:resume 69`）、そのIssueを優先
   - 指定がない場合は、現在のMilestoneのTodoから選択

3. **実装開始:**
   - 選択したタスクの詳細を確認: `gh issue view <number>`
   - 対応するブランチを作成: `git checkout -b feat/<issue>-<description>`
   - CLAUDE.mdの規約に従って実装
   - テストを含める

4. **完了処理:**
   - CLAUDE.mdの「Pre-Push Checklist」に従ってチェック実行
   - PRを作成して自動クローズ:
     ```bash
     gh pr create --body "Closes #<issue>"
     ```

## Implementation Guidelines

- CLAUDE.mdのルールを遵守すること
- 小さな単位でコミット（1機能1コミット）
- PRは1タスク完了ごとに作成
