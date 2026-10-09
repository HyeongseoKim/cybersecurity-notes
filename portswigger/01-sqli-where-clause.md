# PortSwigger — SQL injection in WHERE clause allowing retrieval of hidden data

- **Lab:** SQL injection vulnerability in WHERE clause allowing retrieval of hidden data (Apprentice)
- **Date:** 2026-10-08
- **Note:** My first PortSwigger lab, and my first time changing what a server's own
  query does — not just finding or editing a value, but rewriting the command.

<!-- I do not post flag values. PortSwigger labs have no flag; I describe the input I used
     and why it worked. -->

## Goal

Find the hidden products that are not released for sale yet, and show them all.

## What I tried first

I already knew the first thing to try for SQL injection: add a single quote (`'`) to the
end of the input. So I chose one category and added `'` to the end of the URL, then sent
it. The page returned "Internal Server Error", so I was fairly sure SQL injection was
possible here.

## What the error told me

If the website were safe from SQL injection, it would just show "no results". Instead it
returned a server error. That meant my quote had actually broken the query, so my input
was being treated as part of the SQL, not as plain data.

## What worked

After I confirmed the injection was possible, I tried to bring the hidden products onto
the screen.

The first problem: I had used `'` three times in total (two added by the server, one by
me), so the server thought the string had not ended. I added `--` to comment out the
rest of the query, and the error went away — but the lab was still not solved, because I
was only seeing the hidden products inside one category.

Then I used `OR 1=1`. Because `1=1` is always true, the whole `WHERE` condition becomes
true for every row, so the category filter no longer matters. With `' OR 1=1 --` the page
showed every product, including the unreleased ones, and the lab was solved.

## New to me

| Term | What it means |
|---|---|
| SQL / query | A question the server asks the database. |
| SQL injection | The server puts my input into its query. If it is not protected, my input runs as part of the command, not as data. |
| `'` | Closes the string and lets me break out of the data part of the query. |
| `OR 1=1` | Always true, so it cancels the condition in front of it — every row matches. |
| `--` | Tells SQL to ignore everything after it, so the leftover part of the query is dropped. |

## One thing I still do not understand

`' OR 1=1 --` worked, but I am not sure yet how a site is supposed to stop this. I read
that the fix is to keep data and commands separate (parameterised queries) instead of
just checking the input, and I want to see how that actually looks in code.
