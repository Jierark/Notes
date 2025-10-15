How do we evaluate new ideas in Networking? The gold standard is a Randomized Control Trial (RCT), where clients at random use a new idea without knowing. Unfortunately, RCT is difficult to actually achieve.
- RCTs take a lot of time and resources to do correctly.
- It requires a working implementation of the new algorithm, which might be hard to get on the first go.
	- As a result, it is hard to iteratively improve the algorithm.
Our next best option then is to do simulation - where we make a model of reality. Simulations are the most common method for research, and they are flexible and easy to repeat and control. 
Simulations are commonly seen in AI driven systems. Reinforcement Learning agents and similar learn by exploring their surroundings, often in a "playground" or simulation. Unfortunately, there is a large gap that needs to be kept in mind.

### Sim2Real Gap
This is a well studied problem in fields using simulations, mostly in robotics. The gaps that simulations miss in reality often lead to poorer performance in reality. This also comes up in many ML contexts: ever notice how a validation set or real deployment has worse performance than in training?

# Simulator Types
Full System simulations are the most comprehensive, allowing one to model all components of a network from switches to hosts and other devices. Tools such as mininet or ns3 are great at this. However, it comes with some drawbacks:
- It requires detailed knowledge of the network you want to simulate
- They are tedious to build, rendering them infeasible for larger experiments.

The easier simulation we have are trace-driven simulators. These consist of a trace of packets that can be analyzed and tested on for new network ideas. They are less complex, but often can be less accurate.

Could we do better than traces? Well, not right now. Packet-level simulation is the most common form because it's at the limit of what we can feasible simulate. Larger ideas like flows, applications, clouds, or even the whole Internet are currently too difficult to simulate and we have very little ideas on how to actually do it.

There are a ton of libraries to do packet-level simulation. Unfortunately, it is too low level for many questions, but it's the best we have because we don't have accurate models of real networks. Now, it's possible we could use ML to learn accurate, high level network models from data (we have a lot of data), but that's currently an open research question. We will see one possible method in this lecture!

# CasualSim
Causal Sim works on trace-driven simulation. What does this mean?

Let's suppose I have a component that I want to test. The rest of the system is abstracted away and collected as a network trace. Later, when we want to test the component, we replay those traces to mimic the behavior of the rest of the system and then see what happens. However, this relies on an assumption known as  **Exogenous Trace Assumption** - simulated interventions (that is, the changes we make) would not affect the replayed trace. However, in practice, it turns out this does not hold for many real world traces.
- Congestion Control Protocols will change depending on how the intervention might send packets differently.
- Even application level behavior can be changed by different network characteristics.
This hurts accuracy and can lead to completely wrong conclusions.

CausalSim focuses on the adaptive bitrate algorithms from previous lectures. The trace contains the throughput that users experienced during some streaming session. The changes that are interesting are to the ABR algorithm:
$$b_{t+1} = max(b_{t}-d_{t}, 0) + T$$
where 
- $b_{t}$ is the playback buffer in seconds
- $d_{t}$ is the time it takes to download a new chunk
- $T$ added to playback buffer at end of download
Let's suppose the new algorithm to test computes the download time as
$$\frac{a_{t}}{m_{t}}$$
where
- $a_{t}$ is the chunk size, chosen by the algorithm
- $m_{t}$ is the achieved throughput from the replayed trace.

This is what we will test in simulation. But where do we get our traces from?

## Puffer
Puffer was a Randomized Control Trial that ran between July 2020 and June 2021 in Stanford which captured network traces of streaming sessions. 5 ABR algorithms were used, and it contains 56 million downloaded chunks over 230,000 streaming sessions, totaling 3.5 years worth of streamed video.

The task in question: given the traces for all except one ABR algorithm, simulate the held out algorithm on the same paths.

Just to give an idea, this is what a typical trace might look like:
![[Pasted image 20251014213011.png]]
The task is to take this trace and replay it under a new algorithm (known as the target), trying to determine the achieved throughput and the playback buffer size, for instance:
![[Pasted image 20251014213144.png]]
In a standard trace simulator, the throughput of the network is assumed to be exogenous and stay the same. Then we only need to determine the bitrates and playback buffer sizes.

How accurate is trace-driven simulation?
![[Pasted image 20251014213605.png]]
In this case, it looks okay, but there is a noticeable gap in simulation vs ground truth.