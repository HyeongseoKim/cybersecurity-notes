# picoCTF — GET aHEAD (Web Exploitation, Easy)

## Goal

This problem was about changing the HTTP method from GET to HEAD.

## What I tried first

I first looked for things I could do on the page itself. I clicked the button on the screen, and I opened the page source to check if there was any unnecessary or hidden code. But there was too much code, and most of it was something I could not understand.

## Where I got stuck

Honestly, I was stuck from the beginning. I did not know where to start, so I checked the hint. The hint said to use a tool to change the request I was sending, and then look at the result.

## What worked

After reading the hint, I started looking for a way to send the request in a different form. That is how I found the curl command with the `-X` option.

I ran it, but the command never finished — the terminal just kept running. I looked up why. The `-X` option only changes the method word in the request, and it does not change anything else about how curl behaves. So curl still expected a body, and it kept waiting for one that was never coming.

Then I used `-I` instead, and it worked. `-I` sets the method in my request to HEAD and does not wait for a response body.

When I ran it, the terminal printed information about the address: `Date`, `Server`, `X-Powered-By`, `Content-Type`, and the flag was there with them.

The flag was in the header, not in the body. A browser only draws the body on the screen, so no matter how long I looked at the page, I could not see it.

## New to me

| Term | What it means |
|---|---|
| HTTP method | The word that tells the server what to do. It comes first in the first line of the request. |
| GET | A method that asks for the content. |
| HEAD | A method that asks for the information only. |
| `-X` | A curl option that changes the method word in the request. It changes nothing else, so curl still waits for a body. |
| `-I` | A curl option that sets the method in my request to HEAD and does not wait for a response body. |
| Request | The message I send to the server. |
| Response | The message the server sends back. It has two parts: headers and body. |
| Header | Information about the response — `Date`, `Server`, `Content-Type`, and so on. |
| Body | The actual content of the response, such as HTML. This is the part the browser draws on the screen. |

## One thing I still do not understand

I am not sure how you decide which option to use. Do you just try things and see, or is there a sign or a situation that tells you to use a certain one? I want to know if there is a rule for this.
