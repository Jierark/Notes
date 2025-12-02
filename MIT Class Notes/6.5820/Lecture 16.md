DNS is one of the fundamental pillars of the Internet. As a refresher on how it works, the basic idea is that it maps **Domain Names** to IP addresses, making the process of accessing web pages much easier.

More precisely, DNS maps domain names to resource records. There are four(?) types of records stored on DNS servers:
- Cname
- A: Stores IP addresses
- AAAA: Store IPv6 addresses
- S: Stores locations of name servers.
DNS also contains a caching mechanism. In short, when the correct IP address is found, the DNS resolver will cache that result for a fixed period of time, allowing for fast access to repeat queries.

The protocol for any device to connect to an access point is called DHCP. It's not very important to know completely for this.

There are 13 root nameservers - coincidentally chosen to be able to fit into a 512-byte DNS packet. These contain records for top level domains and are basically the backbone of DNS. As a result, they also process an extraordinarily high amount of traffic.

Multiple domains can also map to the same address. This is known as Anycast.

DNS also supports the mapping of IP addresses to hostnames. This is coined as a reverse lookup, and its top level domain is inaddr.arpa. This can be handy to figure out who a specific client is.


# Content Delivery Networks
The motivation behind a CDN is to lower latency of retrieving web objects. This improves quality of experience because people tend to be impatient.

Some common causes of Internet latency are as follows:
- The content servers are physically far away from consumers.
- Poor optimization of content
- Peering points which connect the Internet have some kind of congestion or similar.
- TCP issues.
Many of these can be mitigated using CDNs. While they have some cost in building them, the improvements to QoE are often way larger and make it worth the investment. There are many CDN providers today, but here we focus on Akamai.

Akamai essentially operate as a software layer network on top of the Internet. The main components of Akamai are as follows:
- A mapping system to direct users to the optimal edge-server based on real-time network conditions
- The edge servers are deployed in thousands of networks close to users, serving any cached or dynamic content
- A Transport System moves data between origin servers and edge servers eﬃciently.
- Communications and Control System which distributes conﬁguration and control data fault-tolerantly. 
- Data Collection and Analysis System that gathers logs and metrics for analytics, billing, and monitoring.
- A Management Portal with a Customer-facing interface for conﬁguration, monitoring, and analytics.

DNS is used in an interesting way. In short, it's possible to determine all possible URLs needed to load a specific page and host some of the more dynamic content on CDNs. This often includes some invisible icons for various purposes. As this content is updated, it gets pushed to the edge servers.

How does Akamai send clients to the best edge server? Akamai has a performance map of the Internet that gives a good idea of what the Internet looks like at any time. The mapping service is then capable of routing to the fastest edge server for the user.

There were many tangents in this lecture. But the main points are probably captured here.


