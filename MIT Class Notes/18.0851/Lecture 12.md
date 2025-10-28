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

Let's try a slightly different approach than before. We can eliminate the vector e using the following
$$w = Ce \to e = C^{-1}w$$
Then we can rewrite $e = b - Au$ as the following equation:
$$C^{-1}+Au = b$$
If we include the equation $A^Tw=f$, then we can convert the system into a matrix-vector equation:
$$\begin{bmatrix}
C^{-1} & A \\
A^T & 0
\end{bmatrix}\begin{bmatrix}
w \\
u
\end{bmatrix} = \begin{bmatrix}
b \\
f
\end{bmatrix}$$
This is called Block Decomposition. Notice that the matrix is symmetric, meaning the eigenvalues are real. However, it is indefinite and we cannot make any conclusions about the eigenvalues. Regardless, this is a useful method for determining the stability of a system.

We can perform gaussian elimination on this matrix as well. From row 2, subtract $A^TC$ (row 1) to get
$$\begin{bmatrix}
C^{-1} & A \\
0 & -A^TCA
\end{bmatrix}
\begin{bmatrix}
w \\
u
\end{bmatrix}
=\begin{bmatrix}
b \\
f-A^TCb
\end{bmatrix}$$
The second equation reads $-A^TCAu=f - A^TCb$ and we can arrive at the same solution as before.

# Trusses
Consider the following structure (known as a truss):
#drawing 
We now operate in 2D, so displacements have both a horizontal and vertical component. Each edge represents a bar, while a node represents a pin joint: centers of rotations for the bars it connects.

We fix the structure by defining nodes 4 and 5 to have 0 displacement. There are 5 edges and 3 nodes which can move, and we are interested in measuring displacement of nodes. The number of unknowns is 3 * 2:
$$u=\begin{bmatrix}
u_{1}^H \\
u_{1}^V \\
u_{2}^H \\
u_{2}^V \\
u_{3}^H \\
u_{3}^V \\
\end{bmatrix}$$
How do we measure these displacements? We know that bars can rotate around each joint. This implies a rotation by some angle $\theta$, like this example in node 2:
#drawing 
Suppose the bars are all equal lengths L. Then,
$$u_{2}^H = L \sin \theta$$
$$u_{2}^V = -L(1-\cos \theta)$$
Apply this to all nodes. Then we get
$$u=\begin{bmatrix}
L \sin \theta \\
-L(1-\cos \theta) \\
L \sin \theta \\
-L(1-\cos \theta) \\
L \sin \theta \\
-L(1-\cos \theta) 
\end{bmatrix}$$
The We can go further, though. The Taylor expansions for sine and cosine are the following:
$$\sin \theta \approx \theta$$
$$\cos \theta \approx 1 - \frac{\theta^2}{2}$$
We are interested in looking at small changes to $\theta$. Therefore, we can use these approximations.
$$u\approx \frac{L\theta}{\epsilon}\begin{bmatrix}
1 \\
0 \\
1 \\
0 \\
1 \\
0
\end{bmatrix} + O(\epsilon^2)$$
We can ignore the error term, as it is negligent.

We can apply the same framework from before to solve for our displacements, defining the matrix A and C to go from displacements $u \to$elongations $e \to$ internal force $w \to$ forces $f$.

How do we construct the matrix A? Firstly, we can deduce that the columns of A will represent the different displacements, and the rows of A will correspond to the elongation of the bars. The dimension of A is then 5x6.

Let's consider this simple example:
#drawing 
The elongation, horizontally, is defined as $$e=u_{2}^H-u_{1}^H$$
We can ignore the vertical component because it scales quadratically in angle, which we are taking to be small. For a first order approximation, this is fine to ignore. These arguments would be the same for a perfectly vertical bar, just flipped in dimensions.

For an arbitrary angle $\phi$
#drawing 
The elongations are defined as a dot product:
$$e = (\vec{u_{2}}-\vec{u_{1}})\cdot  \hat{e_{\phi}}$$ where $\hat{e_{\phi}} = \begin{bmatrix}\cos \phi \\ \sin \phi\end{bmatrix}$
Then the equation $Au=e$ looks like the following for A, u:
Horizontal:
$$Au = \begin{bmatrix}
-1 & 0 & 1 & 0
\end{bmatrix}\begin{bmatrix}
u_{1}^H \\
u_{1}^V \\
u_{2}^H \\
u_{2}^V \\
\end{bmatrix}$$
Vertical:
$$Au = \begin{bmatrix}
0 & -1 & 0 & 1 \\
\end{bmatrix}\begin{bmatrix}
u_{1}^H \\
u_{1}^V \\
u_{2}^H \\
u_{2}^V \\
\end{bmatrix}$$
Arbitrary:
$$Au = \begin{bmatrix}
-\cos \phi & -\sin \phi & \cos \phi & \sin \phi
\end{bmatrix}\begin{bmatrix}
u_{1}^H \\
u_{1}^V \\
u_{2}^H \\
u_{2}^V \\
\end{bmatrix}$$
Substituting $\phi=0,\phi=\frac{\pi}{2}$ will get you the original results for horizontal and vertical, respectively.