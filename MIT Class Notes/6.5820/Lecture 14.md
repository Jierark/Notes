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
Typically, datacenter networks follow a tree topology with some amount of redundancy for failure resiliency. This unfortunately brings about an oversubscription problem, resulting in a limited server-to-server capacity. #TODO 

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
- (more that I missed)

# VL2
VL2 was a proposed method to address some of these issues. While mostly outdated today, it still outlined a good basis for the modern networks. The key ideas are as follows:
- A Clos topology connects servers, granting full bisection bandwidth. This means the bandwidth of any cut is the same #clarify 
- Valiant Load Balancing provides high bisection bandwidth across many paths
	- This utilizes randomized flow paths as opposed to randomizing hops.
- ECMP handles failures at top level servers.
- Name/location separation allows applications to move physical locations without address changes.

