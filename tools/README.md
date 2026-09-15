# tools/bandit.sh — a small connect script for Bandit

**Date:** 2026-09-15

## The problem

Every time I connect to a Bandit level, I type the same long line and only change
one number:

```bash
ssh bandit18@bandit.labs.overthewire.org -p 2220
```

Changing the number each time, and having to remember (or look up) the full
address and port, was the part that annoyed me most. I wanted to type only the
level number.

## What it does

```bash
./bandit.sh 18
```

That connects me to level 18. The script fills in the address and port, so I only
give the number.

```bash
#!/bin/bash
ssh bandit${1}@bandit.labs.overthewire.org -p 2220
```

- `#!/bin/bash` (the shebang) tells the system to run the file with bash.
- `$1` is the first argument I pass, so `./bandit.sh 18` puts `18` into `$1`.
- I wrote it as `${1}` with curly braces. The braces mark where the variable name
  ends, so bash reads only `1` and does not try to read `1@bandit...` as one long
  name. It happens to work without braces here, but `${1}` is the safe habit.

## How I built it

1. Wrote the two lines into `tools/bandit.sh`.
2. Made it executable with `chmod` (the `x` permission — the same idea I saw in
   the setuid level).
3. Tested it with `./tools/bandit.sh 20` and a lower level number to check that
   the number really gets substituted.

## What I got stuck on

Three things tripped me up.

First, I wrote the variable as just `$1`. I later used `${1}` with curly braces,
because the braces mark where the variable name ends, so bash reads only `1` and
not the text right after it.

Second, after I made the script I tried to look at it with `cat`. Nothing
happened, because `cat` only reads the file — it does not run it. The `${1}` stays
as plain text until the script is actually run.

Third, when I ran it with `./bandit.sh`, I got `permission denied`. A new script
is not executable by default. I had to add the execute permission with
`chmod +x`, and after that `ls -l` showed the `x` bit and the script ran.

## One thing I chose not to do

I did not put the password in the script. Hardcoding a password would let me skip
typing it, but anyone who read the file would get the password. That is a bad
security habit, so the script only builds the connection, and I type the password
myself.

## What I would add later

## What I would add later

Right now, if I forget the number and just run `./bandit.sh`, it tries to connect
as `bandit@...`, which is wrong. I would like to add a check that prints a short
usage message when no number is given, so the script tells me what it expects
instead of failing in a confusing way.