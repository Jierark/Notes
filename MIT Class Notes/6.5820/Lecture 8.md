Previous lectures presented multiple algorithms to address congestion control, with the math used to explain why and how it deals with congestion. This lecture aims to reframe congestion control in a general way. This makes it more systematic to create and analyze algorithms. It's an interesting field, containing an overlap of many different fields.

# The TCP Problem
What problem does TCP actually solve? How does one frame congestion control in a general sense? Currently, we have the following problem setup.
![[Pasted image 20251008163156.png]]
By max-min fairness, these flows should all be equal to $\frac{1}{3}$Mbps. Different flows, of course, have different optimal flows, and it becomes increasingly difficult to do analysis when the topology of the network becomes complex like this:
![[Pasted image 20251008163247.png]]

In the 1990s, a paper was published that proved TCP is really solving an optimization problem. This discovery led to a huge research program on using techniques to solve these kinds of problems to analyze and design new network protocols.

# Network Utility Maximum (NUM)
First, let's define a utility function U(x).
![[Pasted image 20251008163457.png]]
U(x) represents the "benefit" from sending packets at the rate x. It is assumed to be increasing and concave. There are many functions which fit this definition, such as $\sqrt{x},\log(x),-\frac{1}{x}$. This assumptions turns out to be a good model for any "elastic traffic", such as file downloads.

With this, we can revisit a network optimization problem and see how using different utility functions changes the results:


For example, maximize$$\log x_{1}+\log x_{2}+\log x_{3}$$subject to constraints 
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

Given N flows and L links, maximize $$\sum_{i=1}^{N}U_{i}(x_{i})$$ Subject to these constraints
![[Pasted image 20251008163535.png]]
Now, consider the following flows:
![[Pasted image 20251008163703.png]]
The first utility function we could optimize is Throughput Maximization:
- Maximize $x_{1}+x_{2}+x_{3}$ 
- Subject to constraints 
	- $x_{1}+x_{2} \leq 1$
	- $x_{1}+x_{3} \leq 1$
- The answer is to give all the flow of the bottlenecks to $x_{2},x_{3}$ and none to $x_{1}$, so
	- $x_{1} = 0$
	- $x_{2} = 1$
	- $x_{3}=1$
A max-min allocation would give each flow $\frac{1}{2}$.

If the utility function is a logarithm, then it is considered proportional fairness:
- Maximize $\log x_{1}+\log x_{2}+\log x_{3}$
- Subject to the same constraints
- The key idea with solving this is that the bounds are tight, so you can express the other variables in terms of $x_{1}$. With some substitution and a derivative, one can find the solution to the optimal.
- The answer becomes
	- $x_{1}=\frac{1}{3}$
	- $x_{2}=\frac{2}{3}$
	- $x_{3}=\frac{2}{3}$

A general case for fairness is $\alpha$-fairness:
- Maximize $\sum_{i=1}^3 \frac{x_{i}^{1-\alpha}}{1-\alpha}$ where $\alpha\geq 0$
- Same constraints as before.
- When $\alpha=0$ the optimal allocation is identical to throughput maximization
- As $\alpha \to 1$, the optimal allocation approaches proportional fairness
- As $\alpha \to \infty$, the optimal allocation approaches max-min fairness.

# Distributed Algorithm for NUM
For now, we are going to use the same problem as before, using proportional fairness.
How do we come up with a distributed algorithm, where each piece of the system makes a local decision, and reach optimal fairness?

Let's say that there is a "congestion price" $p_{l}$ associated with using link l. It charges some price per unit of bandwidth. A source i then accumulates a total price $q_{i}$ depending on how much bandwidth it uses at each bottleneck.

Links can dynamically adjust their prices periodically, based on how much congestion they observe:
$$p_{l}(t+1) = \max(p_{l}(t) + \kappa(y_{l}(t)-c_{l}), 0)$$
This is a similar structure to congestion control algorithms such as PIE and XCP. This quantity is computed independently at each link.

Each source then picks a rate $x_{i}$ to maximize the utility function $\log x_{i} - q_{i}x_{i}$, where $x_{i}=\frac{1}{q_{i}}$. Each source does this independently from the others.

Put together, the problem setup looks like this:
![[Pasted image 20251008171324.png]]

# TCP's NUM Problem
The formulation for TCP looks like this:
![[Pasted image 20251008172302.png]]
What utility function is it optimizing for? Roughly $\frac{1}{\sqrt{x }}$.

Some other optimization objectives are
- Minimal flow completion time (either average or tail)
- Minimal coflow completion time
- Maximal Power
- Maximal application-level utility.