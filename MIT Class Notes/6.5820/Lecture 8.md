Previous lectures presented multiple algorithms to address congestion control, with the math used to explain why and how it deals with congestion. This lecture aims to reframe congestion control in a general way. This makes it more systematic to create and analyze algorithms. It's an interesting field, containing an overlap of many different fields.

# The TCP Problem
What problem does TCP actually solve? How does one frame congestion control in a general sense? Currently, we have the following problem setup.
#drawing 3 flows thru 1 bottleneck.
By max-min fairness, these flows should all be equal to $\frac{1}{3}$Mbps. Different flows, of course, have different optimal flows, and it becomes increasingly difficult to do analysis when the topology of the network becomes complex like this:
#drawing oh god what is that network

In the 1990s, a paper was published that proved TCP is really solving an optimization problem. This discovery led to a huge research program on using techniques to solve these kinds of problems to analyze and design new network protocols.

# Network Utility Maximum (NUM)
First, let's define a utility function U(x).
#drawing utility function
U(x) represents the "benefit" from sending packets at the rate x. It is assumed to be increasing and concave. There are many functions which fit this definition, such as $\sqrt{x},\log(x),-\frac{1}{x}$. This assumptions turns out to be a good model for any "elastic traffic", such as file downloads.

With this, we can revisit a network optimization problem and see how using different utility functions changes the results:
#drawing

Problem: Maximize$$\log x_{1}+\log x_{2}+\log x_{3}$$subject to constraints 
$$x_{1}+x_{2} \leq 1$$
$$x_{1}+x_{3} \leq 1$$
It might be a better to rewrite the constraints as a matrix-vector inequality:
$$\begin{bmatrix}
1 & 1 & 0 \\
1 & 0 & 1
\end{bmatrix}
\begin{bmatrix}
x_{1} \\
x_{2} \\
x_{3}
\end{bmatrix}
\leq \begin{bmatrix}
1 \\
1
\end{bmatrix}
$$
More generally, the NUM problem looks like this:

Given N flows and L links, maximize $$\sum_{i=1}^{N}U_{i}(x_{i})$$ Subject to the constraints
#drawing constraints i aint writing that in latex hell no
