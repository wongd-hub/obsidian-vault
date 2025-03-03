#course_cs50 
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
