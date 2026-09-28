# picoCTF — Cookies (Web Exploitation, Easy)

- **Date:** 2026-09-28
- **Note:** Second web challenge. Builds directly on GET aHEAD — same request/response
  and header ideas, one step further.

<!-- On spoilers: I do not post the flag value, and I do not say which number it was.
     I describe how I found it, not the answer. -->

## The goal, in my own words

Find the flag by working with the site's cookie.

## What I tried first

I opened the site and there was a search box with the answer written next to it, so
I typed it in and pressed enter. The page changed, but there was nothing else I could
do on it, so I opened the page source to look for anything unusual. While doing that I
found a Cookies section in the browser storage. There was a cookie there, and the
Domain column showed a site address, so I visited it. Nothing happened, so I gave up on
that and went back to look at the rest of the cookie.

## Where I got stuck

I did not know a cookie is a value the site gives me to recognise who I am. I saw the
value `0` and, because of the Bandit script I made earlier, I assumed `0` meant
something like "go back to home." So it did not occur to me that I could change it.

(This is the same mistake as before: I took a symbol from one place — Bandit — and
assumed it meant the same thing here. I want to stop doing that.)

## What worked

Something felt off, so I started comparing the first page with the current page to see
what was different. That is when I noticed the cookie value: `-1` on the first page,
`0` on the current one. To check whether it mattered, I changed the value — and the
cookie name shown on the page changed with it. I kept increasing the number until the
flag appeared.

## New to me

| Term | What it means |
|---|---|
| Stateless | The server does not remember me on its own. Each request stands alone. |
| Cookie | A value the site gives me. It is stored in my browser, and I send it back on later requests so the server knows who I am. Because I can change it, the server should not fully trust it. |
| Where a cookie travels | It is stored in my browser, and it rides in the headers — the server sets it with a response header, and I send it back in a request header. So changing the stored value does nothing until I refresh and send a new request. |
| Predictable value | This challenge used `0, 1, 2, ...`, so I could just try them in order by hand. A real session cookie looks like `session=8f3a9c2e1b7d4f6a0c5e9b2d7a1f4c8e` — long and not guessable — so you cannot do that. The weakness here was that the value was predictable. |

## One thing I still do not understand

If I copy the cookie from a logged-in page, log out, and paste that cookie back in,
would I be logged in again just from the cookie? I have not tested this yet.
