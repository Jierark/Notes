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
A more realistic solution is to use variable rates. Define $R_{i}(t)$ as the instantaneous rate of user i. The average rate $R_{avg,i}$ is defined as $$\begin{cases}
\alpha R_{i}(t). + (1-\alpha)R_{avg}(t-1) & i^*=i; \\
(1-\alpha)R_{avg}(t-1) & \forall i\neq i^*
\end{cases}$$
The choice of $R_{i}$ determines the fairness factor. Max-min fairness is achieved when $R_{i}(t)$ is low. A higher choice of $R_i(t)$ maximizes throughput. Proportional fairness is achieved for $i^* = max_{i}\left( \frac{R_{i}(t)}{R_{avg,i}(t)} \right)$

# Roofnet
Roofnet started as a CSAIL Project in the 2000s, with the goal of creating a mesh network at scale. It aimed to achieve operation without central management, while maintaining a wide coverage and acceptable performance. Some of the nodes are placed around MIT campus.

Some of the key design decisions in Roofnet are:
- Unconstrained node placement.
- Omni-directional antennas
- Multi-hop routing - using both link-state and source routing
- ETT - a routing metric that optimizes for throughput in a slowly changing network. This comes as an extension from ET.
- SampleRate - a bit-rate selection method using throughput estimates from actual data delivery.