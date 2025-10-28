Last time, we started analyzing trusses. As before, here's the truss we will be looking at.
#drawing 

For a single bar, recall the elongations as the following:
#drawing 
(By convention, angles are measured counterclockwise, so this angle would be negative)

 For a single bar, we arrived at the conclusion that
 $$A=\begin{bmatrix}
-\cos \phi & -\sin \phi & \cos \phi & \sin \phi
\end{bmatrix}$$
(does it matter which how nodes get assigned? Ask during class)
Apply this to the truss we were looking at previously. Angles are defined as such:
#drawing 
The construction of A becomes the following. This part can be easy to mess up, so take it slowly and go bar by bar:
$$A=\begin{bmatrix}
\frac{1}{\sqrt{2}} & \frac{1}{\sqrt{ 2 }} & -\frac{1}{\sqrt{ 2 }} & -\frac{1}{\sqrt{ 2 }} & 0 & 0 \\
-\frac{1}{\sqrt{2}} & \frac{1}{\sqrt{2}} & 0 & 0 & \frac{1}{\sqrt{2}} & -\frac{1}{\sqrt{2}} \\
0 & 0 & -1 & 0 & 1 & 0 \\
0 & 0 & 0 & 1 & 0 & 0 \\
0 & 0 & 0 & 0 & 0 & 1 \\
\end{bmatrix}$$
What does the null space of this matrix represent? A vector x such that $Ax=0$?

This "mechanism" represents displacements which cause no elongation. In this example, the vector $$\begin{bmatrix}1 \\ 0 \\ 1 \\ 0 \\ 1 \\ 0\end{bmatrix}$$is in the null space of A. These represent horizontal displacements of nodes 1,2,3 in the same direction. However, this truss will eventually collapse since nodes 4 and 5 are fixed, so this is not a stable structure. How can we address this issue?

We can simply add a row of A that is not linearly dependent on the other entries. Physically, we need to add another bar between nodes that isn't redundant. A bar from 2 to 5 (or 3 to 4) will work for this example:
#drawing 
Then A has no null space and no mechanisms.

Now consider this structure:
#drawing 

There are 6 bars and 8 unknowns. Our system is underspecified, so we have 2 possible mechanisms (recall rank-nullity theorem of a matrix). The mechanisms are:
- Move the top 2 nodes horizontally
- Move the middle 2 nodes horizontally
Moving both also works since it's just a linear combination. We add 2 bars as such:
#drawing 
Fun fact: this is how cranes need to be built and be stable when lifting extremely heavy objects.

#drawing 
This construction will not work, as the second bar from 2 to 3 is redundant. In linear algebra terms, this row will be linearly dependent on the others.

What if we did not have a fixed system?
#drawing 
The mechanisms for this would be
- Horizontal displacement
- Vertical displacement
- Rotational displacement

#drawing 
This system has 2 mechanisms. Can you spot them?

Let's go back to the framework we've been working with. There's two more interesting notes about this:
- We could divide the framework between interactions at nodes and interactions at edges, like such.
- #drawing 
- This framework is an extremely common framework (and we've seen a few uses of it) so it's nice to have memorized.

Suppose we have a simple system like this:
#drawing 
This is a "Determinate system" and the solutions can be shortcut:
$$A^Tw=f$$
$$w=(A^T)^{-1}f$$ only because $K^{-1} = A^{-1}C^{-1}(A^T)^{-1}$
This system is indeterminate:
#drawing 
Because we don't know which bar carries the most load and is dependent on the values of c.

Recall that $K=A^TCA$. What does this look like from a matrix perspective?
#drawing 
$$K=c_{1}(row_{1})^T(row_{1}) + c_{2}(row_{2})^T(row_{2})+\dots$$
K can be interpreted as the accumulation of each bar, each one represented by a matrix. Each one of these is known as an element matrix. This will be important for finite elements.