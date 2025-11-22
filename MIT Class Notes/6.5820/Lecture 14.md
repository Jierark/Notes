Datacenters are clusters of multiple servers (computers), offering large amounts of computation power for various purposes. Modern scale datacenters are < 10 years old, and they have many uses cases today, such as cloud computing, TODO.

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