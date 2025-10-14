There's a few cleanup notes we need about BGP before we move onto the main topic of discussion.
# BGP, continued
Last time, we ended on the import and export policies of BGP. To recap:
- BGP will export all learned routes to its customers, but it will only export routes to customers and self-origin to peers and providers.
- BGP chooses a route for a given destination based on the longest-prefix match, and will prefer customers over peers over providers in the case of a tie.

Now, an Autonomous System is composed of many servers, each with various addresses and varying traffic. If we used all of them to route BGP, it would become a massive scaling problem. Instead, there are specific routers, known as "Border Routers" that exist on the metaphorical "edge" of an AS. These are what handle communications with other ASes, and this previously used to require a physical IP link to establish communications. It should be obvious that BGP runs over TCP, as you need the reliability to transmit large routes without error.

Here's what a Routing Table might look like:
![[Pasted image 20250929152118.png]]
An interesting note is the AS Path. This shows, from left to right, the order of ASes which will be traversed to reach the destination (the right-most AS). This is mainly to limit the possibility of a cycle. Anyways, the router will chose one of these based on its own local preferences. Maybe the top entry is a customer provided route, so it will forward a packet through there.

# Path-Vector Routing
The structure of routes looks quite similar to those in path-vector routing. This is an extension of distance-vector routing, but works better for flexible routing policies. 

It has a very simple idea: you advertise the entire path to a destination. 
- For each destination d, you would send the distance metric to d, and the entire path to reach it.
Why would we do it this way? Consider classical shortest path algorithms, such as Dijkstra's Algorithm. There's a few reasons why they wouldn't work:
- They scale very poorly with the scale of the Internet.
- There's no way to "keep routes secret" (which here is done by specific export policies)
We can use path-vector routing to achieve both of those qualities, and each AS gets to apply their own local policy.

# Internal BGP
So far, we have been looking at external BGP, which exchanges routes between ASes. Internal BGP, as the name suggests, is the protocol by which external routes are distributed between routers inside an AS. Note that this is separate from IGP (Internal Gateway Protocol), which exchanges routes to internal destinations within the routers of an AS. 

There are two different ways to do iBGP:
- Full Mesh - Each eBGP router has an iBGP session with every other router in the AS. 
- Route Reflection - Each eBGP router has an iBGP session with a logically central route reflector, and each router has an iBGP session with the route reflector.
Note that for an AS with 1 border router, there isn't a need for iBGP.

# Route Attributes and Route Selection
BGP contains multiple ways to choose the "best" routes, with each one focusing on a different attribute of the route. Multiple attributes may be considered in order to break a multiple-way tie between routes.

## Local Preference
Local Preference specifies an order over which routes outbound traffic should take. Higher values take priority over lower ones, but lower valued routes may be taken in certain conditions (for example, network outages).


### AS Path Length
If there are multiple routes with the same local preference, this attribute breaks the tie by selecting the path with the shortest AS path length. However, this route may not be the shortest in terms of IP path length or lowest delay.

### Multiple Exit Discriminator
??? there's 1 slide on this??? guess i'll go watch the recording...

### Hot Potato Routing
Interestingly, more than 1/2 of internet paths aren't exactly symmetric. This means the paths from S to D and D to S may not traverse the same ASes. One potential reason is that an AS typically has mutliple border routers, so a packet from inside has multiple possible routes. This leads to the idea of Hot Potato Routing.

The general idea of Hot Potato Routing is that traffic needs to leave the AS as soon as possible. To do so, it prefers routes with shorter IGP path cost to next-hop. However, this can cause large traffic shifts if IGP weights shift suddenly.

### Router ID Tiebreaking
At the lowest level, if there are still ties between which routers to pick, a router ID may be hardcoded to be favored over other ones.

# Issues with BGP
BGP is an incredibly complex protocol with many parameters that need to be fine-tuned to work correctly. It also needs to accommodate ISP policies which may not be optimal in terms of delays or lengths. As a result, BGP is prone to misconfiguration. BGP is also slow to converge during failure recovery, and it can suffer from performance, especially in app-specific contexts.

BGP also has a large issue with security and a lack of authorization. This is mostly due to its design. For an example, see the example in the reading with Youtube and Pakistan. There are two possible ways to include security in BGP:
- Origin authentication - An AS announces that it "owns" a specific IP range. This requires either a form of registry or another self-certifying binding between the AS and the specified IP range.
- Path authentication - Routes are augmented with a cryptographic scheme such that any tampering of the advertisement is difficult. One can achieve this with a number of asymmetric cryptography schemes.

BGP also suffers from a vulnerability known as BGP hijacking. In short, a malicious BGP router can announce it has a path to a certain IP range to neighboring ASes. This router is placed in a way such that the actual owner of the IP range is far from the targeted ASes. There are a few potential harms of BGP hijacking:
- Loss of availability as packets going to that destination end up going nowhere.
- Abuse of IP Addresses
- Fake end points can hijack requests intended for the destination and steal sensitive information. This has already been used to steal cryptocurrency, for example.

# Espresso
Google offers many services that need to be connected to many ISPs and users, and they often are handling terabits of traffic that require low latency. Traditional routers are not the best for a few reasons:
- They are very expensive, often being 4-10x the cost per port
- Routers are often rigid and feature-limited by vendors
- There is an inherent risk of upgrades and failures, which can affect large amounts of traffic.
Espresso utilizes Google's internal servers to handle many of the routing logic, as opposed to traditional architectures where the edge routers handle the logic. It will meet the following requirements of Google:
- Efficiency - lower cost and better utilization
- Interoperability - supports BGP and IPv4/6
- Reliability - availability is up 99.999% of the time.
- Incremental Deployment - can co-exist with legacy standards.
- High feature velocity - new features can be up and running within weeks instead of years.

Before we get into the specifics, let's cover a few key definitions
- BGP: Border Gateway Protocol - already covered in detail
- SDN: Software-Defined Network - moves control from distributed routers into logically-centralized server software
- MPLS: Multiprotocol Label Switching - label-based forwarding as opposed to direct addresses
- GRE tunnel: Encapsulation for Virtual Links
- FIB: Forwarding Information Base - (Forwarding Table which is based on the routing table)
- ACL: Access Control List (where you set firewall rules)
- DDoS: Distributed Denial of Service - a type of network attack whose origin spans multiple IP addresses, with the goal of rendering a service inoperable or completely offline.

Espresso was designed with the following design principles in mind:
- It utilizes a Hierarchical control plane, where a local control plane provides fast, local decisions to outages and a global plane handles overall traffic engineering
- It utilizes a fail-safe system: if the global controller fails or is unreachable, then it will maintain the last known good state
- Programmability - Logic of handling packets are done in servers and software, all within Google's internal servers, as opposed to the routers
- Testability - Automated and layered testing can be done. Also Canaries, whatever that means.
- Intent-driven management - goals can be easily and automatically translated to code and configuration

Externally, Espresso operates the same as a regular BGP instance. Internally, Espresso is able to do much more with the packet, including traffic optimization, load balancing, and apply ACL rules. The paper also claims they are able to perform DDoS handling in software, but it's more likely that it's not as simple as it is described based on how other DDoS mitigation mechanisms work.

The use of a hierarchical control plane also allows for more flexibility - local controllers can use application-specific contexts to handle routing decisions as opposed to only relying on routing tables. This turns out to be significant in applications such as youtube.

Evaluation results of Espresso resulted in more internet traffic to Google, as well as improved quality of experience for videos and a 50x faster release velocity than traditional routers.