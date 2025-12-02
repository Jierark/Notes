Previously we started our discussion on datacenter networks and covered the VL2 paper, which laid the foundation for its design. The paper for this lecture focuses on Falcon: a novel hardware implementation for datacenter networking.

# Historical Context
Datacenter networking started to gain traction in 2008, with the VL2 paper being published in 2009. At this point, the classical TCP/IP stack was the most common implementation, but it was severely hindering the capabilities of hardware at the time. One of the main issues was queue buildup - datacenters had the capability of delivering packets in microseconds, but the millisecond delays associated with queues. This mostly resulted due to bursty traffic patterns from applications (this is known as incast traffic). Additionally, Congestion control is difficult to implement: a delay-based solution is difficult to utilize across servers (ECN solutions tended to work better).

Post 2010s, network speeds began to rise. 40 Gb/s speeds were attainable (and nowadays it is way faster) but buffers could not keep up with these speeds. Software-based packet processing started to become too slow: the TCP stack took about 1 microsecond to run. Technically, there was a method to bypass the kernel for networking, but this was quite unstable in practice. This led to the search for solutions in hardware. One example was RDMA - allowing for remote reads and writes to storage without involving the CPU.

# Motivation for Hardware Transport
New and demanding workloads require better solutions than what is currently out there, but we also need to satisfy the demands of existing ones. Software based solutions have gotten us so far, but a new solution in hardware should prove to be orders of magnitude better. The current hardware solutions like RDMA over Converged Ethernet (RoCE) isn't well suited for general purpose datacenters and works only in niche use cases. 

An ideal hardware transport satisfies the following requirements:
- Predictable Performance under Diverse Networks
- Robustness under stress including losses and reordering of packets
- Support standard interfaces for lift-and-shift customers
- Operational ease & rapid iteration

# Falcon Overview
Falcon is a hardware transport inside the NIC. It interfaces with applications by the various Upper Layer Protocols (ULPs) such as RDMA or NVME and does so with a request-response transaction interface. There are three layers to Falcon:
- The ULP Mapping Layer translates between various ULPs to internal Falcon connections and maintains flow control
- The Transaction Layer provides the request-response interface and manages the scheduling and order of transactions.
- The Packet Delivery Layer interfaces with the network and manages congestion. It uses a variant of a CC scheme called Swift.
This ensures support for multiple ULPs without dealing with the multiplicative increase in hardware complexity.

Falcon also achieves load balancing by leveraging multiple paths in the network fabric transparently. A naive method of spraying packets doesn't really work unless the network conditions are roughly equal for each path. This isn't very true in general. It does this by enforcing per-connection window sizes.

One challenge with hardware solutions is that new modifications to the transport layer could require new hardware to accomodate those changes. The Falcon Adaptive Engine (FAE) manages these transport features separately from the hardware mechanisms, and this runs in a general CPU.

Falcon does achieve the requirements outlined before.
- Predictable Performance under Diverse Networks - delay-based congestion control and multipathing
- Robustness under stress including losses and reordering of packets - HW-based reliability and use of on-NIC resources
- Support standard interfaces for lift-and-shift customers - can interface with multiple ULPs through its layered design, and the transaction interface is simple
- Operational ease & rapid iteration - Hardware-assisted programmability through FAE
Google's evaluation of deploying Falcon is quite impressive. It basically meets the ideal requirements in testing.