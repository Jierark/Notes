Last time, we looked at the least squares approximation of a graph of a series of points. There's a few more interesting notes to discuss.

Let's look at the problem setup again.
![[Pasted image 20251022174403.png]]
As usual, we want to minimize the error term, but we're going to introduce the term $\sigma$ which represents variance or uncertainty.

The objective function changes; the goal is to minimize the following expression:
$$\frac{e_{1}^2}{2\sigma_{1}^2} + \frac{e_{2}^2}{2\sigma_{2}^2}+\dots+\frac{e_{6}^2}{2\sigma_{6}^2}$$
We redefine it in the following way:
$$\vec{e}=\begin{bmatrix}
e_{1} \\
\dots \\
e_{6}
\end{bmatrix}$$
The above equation is rewritten as $\vec{e}^TC\vec{e}$ where C = $\begin{bmatrix} \frac{1}{\sigma_{1}^2}  &   & \\ & \dots  &  \\  &  & \frac{1}{\sigma_{6}^2}\end{bmatrix}$

This is known as weighted least squares. The matrix C is the inverse of the covariance matrix
$$\Sigma=\begin{bmatrix} \sigma_{1}^2  &   & \\ & \dots  &  \\  &  & \sigma_{6}^2\end{bmatrix}$$
Let's take another look at how we arrived at the approximation.
- We were interested in the vector $\vec{u}$ in the equation $A\vec{u}=\vec{b}$
- We then used the matrix $A$ to define a quantity $\vec{e}=A\vec{u}-\vec{b}$.
- We apply the matrix C to this vector to get the equation $\vec{w}=C\vec{e}$
- We then apply the matrix $A^T$ to get the result $A\vec{w}=\vec{0}$.

This is the same framework we saw previously when we were studying boundary condition systems!

The final equation is then
$$A^TC(A\vec{u}-\vec{b})=\vec{0}$$
$$A^TCA\vec{u}=A^TC\vec{b}$$
If $C = I$, then we arrive at our original solution:
$$u=(A^TA)^{-1}A^T\vec{b}$$The matrix $(A^TA)^{-1}A^T$ is known as the pseudo inverse of $A$, denoted as $A^+$. If A is a square and invertible matrix, then this will reduce back to the inverse.

# Graphs
![[Pasted image 20251022174421.png]]
A graph is a series of edges and nodes. We are only going to look at directed edges for this lecture. How can we represent this graph as a matrix?

Define the matrix A as the Incidence matrix. Each row represents an edge, and each column represents the corresponding nodes. Each value in the matrix is -1 if the corresponding node is the source of the edge, +1 if it is the destination, and 0 if there are no edges. In this example, the matrix A looks like this:
$$A=\begin{bmatrix}
-1 & 1 & 0 & 0 \\
-1 & 0 & 1 & 0 \\
0 & -1 & 1 & 0 \\
0 & -1 & 0 & 1 \\
0 & 0 & -1 & 1
\end{bmatrix}$$
Notice that this matrix has more rows than columns. $A$ will have a nonzero nullspace, as the vector $\begin{bmatrix}1 \\ 1 \\ 1 \\ 1\end{bmatrix}$ exists in its nullspace. 

From rank-nullity theorem, we know some more information about this matrix:
The # of columns (the rank) is equal to the dimension of the column space plus the dimension of the null space.  The dimension of the null space is 1, so the column space has 3 dimensions. We can also draw some other conclusions based on this:
- Three of the columns are linearly independent
- The rank of A is 3
- A also has three non-zero pivots

Define the matrix K as $A^TA$.
$$K = \begin{bmatrix}
-1 & -1 & 0 & 0 & 0 \\
1 & 0 & -1 & -1 & 0 \\
0 & 1 & 1 & 0 & -1 \\
0 & 0 & 0 & 1 & 1
\end{bmatrix}\begin{bmatrix}
-1 & 1 & 0 & 0 \\
-1 & 0 & 1 & 0 \\
0 & -1 & 1 & 0 \\
0 & -1 & 0 & 1 \\
0 & 0 & -1 & 1
\end{bmatrix} = \begin{bmatrix}
2 & -1 & -1  & 0 \\
-1 & 3 & -1 & -1 \\
-1 & -1 & 3 & -1 \\
0 & -1 & -1 & 2
\end{bmatrix}$$
The matrix K is known as a graph Laplacian. It maps nodes to nodes, and it encodes additional information about the graph:
- The diagonal represents the number of nodes that a node is connected to, also known as it's degree.
- Elements with a -1 represent a connection between nodes.
- Elements with a 0 mean there is no edge.
Another way to derive K: $K = D - W$, where D is a matrix which represents the degrees of the nodes, and W is the adjacency matrix of the graph.

# Electrical Networks
Consider the following electrical network:
![[Pasted image 20251022174518.png]]

A is defined in the same way as before.
Define the following quantities:
$u_{i}$ = potentials/voltages at nodes
$e_{i}$ = voltage drops/potential differences at edges
$c_{i}$ = conductivities on edges
$w_i$ = currents on edges

To go from potentials to voltage drops, we multiply by the matrix A and get $\vec{e} = \vec{b}-A\vec{u}$. $\vec{b}$ will represent the battery on edge 4. We will see the convention of signs later.

To go from voltage drops to currents, we use Ohm's Law. Conductivities are measured in $\Omega^{-1}$, so keep this in mind. The diagonal matrix C is composed of the conductivities of each resistor.
$$C=\begin{bmatrix}
c_{1} & 0 & 0 & 0 & 0 \\
0 & c_{2} & 0 & 0 & 0 \\
0 & 0 & c_{3} & 0 & 0 \\
0 & 0 & 0 & c_{4} & 0 \\
0 & 0 & 0 & 0 & c_{5}
\end{bmatrix}$$
Finally, we can relate these to the current source $f$ with the matrix $A^T$
$$A^Tw=f$$
Put together, we get
$$A^Tw=A^TCe$$
$$=A^TC(\vec{b} - A\vec{u}) = \vec{f}$$
$$A^TCA\vec{u}=\vec{f}-A^TC\vec{b}$$
Given the current source $f$ and the battery, the goal is to find $u_i$, the voltages at each node. The nullspace has a special meaning in each matrix: #todo watch recording when canvas isn't messed up. We can eliminate this meaning by grounding one of the nodes (setting its voltage to 0).

In this example, set $u_4=0$ and set the conductivities to 1. Then 
$$K\vec{u}=\begin{bmatrix}
2 & -1 & -1 & 0 \\
-1 & 3 & -1 & -1 \\
-1 & -1 & 3 & -1 \\
0 & -1 & -1 & 2
\end{bmatrix}\begin{bmatrix}
u_{1} \\
u_{2} \\
u_{3} \\
u_{4}
\end{bmatrix} = \vec{f} - A^T\vec{b}$$
By setting $u_{4}=0$, we can remove the last column and row of the K matrix, and the associated dimension in the right hand side. The remaining matrix of K is invertible.

Consider $A^Tw$.
The dimension of the column space of $A^T$ is the same as $A$: 3. The null space of thus of dimension 2. One of these vectors is $\begin{bmatrix}1 \\ -1 \\ 1 \\ 0 \\ 0\end{bmatrix}$. If you assign these currents accordingly, you can see the resulting loop current. The one vector in the null space will correspond to the other possible loop in the bottom.

An additional vector in the null space is the sum of these two vectors, which creates another loop current around the entire electrical network. 

The conclusion then is that the null space corresponds to possible loop currents. However, these are not solutions to the system because the source of energy will reverse a current direction.