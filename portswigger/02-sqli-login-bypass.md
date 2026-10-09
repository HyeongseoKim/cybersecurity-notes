# PortSwigger — SQL injection allowing login bypass

- **Lab:** SQL injection vulnerability allowing login bypass (Apprentice)
- **Date:** 2026-10-09
- **Note:** Same payload as the last lab (`' OR 1=1 --`), but used on a login form
  instead of a product filter. Same tool, different target.

<!-- I do not post flag values. PortSwigger labs have no flag; I describe the input I
     used and why it worked. -->

## Goal

Log in as the administrator without knowing the username or the password.

## What worked

I did not know the username or the password, but I already knew how to attack a site
with a SQL injection vulnerability. So I first added a single quote `'` after the
username. The message "Internal Server Error" appeared, so I knew the login form was
vulnerable.

After that I used `' OR 1=1 --` as the username. The page then showed that I was logged
in as `administrator`, and the lab was solved.

## Why it worked

I want to break down `' OR 1=1 --` piece by piece, because at first I thought it was one
single command. It is not — it is four parts:

- `'` — closes the username string early, so what comes after is read as SQL, not data.
- `OR` — just "or". The row passes if the left side OR the right side is true.
- `1=1` — always true.
- `--` — makes the server ignore everything after it.

So the login query becomes roughly:

```sql
SELECT * FROM users WHERE username = '' OR 1=1 -- ' AND password = '...'
```

Because `1=1` is always true, every row passes the `WHERE` condition — the username
check no longer matters. The password check is gone because `--` comments it out. The
server then logs in the first matching row, which was the administrator.

## New to me

The difference between this lab and the last one is the **purpose**, not the method. Last
lab, the same injection revealed hidden products. This lab, it let me log in without a
username or password. One technique, two very different uses.

I also fixed a wrong idea I had: I used to think `OR 1=1` was a single command. Now I see
`OR` and `1=1` are separate parts doing separate jobs.

## One thing I still wonder

`' OR 1=1 --` worked because it lets every row pass, so the first user (the
administrator) was logged in. But the admin is not always named "administrator". If I
did not know the name, I would first need to find the usernames in the database, then log
in as that specific user with something like `administrator'--`. I think that is what the
UNION attack topic is for.
