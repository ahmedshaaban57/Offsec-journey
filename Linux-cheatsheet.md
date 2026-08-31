# Linux Cheat-Sheet — Navigation & Filesystem

## Core navigation commands

| Command | What it does | Example | Why it matters (offensive) |
|---|---|---|---|
| `pwd` | Print current directory | `pwd` → `/home/ahmed` | Know where you landed after a shell pops |
| `ls` | List files here | `ls` | See what's in a directory |
| `ls -la` | List all files (incl. hidden) + details | `ls -la /home/ahmed` | Hidden `.` files hold configs, keys, creds |
| `cd <dir>` | Change directory | `cd /etc` | Move around the target's filesystem |
| `cd` | Go to home directory | `cd` | Quick reset when lost |
| `cd ..` | Go up one level | `cd ..` | Climb back toward `/` |
| `cat <file>` | Print a file's contents | `cat /etc/passwd` | Read configs, creds, flags — can't `cd` a file! |

## Key directories (the tree)

| Path | Holds | Why a hacker looks here |
|---|---|---|
| `/` | Root of everything | The top of the tree |
| `/home` | Normal users' files | User data, SSH keys, notes |
| `/etc` | System config files | `/etc/passwd` (accounts), service configs |
| `/var` | Variable data | `/var/log` (logs), `/var/www` (web files) |
| `/root` | The admin's home | The prize — reachable only as root |

## Remember
- `cd` moves between **folders**; `cat` reads **files**. You cannot `cd` into a file.
- Accounts live in the **file** `/etc/passwd`, not a folder.
