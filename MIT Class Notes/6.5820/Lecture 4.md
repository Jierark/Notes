
So far our congestion control has been focused on individual hosts. One question that one may pose is whether the routers can help with congestion control as well.
There are three different ways that routers could do congestion control:
- Active Queue Management
	- Determine when to signal congestion
	- Determine when to drop packets
- Explicit Congestion Signaling
	- "Mark" packets instead of dropping
	- Tell senders which rates to use
- Packet Scheduling
	- Decide when and which packets to send next
This lecture will cover the first two methods.

# Active Queue Management
The easiest implementation of this is a drop-tail queue. The router maintains a buffer of some fixed size, and drops packets when the buffer is full. It's very simple to build, for obvious reasons.

Here's a question: Does having a bigger buffer always make things better? Probably not. 
- If packets are already being sent at the maximum speed possible, then the delay will start building up as the queue fills up.  That also means there won't be a signal for congestion until the buffer is full. Additionally, many modern applications are quite sensitive to shifts in delay, so this would cause even more issues.
- This also synchronizes the flows between all senders. Each sender will be increasing and decreasing their sending windows simultaneously. This causes the queue to empty out, which then decreases the throughput until it fills up again.
- Despite these, this implementation is quite common among hardware vendors

This shift in queueing delays is known as "bufferbloat", coined by researchers in 2013. It may not seem like it, but changes in RTT can have tangible effects. Longer RTT lead to web pages visibly taking longer to load, for example.

## RED - Random Early Detection
So, how can we deal with bufferbloat? Floyd and Jacobson came up with an idea in 1993 called Random Early Detection (RED). The idea is to drop packets early and probabilistically, which achieves two things:
- It gives sources feedback before the buffer is full, having them decrease their windows
- It also removes synchronization between flows due to the random nature.
So here's how the RED algorithm works:
- Parameters of minimum queue size, maximum queue size, maximum drop probability p
- Before the minimum size, no packets are dropped
- Between the minimum and maximum sizes: drop packets with probability p, which increases linearly up to the maximum
- At the maximum threshold - drop packets with probability 1
- The current size is not the instantaneous size, but rather it uses a rolling average x(t), which typically is the EWMA algorithm. This is because modern network traffic tends to travel in bursts as opposed to continuous increases or decreases.
	- This looks like $x(t) \gets (1-w_{q})x(t-T) + w_{q}q(t)$ at each time step T.
RED is a very nice solution, and although the implementation exists in routers, often it is turned off.
- There is a lot of setup required, and choosing parameters heavily depends on the current state of the network
- A bad choice of parameters can cause the performance to be worse than the simple drop-tail implementation.
	- While these parameters can be derived from control theory, it is a difficult problem.
- Additionally, because it uses the queue length as its metric to drop packets, the drop probability is proportional to the squared number of senders.
## PI - Proportional Integral
Due to these issues, we should look to separate the queue length from the number of senders. This is what the Proportional Integral (PI) Algorithm does.
- The goal of PI is to keep the current queue size $q(t)$ as close to some defined reference point $q_{ref}$ .
	- Another way to say this is that the error term $e(t) = q(t) - q_{ref}$ should be kept as close to 0 as possible.
- It adjust the drop probability in the following way:
	- At every time step T, $$
	p(t) \gets p(t-T) + \alpha(q(t)-q_{ref})$$
	- This is known as an "integral control". It's apparently a concept in control theory.
Here's a related question: what's the impact of delay in the control loop?
As the delay of a control system increases, the oscillations tend to increase as well.
In PI, what we could do is make $\alpha$ small with increased delay, but that will also make changes take longer to occur.
A better way to address this is to introduce a derivative term: $$\beta(q(t)-q(t-T))$$
This gives information about the slope at the current time t, improving stability and decreasing oscillations. So all together, the rule looks like:$$
	p(t) \gets p(t-T) + \alpha(q(t)-q_{ref}) + \beta(q(t)-q(t-T))$$
PIE, PI Enhanced, builds upon the ideas in PI in a few ways:
- It uses the control delay instead of the queue length
- It is capable of auto tuning $\alpha, \beta$ based on the current value of p
- It also uses other heuristics to maintain steady flow, such as burst allowance.

# Explicit Congestion Control
The basic idea of explicit congestion control is simple: each packet has a bit that the router can change to signal congestion instead of dropping packets. This separates packets being dropped due to congestion versus other reasons. The senders then respond to this ECN mark/bit like they would congestion.

There are a few issues with the basic idea, however.
- A simple binary feedback is not very descriptive. The source has no knowledge on the extent of congestion
- TCP involves a slow, linear increase to window size, and an aggressive decrease upon congestion. This is quite problematic in high BDP networks, as it could take many RTTs to ramp up to maximum bandwidths.
Can routers provide more information?

## XCP: An eXplicit Control Protocol
In XCP, the packet headers contain additional information from the sender:
- Current Round Trip Time
- Current Congestion Window
- Feedback Field
As a packet traverses the network, each router is able to change the feedback field and signal whether it needs the sender to increase or decrease its current window. At the end of a packet's trip, the sender receives an aggregate number that tells it how much to change its current congestion window. Routers do not need to maintain any per-flow state, as each sender will always be letting it know what its current window size is, simplifying the design.

How does an XCP router compute the feedback it needs? Each router is composed of two different controllers: an efficiency controller, and a fairness controller.

The efficiency controller's goal is to get the aggregate of its input traffic, y(t) to match its capacity C. The amount of spare bandwidth a router has can be computed as
$$C - y(t)$$Define d as the average round trip time in the network. Then the possible change in the network can be defined as 
$$\alpha *d(C-y(t))$$where $\alpha$ is some constant parameter.
However, we need some additional terms for some specific behavior:
- $y(t) \to C$ to maximize bandwidth
- $q(t) \to 0$ so the queue is empty.
To allow for this behavior, we introduce another term, and the final equation for the desired change becomes
$$\Delta = \alpha*d(C-y(t))-\beta*q(t)$$
The fairness controller's goal is to divide the desired change $\Delta$ between flows such that the flows converge to fairness. We will see the specifics of the fairness controller in the next lecture.