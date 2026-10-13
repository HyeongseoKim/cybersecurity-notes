# PortSwigger — SQL injection UNION attack, retrieving data from other tables

- **Lab:** SQL injection UNION attack, retrieving data from other tables (Apprentice)
- **Date:** 2026-10-13
- **Note:** The first lab where I actually read real data out of the database — the admin's
  password — instead of only bypassing a check. This is the answer to the question I had
  two labs ago: "how do I find the admin's name and password?"

<!-- No password values here. I only name the account (administrator). -->

## Goal

Read the `users` table (its `username` and `password` columns) and log in as the
administrator.

## What I did — three steps

**1. Count the columns.**
I first tried `UNION SELECT NULL`, but the site kept returning an error. The reason was my
mistake: I forgot the `UNION` in front of `SELECT`, so it was never a valid UNION. After I
fixed that, `' UNION SELECT NULL,NULL --` stopped giving an error, so there are **2
columns**. (`' ORDER BY 3 --` also errored, which agrees: the count is 2.)

**2. Find which columns accept text.**
Username and password are text, so I needed text columns to hold them. I replaced the
NULLs with letters: `' UNION SELECT 'a','a' --`. The page showed `a` with no error, so
**both columns accept text**.

**3. Pull the real data.**
I put the real column names in place of the letters:

```
' UNION SELECT username, password FROM users --
```

The `users` table came out under the normal product list, including the administrator's
row. I copied the administrator's password, logged in as `administrator`, and the lab was
solved.

## New to me

| Term | What it means |
|---|---|
| Table | A whole set of rows in the database (e.g. `products`, `users`). |
| Column | One field within a table (e.g. `username`, `password`). |
| `UNION SELECT col1, col2 FROM table` | Joins a second query onto the first, so another table's data comes out with the normal results. |
| Checking column types | Before pulling text, test each column with `'a'` — a text column shows the letter, a non-text one errors. |

## What tripped me up

I kept getting errors on `SELECT NULL` and could not see why. The cause was that I had
left out `UNION`. `ORDER BY` works on its own, but `SELECT NULL` only makes sense when it
is joined with `UNION` — without it, the two queries are not connected and it is just a
syntax error. (This also cleared up my earlier confusion about why it is called a "UNION
attack".)

## One thing I still wonder

This lab told me the table is `users` and its columns are `username` and `password`. In a
real case, nobody tells me those names. I read that you can ask the database about its own
structure using `information_schema` — list the tables first, then the columns of the
table you want — and I think that is the "Examining the database" topic, which I will do
next.
