# gh-dev

GitHub Project & Milestone based development workflow commands for Claude Code.

## Commands

| Command | Description |
|---------|-------------|
| `/gh-dev:resume [issue]` | Resume development from where you left off |
| `/gh-dev:status` | Show milestone progress and next tasks |
| `/gh-dev:complete <issue>` | Create PR to complete a task and close the Issue |

## Environment Variables

| Variable | Required | Default | Description |
|----------|----------|---------|-------------|
| `PROJECT_NUMBER` | No | - | GitHub Project number (`gh project list --owner @me`) |
| `PROJECT_OWNER` | No | `@me` | GitHub Project owner (user or org name) |

Without `PROJECT_NUMBER`, commands fall back to `gh issue list`.

## Prerequisites

- [GitHub CLI (`gh`)](https://cli.github.com/) authenticated
- `jq` installed
