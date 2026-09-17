# picoCTF — Inspect HTML (Web Exploitation, Easy)

- **Date:** 2026-09-17
- **Time spent:** about 5 minutes 
- **Note:** My first web challenge, right after pausing Bandit at level 20.

<!-- On spoilers: I do not post the flag value. picoCTF is fine with writeups,
     but I describe how I found the flag, not the flag itself. -->

## The goal, in my own words

The challenge gave me a web page and asked for the flag. The page showed the word
"Histiaeus" and some text, but nothing that looked like a flag. So the flag was
not on the visible page — it had to be hidden somewhere in the page's code.

## The hint I actually understood

The page mentioned Histiaeus. He was an ancient Greek who tattooed a message on a
servant's shaved head, let the hair grow back, and sent him off, so the message
travelled hidden in plain sight. That is the whole idea of this challenge: what
you see on the screen is not all there is.

## What actually worked

A web page is not the same as the code the browser received. The browser draws the
page, but the source code behind it can hold things that are never shown.

I opened the page source with the browser's web inspector (right-click → view
source). The flag was written into the HTML itself, in a spot that does not appear
on the screen.

## Where I got stuck

When I found `picoCTF{}`, I thought the flag was only the part inside the braces.
The site said it was incorrect, so I went back to check what was wrong, and I
realized I needed to copy the whole thing, `picoCTF{...}` and all.

## New to me

| Concept | What it means | Where else I'd use it |
|---|---|---|
| page vs source | What the browser draws is not the same as the code it received; the source can hide things | Every web challenge starts by reading the source |
| web inspector / view source | The browser tool that shows the real HTML, CSS, and JS behind a page | |
| flag format `picoCTF{...}` | The answer is the whole thing, including `picoCTF{` and `}`, not just the middle | Every picoCTF challenge |

## One thing I still don't fully understand

In this challenge the flag was just sitting in the HTML. If a flag were hidden in
the JavaScript or CSS instead, I do not yet know how I would find it.