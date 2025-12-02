The final lecture!

This paper covers an interesting problem for BGP providers.
![[Pasted image 20251124091250.png]]
ignore the amogus
More succinctly,

Suppose you are an operator of AS1. AS3 is your provider, and AS2 is your customer. Normal flows look like those in red or green. But what about flows that resemble the one in purple? You can clearly see that it's source is not AS2, even if it claims to be that. How can you mitigate these flows?

The simplest method would be to check your BGP routing table. If a packet took the expected path, then it's probably valid. This approach has two issues. Firstly, the process is slow to do for all packets, killing throughput. This could be mitigated by random sampling, but the other issue is that any network issues can desync the data and control planes.

# Detecting Legitimate Traffic
So, how do we tell when traffic is legitimate? Ideally, legitimate traffic should follow a closed loop: basically a typical TCP flow where a packet is sent and some ACK is used as the reply.

A checker can act between the source and destination and modify the path or tweak TCP traffic. However, this could affect the return path, as these tend to be asymmetric and would not be caught by this checker. The simplest thing to do is to drop a data packet. Legitimate traffic will include a retransmission; spoofed traffic won't. This doesn't really work in practice for a few reasons:
- The path taken on retransmission may take a different path and completely miss the checker. This might occur if network conditions change or the source thinks some path is down.
- Spoofed traffic can still subvert the checker by retransmitting the packet manually after some delay.

Can we do better?

# Penny
The method outlined in the paper extends the idea from before: dropping more packets builds more confidence on if it is legitimate traffic or not.

Penny operates on a statistical model comparing two hypotheses:
- $H_{1}$: The flow is closed-loop
- $H_{2}$: The flow is not closed-loop and is spoofed.

It keeps track of the following quantities:
- $n_{RTX}$ represents the number of observed retransmissions for dropped packets
- $n_{noRTX}$ represents the number of packets which did not have a retransmission.
- $p_{noRTX}$ is an assumed probability of not seeing a retransmission for a packet within a closed-loop flow.
- $f_{dup}$ is the fraction of observed packets with one or more duplicates.
- The following probabilities will be important:
	- $P(H_{1}) = (p_{noRTX})^{n_{noRTX}}$
	- $P(H_{2})=(f_{dup})^{n_{RTX}}$
	- $P(genuine) = \frac{P(H_{1})}{P(H_{1})+P(H_{2})}$
When a packet is dropped, it records a snapshot of the observed, duplicate, and retransmitted packets at the time of drop and updates the values above accordingly. When a retransmission is received or expires, it will update the corresponding snapshots.

What if the TCP flows are relatively short? There may not be enough packets dropped to reach a definitive conclusion, so the solution is to aggregate statistics from multiple flows and use the same statistical model.

In cases where flows have abrupt interruptions, the snapshots will simply disregard any pending retransmissions and observe packets for that flow.

# Evaluation
In a system of all closed-loop traffic, Penny's statistics always work: it will correctly identify flows as closed-loop. In malicious traffic, Penny is correct so long as enough packets are dropped (which turns out to be at least 12). However, it doesn't work well in mixed traffic, but it will test some individual flows if the aggregate looks spoofed. Additionally, the dropping of packets does not have a huge impact on performance, and it does not have huge performance or memory requirements, allowing it to be run in a large range of environments. Penny can be a worthwhile primitive to improve the accuracy of ISPs' measurement and monitoring systems.