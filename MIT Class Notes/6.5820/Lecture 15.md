Previously we started our discussion on datacenter networks and covered the VL2 paper, which laid the foundation for its design. The paper for this lecture focuses on Falcon: a novel hardware implementation for datacenter networking.

# Historical Context
Datacenter networking started to gain traction in 2008, with the VL2 paper being published in 2009. At this point, the classical TCP/IP stack was the most common implementation, but it was severely hindering the capabilities of hardware at the time. One of the main issues was queue buildup - datacenters had the capability of delivering packets in microseconds, but the millisecond delays associated with queues. This mostly resulted due to bursty traffic patterns from applications (this is known as incast traffic). Additionally, Congestion control is difficult to implement: a delay-based solution is difficult to utilize across servers (ECN solutions tended to work better).

Post 2010s, network speeds began to rise. 40 Gb/s speeds were attainable (and nowadays it is way faster) but buffers could not keep up with these speeds. Software-based packet processing started to become too slow: the TCP stack took about 1 microsecond to run. Technically, there was a method to bypass the kernel for networking, but this was quite unstable in practice. This led to the search for solutions in hardware. One example was RDMA - allowing for remote reads and writes to storage without involving the CPU.

# Motivation for Hardware Transport
#TODO watch recording

# Falcon Overview
