# 🛡️ Claude Code Destructive Command Guard

A `pre-tool-use` hook that blocks dangerous bash commands in [Claude Code](https://docs.anthropic.com/claude-code/hooks).

## Installation (2 commands)

```bash
mkdir -p ~/.claude/hooks
cp pre-tool-use ~/.claude/hooks/ && chmod +x ~/.claude/hooks/pre-tool-use
```

That's it. Claude Code picks it up automatically on the next tool call.

## What It Blocks

| Pattern | Example | Why |
|---|---|---|
| `rm -rf /` | `rm -rf /var` | Destructive recursive delete |
| `DROP TABLE` | `DROP TABLE users` | Destructive DDL |
| `TRUNCATE` | `TRUNCATE orders` | Destructive DDL |
| `git push --force` | `git push origin main -f` | Rewrites history |
| `DELETE FROM` (no WHERE) | `DELETE FROM users` | Mass data loss |
| `UPDATE` (no WHERE) | `UPDATE users SET role = 'admin'` | Mass corruption |
| `mkfs.*` | `mkfs.ext4 /dev/sda` | Filesystem destruction |
| `dd` to block device | `dd if=/dev/zero of=/dev/sda` | Raw disk wipe |

## Logs

Every blocked command is logged to `~/.claude/hooks/blocked.log`:

```
[2026-06-18T14:30:00Z] BLOCKED | pattern=rm-rf-recursive | command=rm -rf /var/log | project=/home/user/my-project
```

## Does it interfere?

No. Only the patterns above are blocked. Normal commands like `ls`, `cat`, `git commit`, `npm install`, `curl`, etc. pass through without any overhead.

## Customizing

Edit the `PATTERNS` array in `pre-tool-use` to add or remove patterns.

---

_Claude Builders Bounty #3 · $100_
