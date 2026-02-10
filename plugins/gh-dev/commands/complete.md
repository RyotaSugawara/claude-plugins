---
description: Create PR to complete a task and close the linked Issue
argument-hint: <issue-number>
---

# Complete Task

タスクを完了し、PRを作成して Issue を自動クローズします。

## Arguments

- `$ARGUMENTS`: Issue 番号 (例: 35, 36)

## Current Open Issues

!`gh issue list --state open`

## Your Task

1. **変更をコミット:**
   - 未コミットの変更があればコミット
   - conventional commits 形式: `feat:`, `fix:`, etc.

2. **PRを作成:**
   ```bash
   gh pr create --title "feat: description" --body "Closes #$ARGUMENTS"
   ```

3. **確認:**
   - PR の URL を報告
   - Issue がリンクされていることを確認

## Notes

- PR マージ時に Issue は自動クローズされます
- GitHub Project も自動更新されます
- マージ前に CI が通ることを確認してください
