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

There are a ton of libraries to do packet-level simulation. Unfortunately, it is too low level for many questions, but it's the best we have because we don't have accurate models of real networks. Now, it's possible we could use ML to learn accurate, high level network models from data (we have a lot of data), but that's currently an open research question.

# CasualSim
