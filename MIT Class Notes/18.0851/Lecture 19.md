Previously, we looked at the Poisson Equation on a unit square. The Fourier Analysis framework we used generally applies to any Partial Differential Equation with constant coefficients and simple boundaries.

To recap last time, we were interested in the following problem
$$\begin{cases}
-\Delta u = f & x \in [0,1]^2 \\
u=0  & x \in \delta[0,1]^2 (\text{boundary condition}) \\
\end{cases}$$
We solved this using a finite difference scheme of the form
$$K_{2D}\vec{u} = \vec{f}$$
Previously, we saw the Fast Poisson Solver, which uses the fact that the eigenvectors of $K_{2D}$ are sines, allowing us to apply ideas of Fourier Series:
1) Expand $\vec{f}$ in the basis of eigenvectors, computing the corresponding coefficients as $c_{k}=\vec{y}^T\vec{f}$
2) Scale the coefficients: $c_{k} \leftarrow \frac{c_{k}}{\lambda_{k}}$
3) Reassemble $\vec{u} = \sum_{k} \frac{c_{k}}{\lambda_{k}}\vec{y}_{k}$
This is essentially doing $\vec{u} = K_{2D}^{-1}\vec{f}$ using the eigendecompoisition. We don't need to store the full matrix inverse because we can Fourier and Inverse Fourier Transforms for steps (1) and (3).

Let's look at bit closer at the eigendecomposition.

S = $\begin{bmatrix}\vec{y_{1}} \vert \vec{y_{2}} \vert\dots\vert \vec{y_{k}}\end{bmatrix}$ will be a discrete sine transform based on what we know about $K_{2D}$. Why is that?

Recall the following fact: $K_{2D} = K \otimes I + I \otimes K$. Can we use this fact to reason about the eigenvalues and eigenvectors?

First, we know that for the Identity matrix - it only has an eigenvalue of 1, and any vector is an eigenvector if it. With this, we can focus on the K matrix. Giving the following:
$$K\vec{v} = \lambda \vec{v}$$
$$K\vec{w} = \mu \vec{w}$$
Then, $\vec{y}=\vec{v}\otimes \vec{w}$ will be an eigenvector of $K_{2D}$. This vector has the following form:
$$\vec{y} = \begin{bmatrix}
v_{1}\vec{w} \\
\hline v_{2}\vec{w} \\
\hline \dots \\
\hline v_{n}\vec{w}
\end{bmatrix}$$
(A column vector of length $n^2$)

Using the Kronecker Product definition of $K_{2D}$, we can do the following
$$K_{2D}\vec{y} = \begin{bmatrix}
K_{1,1}I  & \dots & K_{1,n}I \\
\dots &  & \dots \\
K_{n,1}I & \dots & K_{n,n}I
\end{bmatrix}\begin{bmatrix}
v_{1}\vec{w} \\
\dots \\
v_{n}\vec{w} \\
\end{bmatrix} + 
\begin{bmatrix}
K & &   & \\
 & K &   & \\
 &  & \dots &  \\
 &  &  & K
\end{bmatrix}\begin{bmatrix}
v_{1}\vec{w} \\
\dots \\
v_{n}\vec{w} \\
\end{bmatrix}$$
Knowing that $\vec{v},\vec{w}$ are eigenvectors of $K$, how can we simplify this quantity?

On the left, we can simplify $K_{i,j}v_{i}\vec{w}$ as $\lambda \vec{w}$. Convince yourself this is true based on how $K_{i,j}v_{i}$ simplifies.

On the right, we can do the following:
$$Kv_{i}\vec{w} = K\vec{w}v_{i}=\mu v_{i}\vec{w}$$
With this, we simplify the equation as
$$K_{2D}\vec{y} = \lambda \begin{bmatrix}
v_{1}\vec{w} \\
\dots \\
v_{n}\vec{w}
\end{bmatrix} + \mu\begin{bmatrix}
v_{1}\vec{w} \\
\dots \\
v_{n}\vec{w} \\
\end{bmatrix}$$
For the matrix $K_{2D} = K \otimes I + I \otimes K$,
- It has an eigenvector of $\vec{v} \otimes \vec{w}$
- The associated eigenvalue is $\lambda + \mu$
More generally, the eigenvectors can be found as
$$\vec{y}_{k_{1},k_{2}} = \vec{v}_{k_{1}} \otimes \vec{v}_{k_{2}}$$
with an associated eigenvalue of
$$\lambda_{k_{1},k_{2}} = \lambda_{k_{1}} + \lambda_{k_{2}}$$
Note that there will be $n^2$ such eigenvectors

Let's look at a few concrete examples

Recall the K matrix
$$K = \begin{bmatrix}
2 & -1 & 0 & \dots \\
-1 & 2 & -1 & \dots \\
\dots & \dots & \dots & \dots \\
\dots & 0 & -1 & 2
\end{bmatrix}$$
The eigenvectors can be expressed as discrete sines:

The j-th component of the k-th eigenvector is the following:$v_{k}(j) = \sin\left( \frac{jk\pi}{N} \right)$
The Kronecker product of two such eigenvectors looks like this
![[Pasted image 20251203125327.png]]
This would represent how a square drum could vibrate.

Here's another example, showing how boundary conditions change the solution:

Consider the circulant matrix C:
$$C_{4} = \begin{bmatrix}
2 & -1 & 0 & -1 \\
-1 & 2 & -1 & 0 \\
0 & -1 & 2 & -1 \\
-1 & 0 & -1 & 2
\end{bmatrix}$$
It's eigenvectors will be complex exponentials with components
$$v_{k}(j) = e^{i 2 \pi j k/N} = \cos\left( \frac{2\pi jk}{N} \right) + i \sin\left( \frac{2\pi jk}{N} \right)$$
where $j=1,\dots,N, k = -\frac{N}{2}+1,\dots, \frac{N}{2}$
![[Pasted image 20251203125644.png]]

# Discrete Sine Transform
We can now go back and forth between the regular world and the Fourier Series world for discrete systems. It takes the following form:

Given components of a vector $f_{j}$, the associated Fourier Coefficients are calculated as the following:
$$c_{k}=\sum_{j=1}^N f_{j}\sin\left( \frac{jk\pi}{N} \right)$$
And the inverse transform is basically the same to go back to regular world
$$f_{j} = \frac{2}{N}\sum_{k=1}^Nc_{k}\sin\left( \frac{jk\pi}{N} \right)$$
Note the $\frac{2}{N}$ factor is a normalization factor to ensure orthogonality (it might be actually $\sqrt{ \frac{2}{N+1} }$ according to Gemini, but I'm not too sure. Either way, there is some normalization required.)

Similarly, we can now derive the Discrete Fourier Transform

# Discrete Fourier Transform
The previous idea can also be applied to the general Fourier series. This is the discrete Fourier Transform and the Inverse Fourier Transform

$$f_{j} \to c_{k} = \sum_{j=1}^n f_{j} e^\bar{i 2\pi jk/N}$$
$$c_{k} \to f_{j} = \frac{1}{N}\sum_{k=1}^n c_{k}e^{i_{2}\pi jk/N}$$
The result is an orthonormal basis of a vector in $R^n$ with periodic functions.

One can also derive the discrete cosine transform by applying the same ideas on the $B$ matrix. JPEG compression utilizes this as its algorithm. Today, there are better transforms and wavelets that can be used for image compression.