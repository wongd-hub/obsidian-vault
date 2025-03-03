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

# How the internet works

- [[TCP & IP]]
- [[Domain Name System (DNS)]]
- [[Dynamic Host Configuration Protocol (DHCP)]]
- [[Hypertext Transfer Protocol (HTTP)]]
- [[HTTP Status Codes]]
