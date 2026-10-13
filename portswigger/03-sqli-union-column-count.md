# PortSwigger — SQL injection UNION attack, determining the number of columns

- **Lab:** SQL injection UNION attack, determining the number of columns returned by the query (Apprentice)
- **Date:** 2026-10-11
- **Note:** First UNION lab. The goal is not to steal data yet — it is to find out how many
  columns the original query returns, which I need before any UNION attack.

<!-- No flag values. I describe the inputs I used. -->

## Goal

Find how many columns the original query returns.

## Why this step matters

A UNION attack joins my own SELECT onto the original query, and UNION only works if both
sides have the **same number of columns**. But I cannot see the server's query, so I do
not know how many columns it has. So the first job is always to count them.

## What I tried — two ways

I learned two ways to find the column count. They aim at the same answer but work in
opposite directions.

**1. `' ORDER BY <number> --`**
`ORDER BY 1` means "sort by the 1st column", `ORDER BY 2` the 2nd, and so on. I raise the
number one at a time. It works until the number is bigger than the real column count, and
then the server returns an error. So the column count is the **last number before the
error** — i.e. the number that caused the error, minus one.

```
' ORDER BY 3 --   → OK
' ORDER BY 4 --   → error   → there are 3 columns
```

**2. `' UNION SELECT NULL,NULL,... --`**
Here I add one more `NULL` each time. `NULL` fits any data type, so I only test the count,
not the types. It fails while the count is wrong and the error **disappears** the moment
the count is right.

```
' UNION SELECT NULL,NULL --        → error  (2 is wrong)
' UNION SELECT NULL,NULL,NULL --   → OK     (3 is right)
```

So the two methods are opposite: `ORDER BY` goes up until the error **starts**, `NULL`
goes up until the error **stops**. Both told me the same thing: **3 columns**.

## Where I got stuck

At first I tried these in the login box, but they did not work. The injection point for
this lab is the **category parameter** in the URL, not the login form. The payload only
does anything when it lands in the part of the query that uses my input. Finding that
spot comes before the payload.

## New to me

| Term | What it means |
|---|---|
| Column | A vertical field in a database table (e.g. name, price, category). |
| `UNION` | A normal SQL command that joins the results of two SELECTs, one under the other. It is a real database feature, not an attack by itself. |
| UNION attack | Using injection to add a `UNION SELECT` of my own, so a hidden table's data comes out with the normal results. The command is `UNION`; the attack is what I do with it. |
| `ORDER BY n` | Sort by the n-th column. Used here to count columns: raise `n` until it errors. |
| `UNION SELECT NULL,...` | Count columns by matching the number: add `NULL`s until the error stops. |

## One thing I still wonder

The attack is called a UNION attack, but to just count the columns I did not really use
`UNION` for its real purpose yet (I only used it with NULLs, or used ORDER BY instead). I
think the real UNION work — pulling data out of another table — is the next lab, and that
is where I finally use `UNION SELECT` to read actual rows.
