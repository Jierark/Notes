In [[MIT Class Notes/6.5820/Lecture 4|Lecture 4]] , we started our discussion on how routers are able to deal with congestion. We saw two schemes:
- Active Queue Management aims to prevent the queue from reaching it maximum capacity by dropping packets early. The schemes we looked at were Random Early Detection (RED) and Proportional Integral (PI).
- Explicit Congestion Control allows packets to signal the presence of congestion, distinguishing it from packets being dropped for other reasons. A simple scheme involving 1 control bit does not work well, so we looked at the eXplicit Control Protocol (XCP).

We're going to finish covering XCP, then move on to the third method of Router congestion control: Scheduling.

# XCP's Fairness Controller
Recall that XCP utilizes two different controllers: the efficiency controller and the fairness controller.

The efficiency controller determines how much the total bandwidth going through it should change: $$\Delta = \alpha*d(C-y(t))-\beta q(t)$$where
	- $\alpha, \beta$ are constants
	- C is the capacity of the router, and y(t) is the current aggregate rate
	- q(t) is the current size of the occupied queue
	- d is the control interval. Think of it as the length of a window we are looking to analyze.

The fairness controller determines how to best split the desired change between its flows. Let's first consider the high level idea of splitting it by drawing from AIMD congestion control schemes.

$\Delta > 0$ means we want to increase all our flows. In TCP Reno, this meant increasing the window size by 1. If we take that idea and apply it here, then we should try to equally split $\Delta$ between all flows.

$\Delta < 0$ means we want to decrease out flows. In general AIMD schemes, we would decrease the window size by some fractional constant. We note that this decrease is proportional to the current window size. Thus, we should divide $\Delta$ proportionally to all the sending rates.

Now, we will look at how we actually compute the splitting of $\Delta$ mathematically. Let's look at the following example of a control interval:
#drawing 

Let's suppose that $\Delta > 0$ and we have 3 flows we want to split it over. We obviously can't just send the aggregate rate back. We need to split the desired change over each flow, over each packet that was sent. The question now is: how much does each acknowledgement change a given rate's window size?

We know each of the flow rates (these are inside the packet header). We know that the flow rate is equal to the following:
$$r_{i} = \frac{cwnd_{i}}{RTT_{i}}$$
where $cwnd_i$ is the congestion window and $RTT_i$ is the round trip time of the i'th flow.
How many packets do we get in a control interval from flow i? That's easy to compute:
$$\text{\#packets} = r_{i}d$$
Here, we have 3 of these flows. So if we want the total flow to change by $\Delta$, then we need to first: Divide $\Delta$ by 3, then split that across all the packets. Thus we arrive at the following:
	For a packet that comes from flow i, we want the feedback to be equal to $\frac{\Delta}{3r_{i}d}$. Generalized to N flows, we want each flow to have the feedback of $\frac{\Delta}{Nr_{id}}$.

There's a core issue here, though. We don't actually know N stored. Remember, we don't want to store any information about the flows locally. Is there a way we can estimate N with the information we get from each packet? Yes, by doing the following:
	Set a counter $\hat{N}$ to 0 at the start of each interval.
	For each packet, update the counter with the following rule:
$$\hat{N} = \hat{N} + \frac{1}{rd}$$where r is the flow rate, computed above from the packet headers.

# Scheduling
In a scheduling scheme, packets go through a classifier and enter one of many queues. A scheduler at the end then follows specific rules to send a packet from each queue.
#drawing 

This get's around a potential issue with TCP algorithms: a device, in theory, could open multiple TCP flows and gain an unfair share of the network. In practice, there are a few applications where multiple TCP connections are opened, such as the downloading of large files. As it turns out, this really is not a big problem, and the concept of TCP fairness has also not been a huge problem when proposing new algorithms.

Alright, so we now want to get "fairness" across each of the flows. We first really want to define the definition of "fairness" and "flows" before we start defining our algorithms. The answer to these really depends on your overall goal and what design choices you are trying to make.

## Fair Queueing
Ok, let's start with our definition of fairness, which we'll call "Maximum fairness" or max/min fair.

Let's say we have a bottleneck rate of some system at 10 MB/s. If we have 4 senders with demands of 8, 7, 5, and 9 MB/s, what's the best way we divide the 10 MB/s? (it's not a trick question)

The most fair way is to split it evenly, so each sender gets 2.5 MB/s.

Simple enough. What if our demands were 1, 2, 5, and 7 MB/s? Well, we can meet the first two demands exactly, and we don't need to allocate more than they need. The last two demands will have to share the remaining rate evenly, so the senders get 1, 2, 3.5, and 3.5 MB/s, respectively.
Algorithmically, we can determine these with the water-filling algorithm, which operates on a simple idea:
- Across each of the senders, we simultaneously increase their flows by the same amount until their share is completely full, or we run out of resources to share. 
- This maximizes the minimum flow, subject to the rest of the demands, hence we have achieved "maximum fairness".
- The algorithm can be generalized to cases where some senders require specific weights relative to the others.

Suppose we have three queues as such:
#drawing

Let's say we use a round robin scheme to schedule packets. Is it max/min fair? No, it will tend to spend more time on queues with larger average packet sizes.

Ok, well what if we did a bit-by-bit round robin? The idea is we only send 1 bit in each round, from each queue. This would indeed by max/min fair. But good luck trying to parse those requests with how split each of them are going to be.

Can we do an approximate version of bit-by-bit round robin? Maybe. Let's do the following:
1) Determine the finish time of each packet if it was sent bit-by-bit. (assume no future arrivals #clarify)
2) Send packets in order of finish times.
With some analysis, you can show the following property for each packet:
$$F_{p} \leq F_{p}^{bit-by-bit} + \frac{L_{max}}{r}$$where
- $F_{p}$ is the finish time of a packet using this scheme
- $F_{p}^{bit-by-bit}$ is the finish time of a packet using bit-by-bit round robin.
- $L_{max}$ is the maximum packet size #clarify
- $r$ is the sending rate of the link
That is, the finishing time of a packet is at most how long it takes to send round-robin and the actual time it takes to send the largest packet we have.

So, how do we actually implement the Fair Queueing algorithm?
First, we need a way to compute the "finishing" time of a packet. Let's define a **round** as 1 complete
