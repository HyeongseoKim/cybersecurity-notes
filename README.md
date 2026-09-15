# Cybersecurity Learning Notes

Self-directed cybersecurity practice. My high school does not offer computer science
or computer engineering courses, so I work through wargames, build small tools, and
document what I learn here.

Grade 12 international student in Ontario, Canada.

---

## What's in this repository

| Folder | Contents |
|---|---|
| `bandit/` | Level-by-level notes from OverTheWire Bandit — techniques and mistakes, no passwords |
| `notes/` | Linux and shell notes written in my own words |

> **On spoilers:** I do not post passwords or flag values. OverTheWire asks players not to
> publish solutions, so these notes describe *how* I approached a level and what I learned,
> not the answer.

---

## Progress log

| Date | What I did | What was new to me |
|---|---|---|
| 2026-09-14 | Solved and wrote up Bandit 19→20 | The setuid (`s`) bit — a program running as its owner instead of the user, and why that matters for privilege escalation |
| 2026-09-14 | Solved and wrote up Bandit 18→19 | Running a single command over SSH without opening a shell, and spotting a modified file from its owner and timestamp |
| 2026-09-11 | Solved and wrote up Bandit 17→18 | `man -k` for finding a command by keyword, and how `diff` marks changed, added, and deleted lines |
| 2026-09-10 | Wrote up Bandit 16→17 and set up an SSH key for GitHub | `nmap -sV`, self-signed certificates, and that `r` and `x` mean different things on a directory |
| 2026-09-09 | Restructured the repository: moved notes into `notes/`, rewrote the README | GitHub moves a file when you edit its path in the filename field |

---

## Wargames

**OverTheWire — Bandit:** Level 20 of 34

**picoCTF:** not started yet

---

## Notes

- `notes/linux-basics.md` — Basic Linux commands and what each one actually does

---

## Currently working on

- Working through OverTheWire Bandit (currently level 20->21), writing up each level as I solve it
