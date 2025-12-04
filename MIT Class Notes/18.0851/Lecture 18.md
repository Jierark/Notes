Recall the Poisson equation from [[MIT Class Notes/18.0851/Lecture 16|Lecture 16]] . As a refresher, the problem is the following:
$$-\Delta u(x,y) = f(x,y)$$
where $\Delta u = \frac{\delta u}{\delta x^2} + \frac{\delta u}{\delta y^2}$. We will consider Dirichlet boundaries on a unit square:
$$u(x,y) = 0, (x,y) \in \delta[0,1]^2$$
(The function u is 0 on the edges of the square)

Previously, we discretized the problem into a 3x3 grid, ignoring the boundary points:
![[Pasted image 20251202155128.png]]As before, a 2D Finite difference scheme around the point $u_{i,j}$ is expressed as
$$\frac{1}{h^2}(-u_{i-1,j} - u_{i+1,j} +4u_{i,j} - u_{i,j-1} - u_{i,j+1})$$
The resulting system of $K_{2D}U$ take the following form:
$$K_{2D} = \frac{1}{h^2}\begin{bmatrix}{}
4 & -1 & 0 \mspace{25mu} \vert & -1 & 0 & 0 & \mspace{10mu} \vert &  0 & 0 & 0 \\
-1 & 4 & -1 \mspace{10mu} \vert& 0 & -1 & 0 & \mspace{10mu} \vert & 0 & 0 & 0 \\
0 & -1 & 4 \mspace{25mu} \vert& 0 & 0 & -1 & \mspace{10mu} \vert & 0 & 0 & 0 \\ \hline 
-1 & 0 & 0 \mspace{25mu} \vert & 4 & -1 & 0 &  \mspace{10mu} \vert &  -1 & 0 & 0 \\
0 & -1 & 0 \mspace{25mu} \vert& -1 & 4 & -1 & \mspace{10mu} \vert & 0 & -1 & 0 \\
0 & 0 & -1 \mspace{10mu} \vert& 0 & -1 & 4 & \mspace{10mu} \vert & 0 & 0 & -1 \\
\hline 
0 & 0 & 0 \mspace{25mu} \vert & -1 & 0 & 0 & \mspace{10mu} \vert &  4 & -1 & 0 \\
0 & 0 & 0 \mspace{25mu} \vert& 0 & -1 & 0 & \mspace{10mu} \vert & -1 & 4 & -1 \\
0 & 0 & 0 \mspace{25mu} \vert& 0 & 0 & -1 & \mspace{10mu} \vert & 0 & -1 & 4 \\
\end{bmatrix}$$
$$U = \begin{bmatrix}
u_{1,1} \\
u_{1,2} \\
u_{1,3} \\
\hline u_{2,1} \\
u_{2,2} \\
u_{2,3} \\
\hline u_{3,1} \\
u_{3,2} \\
u_{3,3}
\end{bmatrix}$$
(I apologize for the poor formatting. This is the best I could get working)
As you can see, this is quite a different shape than in the 1D case. The unusual placement of entries is due to how the $U$ vector has to be formed to fit a 2D coordinate space into a 1D vector.

We can actually split the $K_{2D}$ matrix into two matrices:
$$K_{2D} = \frac{1}{h^2}\left( \begin{bmatrix}{}
2 & -1 & 0 \mspace{25mu} \vert & 0 & 0 & 0 & \mspace{10mu} \vert &  0 & 0 & 0 \\
-1 & 2 & -1 \mspace{10mu} \vert& 0 & 0 & 0 & \mspace{10mu} \vert & 0 & 0 & 0 \\
0 & -1 & 2 \mspace{25mu} \vert& 0 & 0 & 0 & \mspace{10mu} \vert & 0 & 0 & 0 \\ \hline 
0 & 0 & 0 \mspace{25mu} \vert & 2 & -1 & 0 &  \mspace{10mu} \vert &  0 & 0 & 0 \\
0 & 0 & 0 \mspace{25mu} \vert& -1 & 2 & -1 & \mspace{10mu} \vert & 0 & 0 & 0 \\
0 & 0 & 0 \mspace{25mu} \vert& 0 & -1 & 2 & \mspace{10mu} \vert & 0 & 0 & 0 \\
\hline 
0 & 0 & 0 \mspace{25mu} \vert & 0 & 0 & 0 & \mspace{10mu} \vert &  2 & -1 & 0 \\
0 & 0 & 0 \mspace{25mu} \vert& 0 & 0 & 0 & \mspace{10mu} \vert & -1 & 2 & -1 \\
0 & 0 & 0 \mspace{25mu} \vert& 0 & 0 & 0 & \mspace{10mu} \vert & 0 & -1 & 2 \\
\end{bmatrix} + \begin{bmatrix}
2I & -I & 0 \\
-I & 2I & -I \\
0 & -I & 2I
\end{bmatrix}
\right)$$
Let's use this breakdown to help understand why the $K_{2D}$ matrix corresponds to the correct partials.

