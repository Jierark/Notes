The modern Internet is huge. Imagine if we had to figure out direct connections between all endpoints. It quite literally is impossible to achieve. Fortunately, we have large ISPs that manage our connections and a whole system of other network providers connecting each other to power the internet.
![[Pasted image 20251014153425.png]]
These ISPs operate on Autonomous Systems, which handle the routing of packets to whatever destinations they need to go. This lecture, we're going to cover how these Autonomous Systems connect to endpoints and other AS, and the principle behind how routing information is shared.

# Interconnection
The key principle behind this is cooperative competition. ISPs want to cooperate with each other so they can connect the entire globe, but they also have self-interests that they want to achieve for economic reasons. After all, they are businesses.

Interconnection refers to the various ways networks attach and exchange traffic. This encapsulates both the business practices and the technical mechanisms used. Why do we have separate methods of interconnection? There's two main reasons
1. There is no central authority that manages internet connections
2. High bandwidth and latency sensitive apps create demand for changes to the way Internet traffic is routed.

## History of Interconnection
Before 1995, the ARPAnet heavily relied on NSFnet as its backbone for large-scale connectivity. Smaller networks, such as those local to universities, would connect to that to reach parts of the country they normally couldn't. The routing mechanism during this time is relatively simple:
- Inside the local network, one can route to the various endpoints
- Outside the local network, one would simply connect and send packets to the backbone, which would handle all routing logic.

Between 1992 and 1994, there was an Interconnection Initiative started by Al Gore, with the goal of offloading the maintenance of the network to private companies. Since 1995, the internet shifted to the one we are more familiar with. There are three tiers (1,2, and 3) of ISPs that manage connectivity at different scales, with Tier 1 being the largest and responsible for global connectivity. Notably, these ISPs must know how to route **any** packet, unlike lower tier ISPs which can default packets to a higher tier if they do not have the routing information.

1993 saw the development of BGP4 as the main method for these ISPs to connect. It's an elegant framework, being very flexible and malleable to the specifications of ISPs and its network operators. Note that it inherently does not possess any optimizations.

## Interconnection Today
Today, interconnection has become flat and largely decentralized. Additionally we see the rise of CDNs, content distribution networks that mainly optimize for latency by serving various files at multiple physical locations such that each one can cover a specific subgroup of the population.

Another interesting note of the modern age is the scale of traffic. In 2009, roughly 150 companies produced almost 1/2 of all internet traffic. In 2014, this dropped to only 30 companies, and the latest statistic in 2022 showed that 65% of the traffic came from 6 of the most prevalent tech companies, such as Google, Facebook, and Meta.

# There's Too many Numbers!
The current standard for routing is IP addresses, as you may know. An IP address (IPv4) is a 32 bit address that represents some physical location in the world. For routing, these IP addresses appear as prefixes that cover a large domain of addresses. For example, the notation 18.0.0.0/8 covers all ip addresses from 18.0.0.0 to 18.255.255.255. The number at the end represents how many bits are being used for the address.

Routing is the exchange of information to figure out which routes are available. Forwarding packets uses the longest prefix match to determine which path to take. This is equivalent to choosing the route with the most "specific" match. These rules are laid out in large routing tables within routers. In the case that there are multiple matches of the same length, routers can implement specific rules to break these ties.

# Internet Business Model
Autonomous Systems (being called AS from now on) are the large "routers" that manage networking at large scales. Each one has a 32 bit number that uniquely identifies them. We're going to look at how AS manage to share routes. The easiest way to keep track of these rules is that almost all of the ways BGP operates can be justified with incentives and economics: can an AS make a net profit based on how they share routes?

There are three different groups in the business model of an AS: Customers, providers, and peers. 

Customers pay the AS to get access to their routes. The AS has an incentive to give the customer access to the entire routing table because it results in more traffic and more profits. For routes that the AS can't access, they can send packets to their provider, who they pay for access to more routes. Alternatively, they can establish peering relationships with other AS of the same tier.

Peers will share traffic between each other for no cost. This works great when the amount of traffic is roughly symmetric between them, and they can save on not having to pay providers. Peering is extremely important for Tier 1 providers, who don't have a higher tier provider to send traffic to.

These customer/provider relationships are formally called **Transit**. There are two options of transit that can be given:
- Full transit means the customer gets access to all routes.
- Partial transit means the customer gets access to a subset of all routes.

So overall, how does an AS decide which routes to share?
- If the routes come from a customer, those routes are announced to everyone, resulting in more traffic to the customer and more profits.
- If the routes come from providers, these are only announced to the customers. These are not shared to peers because there is a cost for sending traffic to providers, which can be made up from the customers.
- For peering, it typically occurs when a lot of traffic symmetrically goes between two AS. It saves money by not needing to pay for that traffic between them.
	- A peer will only advertise its customer routes and self-origin routes to another peer. Again, no incentive to advertise its providers.
		- Think about it this way: Let's say A is a provider to both B and C. B and C enter a peering relation to be able to send traffic between their customers.
		- If B, for whatever reason, decides to share its provider's routes to C, then C can send all the traffic destined to A through B, at no cost to C, and B will have to pay for all of it. That's bad business practice.
	- Peering can occur both formally and informally. Nowadays, peering agreements are formal, with full contracts and the likes.

# Policy with BGP
There are two kinds of policies in BGP: Import and Export. Each are provided to BGP as configuration information, and are enforced by BGP choosing paths from multiple alternatives (import) and controlling advertisements to other ASes (export).

## Import Policy
So an AS has all of these routes from peers, providers, and customers. How does BGP determine which route is "best" for routing specific packets? BGP utilizes the longest prefix match (LPM): it chooses the route with the most specific match to a packet's destination to forward it. In the case of a tie between multiple matches, then it will prefer a customer's route over a peer route, over a provider route. This should make sense from a financial standpoint, and the implementation is often through a LOCALPREF variable inside a BGP router.

## Export Policy
The previous sections went over most of how export policies are shared, but it's always helpful to have a more concise overview of it.

To customers, BGP announces all routes, regardless of where it came from, and self-origin routes. Customers are paying for the traffic, anyways.

To providers, it will only announce routes from customers and self-origin. Peers function in the exact same way.

# Physical Interconnection 
For networks to interconnect, there needs to be a **physical** connection between them, in addition to the network connectivity. Typically, the meeting point is some kind of facility with the supporting equipment required for this. These are called colocation facilities, and they will lease a secure space for customers to locate and operate equipment. An access point to a communication provider's network is known as a Point of Presence.

There are two common options for physical interconnection. A **direct interconnection** is a private bilateral agreement between two networks using a dedicated physical connection. A **Public connection** is a multilateral agreement between many parties where all networks connect into a public Internet Exchange Switch. One of these just so happens to exist near Boston.