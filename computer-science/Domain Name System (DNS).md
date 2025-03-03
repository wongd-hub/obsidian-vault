#course_cs50 

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
