Datacenters are clusters of multiple servers (computers), offering large amounts of computation power for various purposes. Modern scale datacenters are < 10 years old, and they have many uses cases today, such as cloud computing.

The network of datacenters is vastly different than that of the Internet. It is on a much smaller scare, occupying a building as opposed to global connectivity. This induces less delay, and control of the network in a datacenter is often singular, so it tends to be more flexible and offer better controls over protocols and distribution of resources. However, the traffic tends to be less predictable. Lastly, datacenters often need to optimize for resource usage as opposed to connectivity.

What constitutes the highest cost of a datacenter? There are many factors that go into it
- Network Infrastructure: ~15%
- Power utilization: ~15%
- Utility: ~15%
- Cost of Servers: ~45%

# Goals of a Datacenter
Efficiently running a datacenter is hard. The applications running in them often have varying demands, which makes it hard to predict. There is also a need to handle failure, which usually is done with redundancy.

So, what are the goals we want to achieve in a datacenter?
- High utilization & Agility - we want services running constantly, and we want to be able to run them on any server. This is often formulated as a packing problem, with servers being the fungible pool of resources. 
	- This goal contributes to lowering costs and high utilization, and applications can be built and run more easily.
- The network is an important component of a datacenter. We want to offer the illusion of "one big switch" to services so they can run fluidly.
- The most important quality is low latency. The goal is then to manage the tail latency - this represents the slowest response

# Conventional Datacenter Network Problems
Typically, datacenter networks follow a tree topology with some amount of redundancy for failure resiliency. This unfortunately brings about an oversubscription problem, resulting in a limited server-to-server capacity. 

# Modern Datacenter Networks
Networks today allow users to specify their own IP address, network configurations, and subnets transparently. Virtualization allows abstraction of these servers; VL2's name and location separation was a precursor to these modern features.

Let's be more specific with the desired features of a datacenter network.
- Uniform high capacity
	- The maximum rate of server-server traffic should only be limited by the Network Interface Chips on them, and should not depend on the network itself.
	- Server-service assignment should be independent of network topology. This allows us to be more flexible with how we physically place services on servers, allowing movement as needed.
- Performance Isolation
	- The traffic of services should not be affected by other services.
- Routing & Addressing
	- IP addresses to VMs should be consistent even after physical migration. This ties to the first feature of assignments being independent of network topology.

# VL2
VL2 was a proposed method to address some of these issues. While mostly outdated today, it still outlined a good basis for the modern networks. The key ideas are as follows:
- A Clos topology connects servers, granting full bisection bandwidth. This means the bandwidth of any cut is the same.
- Valiant Load Balancing provides high bisection bandwidth across many paths
	- This utilizes randomized flow paths as opposed to randomizing hops.
- ECMP handles failures at top level servers.
- Name/location separation allows applications to move physical locations without address changes.

Understanding these design decisions first requires a dive into the typical traffic flows of datacenters. The authors made the following observations:
- Most flows are quite small, on the order of less than 100 MB.
- Most of the bytes or packets flying around belong to large flows between 100 MB and 1 GB in size.

The design principles of VL2 are as follows:
- Randomization to cope with Volatility - VLB achieves destination independent traffic by randomly choosing intermediate nodes for paths to spread the load in high traffic scenarios.
- Building on proven network technology - VL2 aims to still be compatible with commodity switches to save on hardware costs, so it utilizes the IP routing and forwarding technologies present in these routers.
- Separating names from locators - Names are separated into application specific and location-specific addresses which allows services to be moved around physically without compromising much of its operation.
- Embracing End Systems - A VL2 agent operates on each server to handle interactions with VL2.

# Clos Topology
![[Pasted image 20251124020639.png]]
A Clos topology looks like the following graph. At the bottom are racks consisting of multiple servers, connected to a single router known as Top of Rack routers (ToR). These then connect to the next layer called aggregate, and the top layer is called Core. These connections are set in a way that any link can operate at full throughput. It's known as Bisection bandwidth, and it can be achieved by ensuring that each aggregate switch has the same amount of ports pointing in either direction. (I'll be honest, this might be wrong, but I really couldn't think of a better description). The construction will depend on the routers selected and the desired bisection bandwidth.

# VL2 Routing
It may be tempting to borrow routing techniques used on the internet, such as Distance-Vector or Link-State. However, these scale quite poorly, as with N destinations, there will be O(N) routing entries and messages that need to be sent. With N being in the millions, this quickly turns out to be difficult.

Per-packet load balancing is an idea - suppose a router has packets coming from 3 sources A,B,C and all are heading to D. The router has 3 links that lead to D. Spreading these packets across the links will cause them to arrive out of order (most likely), which does not interact well with TCP.

To address this, it's better to do per-flow load balancing, which is where ECMP Load balancing comes into play:
- Between multiple equal cost paths, use a hash function of a 5-tuple to choose the path.
With any hash function use, one should ask how hash collisions might interact. This would correspond to two flows taking the same path. As it turns out, this is not a huge problem:
- Most flows are small, so this won't cause huge bottlenecks.
- Commodity switch-to-switch links tend to be much thicker than the maximum bandwidth of a flow, mostly due to NIC hardware lagging behind link hardware.

# Name/Address Separation
The servers have flat names that don't have any topological significance. These are maintained in a directory that maps them to their specific Top of Rack Switch. Packets are encapsulated by accessing the directory to determine the ToR switches of each server that is communicating and sent accordingly. 

When a server moves racks, the VL2 agent will update the directory service accordingly. For data that is in transit when this happens, they are periodically sent to the directory service to be resolved.