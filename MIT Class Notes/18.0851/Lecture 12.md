Consider the following simple electrical network:
#drawing 
The battery has an internal resistance, represented by the value r.

There are two nodes $u_1, u_2$ that we want to measure the potential. Left-multiplied by A, this becomes voltage drops across the edges $e_1, e_2$.
The incidence matrix A is equal to $\begin{bmatrix}-1 & 1 \\ -1 & 1\end{bmatrix}$. Recall that the edges are represented by the rows, and the columns correspond to the nodes. To deal with the nonzero nullspace, we'll ground the node $u_1=0$. We can also remove the corresponding column in the A matrix:
$$A_{grounded}=\begin{bmatrix}
1 \\
1
\end{bmatrix}$$
As an aside, the framework we use for these electrical networks can also be applied to other systems, such as fluid flows or heat transfer.

Some matrix/vectors we will need:
Voltages at nodes:$$\vec{u}=\begin{bmatrix}u_{1}=0 \\ u_{2}\end{bmatrix}$$
The battery: b = $$\begin{bmatrix}v \\ 0\end{bmatrix}$$
The matrix C, representing conductivities:
$$C = \begin{bmatrix}
\frac{1}{r} & 0 \\
0 & \frac{1}{R}
\end{bmatrix}$$
We apply the same framework that we are so familiar with:

Starting from the vector of potentials at nodes $\vec{u}$, we can get to the vector of potential drops across the edges $\vec{e} = b - Au$ Then, to get to the currents across edges $\vec{w}$, we use Ohm's law.
$$w_{2}=\frac{-u_{2}}{R}$$
Notice the negative sign. This means that the current flows in the opposite direction of the edge that we defined.
$$w_{1} = \frac{v-u_{2}}{r}$$
The voltage going through this circuit consists of the battery and the node at $u_{2}$. Note the absence of $u_{1}$, since it was grounded to 0. This is why we defined $e$ as $b-Au$ and not the other way around.
Aside: The direction of edges does not really matter in this problem. It will only change a sign in the currents, which means it is flowing in the opposite direction than you defined.

To get to potential current sources, we just need the matrix $A^T$:
$$A^Tw = \begin{bmatrix}
1 & 1
\end{bmatrix}
\begin{bmatrix}
\frac{v-u_{2}}{r} \\
\frac{-u_{2}}{R}
\end{bmatrix}=0$$
$$\frac{v-u_{2}}{r}-\frac{u_{2}}{R}=0$$
Solving for the potential at node 2,
$$\frac{v}{r}-u_{2}(\frac{1}{r} + \frac{1}{R})=0$$
$$u_{2}=\left( \frac{1}{r}+\frac{1}{R} \right)^{-1} \frac{V}{r}$$
$$u_{2}=\frac{R}{r+R}V$$
We can then use these to solve for the currents:
$$w_{2}=-\frac{V}{r+R}$$
$$w_{1}=\frac{V}{r} + \frac{V}{(r+R)r}$$
$$w_{1}=\frac{V}{r}\left( 1+\frac{r}{r+R} \right)$$

This framework we've been using generally applies to most linear systems. They lead to a system of the form 
$$Ku=f-A^TCb$$
Where $K=A^TCA$.

Let's try a slightly different approach than before.

