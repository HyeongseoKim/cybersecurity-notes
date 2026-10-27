# SQL injection attack, querying the database type and version on Oracle

## Goal
In this lab I need to find DB version information and display it on the web page.

## What I tried first
At first, I tried to find how many columns are in the query by using the ORDER BY command. Then I found the number of columns. However, I wanted to double-check by using the UNION command, but the website always displayed an error.

## Where I got stuck
I could not find what the problem was, because in the last lab both commands worked correctly. Also, I had stopped the cybersecurity practice because I was busy, so I forgot how to figure out this problem. So I read the SQL injection cheat sheet again, and after that I found how to solve it.

## What worked
The answer was using FROM dual. I used it and solved the first problem. However, there was another problem: how to display the version string on the screen. I found the answer in the SQL injection cheat sheet. The page listed lots of commands and how to use them. So I checked them and found the key command, and solved it.

## New to me
First, I thought the ORDER BY and UNION commands work only in a basic form. However, there are a lot of forms of commands and they have many usages, so I need to use commands suitably.

- **dual** — A table with almost no information. Oracle requires FROM, so dual fills the place.
- **banner** — The name of a column in v$version. It holds the Oracle version string.

## One thing I still wonder
Once I get this information, what can I do with it and where can I use it?
