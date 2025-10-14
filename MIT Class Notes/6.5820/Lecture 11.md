How do we evaluate new ideas in Networking? The gold standard is a Randomized Control Trial (RCT), where clients at random use a new idea without knowing. Unfortunately, RCT is difficult to actually achieve.
- RCTs take a lot of time and resources to do correctly.
- It requires a working implementation of the new algorithm, which might be hard to get on the first go.
	- As a result, it is hard to iteratively improve the algorithm.
Our next best option then is to do simulation - where we make a model of reality. Simulations are the most common method for research, and they are flexible and easy to repeat and control. 
Simulations are commonly seen in AI driven systems. Reinforcement Learning agents and similar learn by exploring their surroundings, often in a "playground" or simulation. Unfortunately, there is a large gap that needs to be kept in mind.

### Sim2Real Gap
This is a well studied problem in fields using simulations, mostly in robotics. The gaps that simulations miss in reality often lead to poorer performance in reality. This also comes up in many ML contexts: ever notice how a validation set or real deployment has worse performance than in training?

# Simulator Types
