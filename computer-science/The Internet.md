#course_cs50

> [!note]
> In the late '60s and '70s, the U.S. Department of Defence had a project called ARPANET which allowed computers to talk to each other by exchanging data packets.
> 
> Just a few decades later, everything is somehow interconnected in what is now called the internet.

- But how do you get data from any point to another point? The world is filled with *routers* which are computers whose purpose is to route information from point A to point B. 
    - You're typically not going to have a direct connection between A and B though, there will likely be going through many intermediary points on the way there.
    - An email may be put into a data packet, that will be put in the hands of multiple routers that send the packet to the destination.
        - We have IT administrators and software dynamically figuring out the fastest way to route data between servers.
        - The intent of the internet was to be able to route around downed servers, and this is the functionality we have today.

- What is a data packet more specifically? It is analogous to a physical envelope in the real world. You put your data into the envelope, then you hand it off to the mail carrier and humans get it from point A to point B.
    - But the technical term for how data packets are passed around is a pair of protocols called *TCP/IP*.

# TCP/IP

## Internet Protocol (IP)

- This protocol standardises *IP addresses*. We stipulate that every single computer connected to the internet in the world has a unique IP address.
    - There *is* a way to share IP addresses, but we'll ignore this for the sake of this explanation.

- IP addresses are of the format `#.#.#.#`; each of these `#`s represent a number through 0-255 so IP addresses are generally 4 bytes (32 bits) in size. This also means we can have roughly 4 billion IP addresses in the world under this convention.
    - 4 billion is not quite enough though, so the world is gradually transitioning from IPv4 to IPv6 which allows us to have 128 bits which is a huge number of permutations.

- Below is an ASCII depiction to represent the layout of an IPv4 packet. The number of bits is shown at the top (they go up to 32, 0-indexed).
    - We'll focus mainly on the Source Address and the Destination Address in this class, each taking 32 bits of the packet.
    - One of the most important things that IP does is it standardises the format of these addresses.

```diff
 0                   1                   2                   3  
 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1  
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+  
|Version|  IHL  |Type of Service|          Total Length         |  
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+  
|         Identification        |Flags|      Fragment Offset    |  
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+  
|  Time to Live |    Protocol   |       Header Checksum         |  
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+  
|                         Source Address                        |  
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+  
|                      Destination Address                      |  
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+  
|                        Options              |    Padding      |  
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+  
```

- When sending a data packet, you will sign the envelope with the Destination Address (an IP address), as well as the Source Address (another IP address).
    - That's not quite enough to guarantee delivery though, because it's possible for the router to make mistakes and get overwhelmed (limited memory and compute power). If it does this then it might drop packets because there isn't enough room in its memory.

## Transmission Control Protocol (TCP)

- This helps to guarantee delivery of the data packet by having the sender also add a *sequence number* to the packet.
    - e.g. This is packet 1 of 2 that are being sent to the receiver.
    - If the receiver only gets one of these, then there is enough information to know that there should be more packets.

- However, computers can do multiple things at the same time, so TCP also gives us port numbers so that the receiver can tell where to put or how to use the data they received.
    - For example, how does the receiver know that what they're receiving is an email in Outlook vs. a streamed Youtube video.

- The type of protocol being used is sent in the IPv4 header we discussed above, but information such as the ports, sequence number, and actual data are contained in the data payload of the packet.

```diff
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
| IPv4 HEADER (Metadata: Routing, Addresses, Protocol, etc.)    |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
| DATA (Payload: TCP/UDP segment, HTTP request, file transfer)  |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
```

- Deep-diving into the TCP header, it looks something like this. Notice the Source/Destination Ports, and the Sequence Number.

```diff
  0                   1                   2                   3  
  0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1  
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+  
|          Source Port          |       Destination Port        |  
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+  
|                        Sequence Number                        |  
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+  
|                    Acknowledgment Number                      |  
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+  
| Data  | Res.  |U|A|P|R|S|F|                                   |  
| Offset|erved  |R|C|S|S|Y|I|        Window                     |  
|       |       |G|K|H|T|N|N|                                   |  
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+  
|         Checksum              |      Urgent Pointer           |  
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+  
|                    Options                    |    Padding    |  
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+  
|                            Data                               |  
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+  
```

## Port numbers

- As discussed above, port numbers allow data to be received by specific applications on the receiver's end.
- The two most commonly used ports in TCP are:

| Port | Meaning                        |
| ---- | ------------------------------ |
| 80   | HTTP - the web                 |
| 443  | HTTPS - secure version of HTTP |
- There is no mathematical significance of these numbers, they are just convention.
- So the sender will put a colon at the end of their destination address and then the port they want to receive the packet - e.g. `1.2.3.4:80`.

# Domain Name System (DNS)

- We're not in the habit of typing out IP addresses to visit web-pages; instead we're typing out *domain names* such as <www.google.com.>
    - However, your computer has to address data packets with actual IP addresses at the end of the day. There are only 32 bits available for this.
    - There's another type of server on the internet. Routers route information from point A to point B, whereas Domain Name System servers answer questions of what is the IP address for this domain name?

- What your device is designed to do when you put in a URL into your browser is to ask some local DNS server on your mobile carrier's network or your home's network - what is the IP address of the domain name that was requested?
    - The return should hopefully be a valid IP address.
    - Your device is designed to ask DNS servers hierarchically - starting with the nearest one (likely provided by your Internet Service Provider), then moving to the next most important one.
        - Your device used to be manually configured to know the IP address of the nearest DNS server.
    - There are a finite number of *root servers* in the world that know of all possible domain names and their corresponding IP addresses.
    - Part of what you're paying for when renting domain names is for someone to associate your domain name with your desired IP address.

- Conceptually, DNS servers will contain a dictionary or a database table with two columns: a *Fully Qualified Domain Name*, and the corresponding IP address.

# Dynamic Host Configuration Protocol (DHCP)

- These answer questions on what your DNS server and router should be. When your device powers up, it broadcasts a request to the DHCP server and receives the answer.
- These servers also tell your device what IP address it should use.

# Hypertext Transfer Protocol (HTTP)

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

