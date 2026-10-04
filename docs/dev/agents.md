# Running agents on cli-tasks

Status 2026-10-04: the guards are copied from tg-cli's
[`docs/dev/agents.md`](https://github.com/leemour/tg-cli/blob/main/docs/dev/agents.md), where each was
measured. Here the two hooks were run by hand, and the checks in [TESTING.md](TESTING.md) passed inside
the sandbox once these settings took effect; `bin/check-agents` has not been run yet.

## What an agent may do, and what stops it

| | How |
|---|---|
| **File edits only in this project, cli-messaging and cli-core** | [`.claude/hooks/writes-stay-inside.sh`](../../.claude/hooks/writes-stay-inside.sh) on Edit, Write and NotebookEdit, in every mode. Allowed: this checkout, `cli-tasks-*` and `cli-messaging-*` worktrees beside it, cli-messaging, cli-core, the session's scratch folders and this project's memory. It refuses the guards themselves — `.claude/settings*.json` and `.claude/hooks/` in any checkout |
| **Shell commands in the same folders** | the Bash sandbox in [`.claude/settings.json`](../../.claude/settings.json): writes only there, to the pnpm and npm caches and `/tmp`. Local sockets are open, so the keyring over D-Bus works — `gh` needs it |
| **No way out of the sandbox** | [`.claude/hooks/sandbox-stays-on.sh`](../../.claude/hooks/sandbox-stays-on.sh) refuses any Bash call that asks for `dangerouslyDisableSandbox` |
| **Git** | over SSH: `~/.ssh`, `~/.gnupg`, `~/.aws`, `~/.npmrc` and `~/.pypirc` are unreadable; only `~/.ssh/id_ed25519.pub` and `known_hosts` are re-opened, and the SSH agent's socket signs and pushes. The sandbox masks `.git/config`, so **push without `-u`** and **branch with `--no-track`** |
| **Refused, in every mode** | `sudo` |

## Checking the guards

From a terminal, never from inside a Claude session:

```sh
bin/check-agents                 # a headless session in bypass mode tries each item; each says what must happen
bin/trust-folder <worktree>...   # a new worktree's .claude/settings.json is ignored until the folder is trusted
```
