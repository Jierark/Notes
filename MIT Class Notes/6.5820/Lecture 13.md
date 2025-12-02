A few points on Cellular Networks before we start discussing Roofnet.

# Variability
Rates vary in a cellular network for two main reasons. Firstly, people (and devices) tend to move around, causing varying distances to the Base Stations. Interference and high traffic also are a cause of variability.

Suppose we have the following flows to an access point:
![[Pasted image 20251028150402.png]]
Each flow maintains a per-user queue. For simplicity, assume that the packet sizes are all the same. The optimal fairness solution for this would be a round-robin scheduling.

Suppose now that the rates are different. This could be due to the users being different distances from the Access Point:
$$u_{1}=10pk/s, u_{2}=1pk/s$$
Some access points prevent connection if the rate is too low. A better fairness idea is to give fairness with time instead of packets. Here's a new fairness solution:
- In 1 round, send 1 second worth of packets from each. This achieves proportional fairness. An effective rate of $10pk/2s + 1pk/2s = 5.5 pk/s$.
Of course, this doesn't really work well in practice. Devices and users move around all the time, affecting signal strength.

A more realistic solution is to use variable rates. Define $R_{i}(t)$ as the instantaneous rate of user i. The average rate $R_{avg,i}$ is defined as $$\begin{cases}
\alpha R_{i}(t). + (1-\alpha)R_{avg}(t-1) & i^*=i; \\
(1-\alpha)R_{avg}(t-1) & \forall i\neq i^*
\end{cases}$$
The choice of $R_{i}$ determines the fairness factor. Max-min fairness is achieved when $R_{i}(t)$ is low. A higher choice of $R_i(t)$ maximizes throughput. Proportional fairness is achieved for $i^* = max_{i}\left( \frac{R_{i}(t)}{R_{avg,i}(t)} \right)$.


# Roofnet
Roofnet started as a CSAIL Project in the 2000s, with the goal of creating a mesh network at scale. It aimed to achieve operation without central management, while maintaining a wide coverage and acceptable performance. Some of the nodes are placed around MIT campus.

Some of the key design decisions in Roofnet are:
- Unconstrained node placement - the network can still function without the strict topology of most networks.
- Omni-directional antennas simplify the installation process because users don't need to determine the precise direction of other antenaae
- Multi-hop routing - using both link-state and source routing improves coverage and performance
- ETT - a routing metric that optimizes for throughput in a slowly changing network. This comes as an extension from ET.
- SampleRate - a bit-rate selection method using throughput estimates from actual data delivery.

Now, Roofnet uses radio to deliver signals. What can affect the strength of these signals?
- An obvious one: the physical distance between the antennae. Generally, it will decrease as a function of distance.
- Signal-to-Noise ratio is a large factor for delivering radio signals. The basic idea is that the signal needs to be some number of dB in order to overpower general noise. Higher ratios have stronger guarantees on delivery of the signals.

A core issue in these multi-hop networks is that more hops in a path equates to a lower throughput. This is a fundamental issue with radio: each link needs to share the radio spectrum in a way that only one send can happen at a time. Otherwise, interference will ruin a signal. This wasn't a problem previously because wires have shielding that limits interference. Many links are asymmetric, where the delivery path in one direction may have better performance or reliability than the reverse.

What are some metrics we can use to determine which path to choose? 
![[Pasted image 20251124012339.png]]
As an example, consider this network.

The first strawman metric could be to maximize bottleneck throughput. A has two paths to choose to get to C: A-B-C with a bottleneck throughput of 50%, or A-D-C with a bottleneck throughput of 51%. It seems like the bottom path is better, but the actual throughputs are 33% and 25%, respectively.

Maybe we could just maximize end-to-end delivery ratios?
![[Pasted image 20251124012525.png]]
In this graph it would seem like A-B-C is the best path with its delivery ratio of 51%, but the actual throughputs of the links are 33% and 50% for the top and bottom paths respectively.

A better metric to use instead is ETX - Expected Transmission Count. Srcr, which manages routing in Roofnet, will use an extension called ETT (Expected Transmission time), but let's first look at how ETX works.
![[Pasted image 20251124012820.png]]
Consider these three delivery ratios. On the right is what an example link might look like. The Link ETX of these are 1,2, and 3 respectively. This should make sense: if there is a probability x of a packet reaching its destination, then it would take 1/x packets on average to successfully send it.

More formally, Link ETX is derived by the following:
Pr(Tx success) = P(Data success) * P(ACK success) \[The transmission success depends on successfully send the data AND receiving an ACK].
Link ETX = 1 / Pr(Tx success) = 1/(P(Data success) * P(ACK success))

To estimate it in this setting, one can measure the delivery ratios of forward and reverse $r_{fwd}, r_{rev}$ and substitute for the probabilities. The total ETX of a Route is then the sum of all individual ETX of each link.

ETX works great for short routes, and it can quantify properties such as loss, asymmetry, and the throughput reduction of long routes. However, it still has a few limitations:
- ETX assumes all links run on the same bitrate, which isn't always true in a wireless setting.
- ETX also fixes probe sizes to 134 bytes which can speeds of sending due to variable rates.

ETT extends the ideas of ETX by taking into account both delivery rates and the time it takes to transmit packets, factoring in variable bitrates. For each link, additionally multiply the ETX metric by the time it takes to send a packet. The route ETT is the same as ETX: a sum of each link's ETT.