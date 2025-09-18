One of the main problems in designing networks is congestion. This lecture, we will look at End-to-End solutions  for this.

The simplest form of the problem is as follows
#drawing n sources -> Router R_1 -> Router R_2 -> destinations
Assume that all data being sent is the same size. We are interesting in how to best send packets through a bottleneck - the first place where there becomes more data being received than being sent, or the slowest link in the path.

# Goals of the Algorithm
What are some of the key design features we want for the algorithm?
- It should be scalable - considering the size of the Internet, it should be able to be deployed in a wide range of devices
- Each sender needs to make decisions independently of others. Otherwise, the communication overhead might cause too many problems.
- It should be efficient - we want to maximize the speed of the bottleneck.
- It should be "fair" - here, we want the sources $s_{1}\dots s_{n}$ to be transmitting at roughly equal speeds
	- In practice, fairness is hard to define concretely.

Let's define a few key terms:
- Rate - number of packets per unit time
	- Requires the use of an external, central "clock" to define the unit time.
		- This can be problematic during congestion
		- The best way to define a "clock" is to use acknowledgements as the clock. This avoids potential issues during congestion.
- Window - a rolling number of packets to send, set by each sender.
- Round Trip Time (RTT) - the time it takes from the sending of a packet to the receival of the acknowledgement.
	- This value can change due to queue sizes, the physical propagation delay of sending a bit, and other factors. But we can start with a fixed time and figure out the complications later

Ok, so let's start with this algorithm:
- In a period of no congestion, after each RTT, set $w \gets a_{I}w + b_{I}$
- If there is congestion, then set $w \gets a_{D}w + b_{D}$
w is the window size, $a_{I}, b_{I}, a_{D}, b_{D}$ are parameters/coefficients.
For example, let's choose $a_{I}=b_{I}=1,a_{D} = \frac{1}{2}, b_{D} = 0$. Our update rules look like this:
No congestion: $w \gets w+1$
During congestion: $w \gets \frac{1}{2}w$
An overarching question with this is: What is the best way to set this coefficients?

Chiu & Jian found that additive increase, multiplicative decrease algorithms are the best solution for setting coefficients.

A quick aside: the graphs in their paper are known as phase plots, which look at how dynamic systems change over time.
#drawing example plot
#TODO explanation of plot

This brings us to the first pass of a TCP algorithm:

# TCP Algorithm
This algorithm has some very simple rules
- During congestion, decrease the window size by $\frac{1}{2}$
- During periods of no congestion,
	- If the connection had just started, then double the window size after each RTT.
		- This is called "slow start", and helps ramp a connection up to a large enough window size
	- Otherwise, increase the window size by 1 after each RTT
		- Well, more specifically, for each packet that is received in the RTT, increase the window size by 1/w, but it's the same. Similarly for the first rule, it was +1 for each received packet.
		- This step takes effect after the first packet drops, which was the marker for congestion.
