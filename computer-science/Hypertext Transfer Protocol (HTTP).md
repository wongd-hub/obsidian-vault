#course_cs50 


- Governs how web browsers and web servers speak. This protocol standardises what goes into data packet envelopes when it comes to allowing a web browser to request and receive information from a web server.
    - HTTPS is the secure version of this, the connection is encrypted so it's unlikely that someone who intercepts the traffic will know what's in the envelope.

## URLs

- `https://www.example.com/`: the terminating slash means give me the root folder of the website
- `https://www.example.com/path`: this means accessing a certain file on the web server. Note that older websites might show `path.html` but newer servers hide this file extension
- `https://www.example.com/folder/`: this means accessing a folder on the web server.
- `www.example.com`: the *Fully Qualified Domain Name*
    - `www`: the *host name*, not strictly required and this was mainly to signal that a URL in print was a web address. Modern sites hide this.
        - You can re-direct users from the `www` server to another using tricks we'll discuss later.
    - `com`: the *top level domain*, there are multiple different values this can be, and can also be a 2-character country code (e.g. `au`)
    - `https`: the *scheme* or the *protocol*

## Placing user input into URLs

- There is a standard way of sending input from a browser to a server, generally formatted as a URL ending in a `?` and then key-value pairs delimited by ampersands. ^78ae8e

```
https://www.example.com/path?key=value&key=value

# e.g.
https://www.google.com/search?q=dogs
```

## Ways to request information

- There are two main ways to format what goes into the request data packet envelope: `GET` and `POST`. The more common one is `GET` so we'll focus on this.
- Inside of the envelope are HTTP messages that look like this - in this example we are simply visiting the <https://www.harvard.edu> website:
    - Upon pressing Enter in the URL bar, your computer will package up this text into an envelope, address the envelope with the destination IP and the source IP, then hand it off to the nearest router.

```
GET / HTTP/2
Host: www.harvard.edu
...
```

- Breaking this down:
    - `GET` is the verb
    - `/` means get the root path of the website
    - `HTTP/2` means the version of HTTP to use
    - `Host:` is a HTTP header that tells the server what fully qualified domain name it's looking for
        - This is important because you might have multiple websites being hosted on a single server.
        - Notice that this is another key-value pair - similar to a dictionary

- What comes back from the server to the browser? A message like the following:
    - An acknowledgement of the version being used
    - `200` a status code
    - `Content-type: …` telling us what content is in the data packet

```
HTTP/2 200
Content-Type: text/html
...
```

- We can see this if we run the following:

![[Pasted image 20250302225850.png]]
## Devtools

### Network tab

- This is how you can see all the data requests - all the data packets being requested and sent back - whenever you visit a website.
- Each of these rows of output represents a part of the website.
    - The first request is the initial request to the website, analogous to what we discussed above.
- The total number of requests in the bottom left shows how many times a data request was made.