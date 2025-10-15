
Recall our problem from [[MIT Class Notes/6.5820/Lecture 2|Lecture 2]]. We were designing a congestion control algorithm, and ended on a template for a TCP congestion control algorithm.

Let's go back to the RTT. RTT can change depending on a multitude of factors, so we define the variable $RTT_{min}$, the minimum round trip time which only involves the propagation delay.

We now define a new quantity called the **Bandwidth-Delay Product**, which is the product between the rate of the bottleneck and $RTT_{min}$. This represents the number of packets that can be in transit at once without filling any queues.
This is what the throughput might look like for this system:
![[Pasted image 20251014152231.png]]

What happens when we drop packets? That's our signal for congestion, and we scale our window down.
	- Previously, we had these rules:
		- $w \gets w + 1$ on each RTT with no congestion
		- $w \gets \frac{1}{2}w$ when a packet is dropped
TCP-Reno builds on this idea:
- When $w > BDP + B$, predict that a packet will be lost, and set $w\gets \frac{BDP+B}{2}$, where B is the size of the buffer at the bottleneck.
- To maximize throughput, we want to set $B \geq BDP$ , which will bounce the queue between almost empty and almost full. 

Ok, let's do some analysis on our current algorithm:
![[Pasted image 20251014152350.png]]

We're going to define a few quantities here:
- N = number of packets sent in a given cycle
- $\tau$ = number of RTT that it takes before congestion = $\frac{w_{max}}{2}*RTT$
- $p_{drop}$ (drop probability) = 1/N (1 of every N packets get dropped)
- The throughput of this, on average, will be $\frac{N}{\tau}$
Then we can do some interesting analysis
- $N = \sum_{i=0}^{\frac{w_{max}}{2}}\frac{w_{max}}{2}+i$ = $\frac{3}{8}(w_{max})^2$
- $p_{drop} = \frac{8}{3(w_{max})^2} \to w_{max} = \sqrt{\frac{8}{3p_{drop}}}$
- Throughput = $\frac{N}{\tau} = \frac{3}{8}(w_{max})^2 * \frac{1}{\frac{w_{max}}{2}*RTT} = {\sqrt{\frac{3}{2p_{drop}}}\frac{1}{RTT}}$
- There's a subtle issue: in a system with multiple connections, connections with smaller RTTs will have higher throughput. That seems unfair.

# TCP-CUBIC
TCP Reno has a property where it does not remember the previous window size. As a result, it would not be able to increase its window size if the BDP increases.

The idea of cubic is the following:
- After we drop a packet, we log the previous maximum window size, and we want to increase the window size back to that value, $w_{max}$ in some time K. If there isn't any congestion, then we can increase aggressively while there still is no congestion.
- Here, K is a real time measured in seconds, not measured in RTTs.
The function they used for this window size was a cubic function, defined as the following
$$w(t)=C(t-K)^3+w_{max}$$ where C, K are constants.
What does this look like? Well, before time K, it aggressively increases, but then tapers as it gets closer to K.
How do we choose our constants? Well, we can make C dependent on K with the following:
$$w(0^+) = C(0-K)^3+w_{max} = (1-\beta)w_{max}$$
$$C=\frac{\beta w_{max}}{K^3}$$
There are some other subtleties to the algorithm, too. For example, when computing the next window size, if the size from TCP Reno is larger than what this algorithm computes, then it chooses that one. Essentially, it always chooses the larger of the two window sizes.



# BBR
This was developed by a bunch of Google Engineers. Very cool.

Classical algorithms tend to drive the network towards more delay. This is because the ramping of window sizes leads to a buildup in the queue, which causes delays in sending packets.
This algorithm takes a different approach by trying to estimate the BDP.

Recall that $BDP = \mu * RTT_{min}$, where $\mu$ is the bottleneck bandwidth and $RTT_{min}$ is the minimum round trip. The problem in estimating this value is that the most accurate measurement of each value occurs in completely opposite scenarios.
- Measuring $RTT_{min}$ requires a small or no queue, but measuring $\mu$ requires the queue to be very large.
This is what the algorithm does:
- To measure $\mu$, send packets with various rates - faster/slower, and find the rate with the largest number of acknowledgements.
	- More concretely, $\mu_{est} = max(\text{per window rate})$
	- The pacing rate (the rate of a specific window) is defined as $\mu_{est} * \text{pacing gain}$, where the pacing gain is a guess on whether the network has more or less bandwidth available. This changes depending on the previous round trip.
- To measure $RTT_{min}$ , take the minimum seen $RTT$ over the past number of round trips.

The algorithm then sets the window to 2 times the estimated BDP, and checks that there are not that many packets in transit, slowing down if that is true.