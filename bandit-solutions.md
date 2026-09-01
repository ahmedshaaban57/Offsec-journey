# Bandit — Solutions & Reminders (my own notes)

> A re-teaching log, not just a cheat-sheet. If I forget something months from now, reading this should make it click again.
> **Passwords are NOT stored here on purpose** — creds never go in a public repo (building the habit early). If I want them, they live in a private local note. Bandit passwords also reset periodically, so the *method* is what's worth keeping.

## What Bandit is
A Linux/security wargame. Each level hides a password; I use Linux commands to find it; that password logs me into the next level (`bandit0` → `bandit1` → …). The puzzles are simple — the point is drilling Linux + shell fundamentals used in real hacking.

## How I connect (the SSH pattern)
```
ssh banditN@bandit.labs.overthewire.org -p 2220
```
- `banditN` = the level's username
- `-p 2220` = use port **2220**, not SSH's default 22
- When I type the password, **nothing shows on screen** — that's normal, not a freeze.
- `Permission denied` = SSH works, but the **password was wrong** (often case-sensitivity: `O`≠`0`, `l`≠`1`, `S`≠`5`).

---

## Level-by-level (how I found each password)

### Level 0 → 1 — reading a file
- Challenge: password is in a file in the home directory.
- Did: `ls` (saw `readme`), then `cat readme`.
- Why it works: `ls` lists what's here; `cat` prints a file's contents.

### Level 1 → 2 — a file literally named `-`
- Challenge: the file is called `-`.
- Did: `cat ./-`
- Why: bare `-` means "read from standard input (the keyboard)" to many commands, so `cat -` just hangs waiting for me to type. Writing `./-` says "the file named `-` **in the current directory**", which removes the ambiguity.
- New idea: `./` = "the current directory". `./file` and `file` point to the same thing, but `./` forces something to be read as a *path*.

### Level 2 → 3 — a filename with spaces
- Challenge: file is `--spaces in this filename--`.
- Did: `cat './--spaces in this filename--'`  (or `cat -- './--spaces in this filename--'`)
- Why: the shell splits arguments on **spaces**, so without quotes `cat` sees four separate arguments. **Quotes** keep it as ONE argument. The leading `--` also risks being read as options — `./` (or a `--` terminator) fixes that.

### Level 3 → 4 — a hidden file
- Challenge: `inhere/` looks empty with `ls`.
- Did: `cd inhere`, then `ls -la` revealed `...Hiding-From-You`, then `cat './...Hiding-From-You'`.
- Why: files starting with `.` are **hidden** from plain `ls`; `-a` shows all, `-l` shows detail. Had to read the name **exactly** — `...` (three dots) ≠ `..` (parent directory).
- Pro habit: type `cat ./...` then press **TAB** — the shell autocompletes the exact filename.

### Level 4 → 5 — find the human-readable file (in progress)
- Challenge: `inhere/` has many files; only one is readable text.
- Tool: `file ./*`
- Why: `file` reports what *kind* of data each file holds (`ASCII text`, `PNG image`, `data`…). `./*` = "every file in the current directory". The one reported as text is the answer — then `cat` it.

---

## New commands & shell concepts I learned

| Thing | Means | Example |
|---|---|---|
| `pwd` | print working directory (where am I) | `pwd` |
| `ls` / `ls -la` | list files / list all incl. hidden, detailed | `ls -la` |
| `cd dir` / `cd` / `cd ..` | change dir / go home / go up one | `cd inhere` |
| `cat file` | print a file's contents | `cat readme` |
| `file x` | identify what kind of data x is | `file ./*` |
| `.` / `..` | current dir / parent dir | `cd ..` |
| `./` | force "in the current directory" (path) | `cat ./-` |
| `*` | wildcard = match many filenames | `file ./*` |
| `'...'` / `"..."` | quote = treat as ONE argument (spaces safe) | `cat 'a b.txt'` |
| `--` | "stop reading options; rest are filenames" | `cat -- -n` |
| stdin (`-`) | read from the keyboard, not a file | `cat -` hangs |

## Weird-filename toolkit (reusable pattern)
- Spaces → quote it: `cat 'my file'`
- Starts with `-` → path it or terminate options: `cat ./-name`  or  `cat -- -name`
- Both → `cat './--weird name--'`

## The 4 lessons I want to keep
1. **Read the terminal exactly** — one character matters (`...` ≠ `..`, `O` ≠ `0`).
2. **The shell interprets characters before the command runs** — `-`, `*`, spaces, quotes all have meaning.
3. **Commands parse arguments by convention** — `cat -` isn't "open file `-`"; it's "read stdin".
4. **Errors are clues, not failures** — `Permission denied` = auth failed (not connection); `No such file` = my path doesn't match a real name.

## Mental model
```
I type a command
   → the shell expands wildcards/quotes/paths and splits arguments
   → the command receives those arguments
   → Linux does the operation
   → output or error
```

---
*Next: finish Level 4 with `file ./*`, then continue. Keep logging method, never passwords.*
