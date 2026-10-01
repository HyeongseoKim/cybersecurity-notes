# picoCTF — Where are the robots (Web Exploitation, Easy)

- **Date:** 2026-10-01
- **Note:** Third web challenge. This one was not about changing a request —
  it was about finding a page the site never links to.

<!-- On spoilers: I do not post the flag value or the hidden path.
     I describe how I found it, not the answer. -->

## The goal

This challenge asks me to find `robots.txt`, then use the path written inside it to
reach a hidden page and get the flag.

## What I tried first

I checked the website's source, but there was nothing special — no links to other
pages, no hidden pages, and no unusual code. I was stuck at that point.

While looking through the source I did click on a file that looked interesting, but it
turned out to be a Google font file that the site had loaded from `fonts.gstatic.com`.
It had nothing to do with the challenge. I learned to check the domain first: files
from another domain are borrowed, not part of the site I am looking at.

## Where I got stuck

I needed to find a hidden file, but I did not know how to look for one or where it
would be.

## What worked

When I was stuck, I used the hint. It explained that a website can hold information
the creator does not want to show. After I read the hint, I searched for where a site
keeps that kind of information, and found `robots.txt`.

I opened it by adding `/robots.txt` to the site address. Inside there was a `Disallow:`
line with another path. I added that path to the site address in the same way, and the
hidden page opened with no login and no permission check.

## New to me

| Term | What it means |
|---|---|
| `robots.txt` | A text file at the top of a site that asks search engines not to list certain pages. It is a request, not a lock. |
| `Disallow:` | A line in `robots.txt` naming a path the site does not want listed. |
| Hidden vs protected | The page was not linked from anywhere, so you need the exact address to reach it. But nothing stopped me once I had the address — no login, no permission check. Hiding a page is not the same as protecting it. |
| Security through obscurity | Relying on "nobody knows the address" instead of checking permissions. It fails as soon as the address leaks — and here `robots.txt` leaked it. |

## The part that surprised me

I first wrote in my notes that `robots.txt` makes a page hard for people to find. That
is backwards. If the path had not been listed in `robots.txt`, I would never have found
the page. The file did not hide the page — it pointed straight at it.

So a site that writes a secret path into `robots.txt` is publishing a list of the pages
it wanted to keep quiet.

## One thing I still do not understand

This time I only needed to find one hidden file. There are many other kinds of hidden
files and paths on a website, and I do not know them yet.