On the left, you have the familiar finite difference scheme in 1D. Mapping the row index i to x, and the column index j to y, we can see that the left corresponds to $\frac{\delta}{\delta y^2}$ since the column index varies across a row. The matrix on the right will correspond to $\frac{\delta}{\delta x^2}$.

# Kronecker Products
Given two matrices A,B, the Kronecker Product is defined as 
$$A \otimes B = \begin{bmatrix}
A_{1,1}B & A_{1,2}B  & \dots & A_{1,n}B \\
A_{2,1}B & \dots & \dots & \dots \\
\dots & \dots & \dots & \dots \\
A_{n,1}B & \dots & \dots & A_{n,n}B
\end{bmatrix}$$
A specifies the constant, while B is the matrix that gets inserted into the resulting matrix.

This is useful, as we can express $K_{2D}$ as a Kronecker Product of two matrices. Going back to how we broke down $K_{2D}$,

On the left, you have the $K_{3}$ matrix replicated on the pattern of the Identity matrix. We express this as $I \otimes K$.
On the right, you have the Identity Matrix replicated on the pattern of $K_{3}$. This corresponds to $K\otimes I$.
Then, $$K_{2D} = I \otimes K + K \otimes I$$
This is a useful property, as one can prove the following property:
Given $A\vec{v} = \lambda_{1}\vec{v}$ and $B\vec{w} = \lambda_{2}\vec{w}$, then $(A\otimes B)(\vec{v}\otimes \vec{w}) = (\lambda_{1}\lambda_{2})(\vec{v}\otimes \vec{w})$.

# Slow Solver
Suppose that $N = 10^3$. then $K_{2D} \in \mathbb{R}^{n^2\times n^2}$ is a very large matrix, but sparse.
![[Pasted image 20251202163659.png]]
The distance between the diagonal and the off diagonals is n. Storing the $K_{2D}$ matrix will take $O(n^2)$ memory.

The most obvious way to solve this system is LU decomposition: $K_{2D}=LU=LDL^T$. Let's think about what happens at each step of Gaussian elimination:
- Each step needs to make 2 elements 0 below the band.
- However, each step will also create nonzero entries above the band as a result.
- This will eventually create a larger band of nonzero values.
This results in $O(N^3)$ memory complexity ($N^2$ rows and $N$ new nonzero elements per row). The computation complexity will then become $O(N^4)$.

This problem is still an active area of research. Methods such as nested dissection or sparse LU have managed to reduce these complexities to $O(n^2 \log n)$ in memory and $O(n^3)$, which is a marked improvement. Can we do better?

# Fast Poisson Solver
The fast Poisson solver improves complexities to
- $O(n^2)$ in memory
- $O(n^2 \log n)$ in operations
by using Fourier Series.

We know that $K_{2D}$ can be decomposed using its eigenvalues and eigenvectors as
$$K_{2D} = S \Lambda S^T$$
Then, $$K_{2D}^{-1} = S \Lambda^{-1}S^T$$
We can then solve $K_{2D}U=F \to U = K_{2D}^{-1}F = S \Lambda^{-1}S^TF$

The steps of the solver are as follows:
1) Expand F in the basis of the eigenvectors of K: $c_{k}=\vec{y}_{k}F$ where $k=1\dots n^2$
2) Scale coefficients by the eigenvalues: $d_{k}=\frac{c_{k}}{\lambda_{k}}$
3) Reassemble the vector $U$ as $\sum_{k}d_{k}\vec{y}_{k}$
The second step will take $O(n^2)$ as it is simply a multiplication.

Recall that the eigenvectors of the $K$ matrix were discrete sines. Then steps (1) and (3) are analogous to Fourier and Inverse Fourier Transforms, respectively. Then, we can do these steps quickly using the Fast Fourier Transform. The steps can be rewritten as follows:
1) Use the Fast Fourier Transform* on F to get coefficients $c_{k}=\vec{y_{k}}F$
2) Scale each coefficient by $\frac{1}{\lambda_{k}}$
3) Use the inverse Fast Fourier Transform to derive U.
\*due to boundary conditions, this is a slightly modified FFT to obey those conditions.
