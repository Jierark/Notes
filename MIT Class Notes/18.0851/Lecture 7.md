Let's recall our $K_2$ matrix from before. Previously, we were looking at how eigenvalues and eigenvectors can tell us a lot about a matrix and its related system.
$$\begin{bmatrix}
2 & -1 \\
-1 & 2 \\
\end{bmatrix}$$
This matrix has two eigenvalues and eigenvectors: 3 $\begin{bmatrix}1 \\ -1\end{bmatrix}$ and 1 $\begin{bmatrix}1 \\ 1\end{bmatrix}$

Let's define a matrix S, whose entries are the eigenvectors of a matrix A:
$$S = \begin{bmatrix}
\vec{y_{1}} & \vec{y_{2}} & \dots & \vec{y_{n}}
\end{bmatrix}
= \begin{bmatrix}
\end{bmatrix}$$If we compute AS, the resulting product looks like this:
$$AS = \begin{bmatrix}
A\vec{y_{1}} & A\vec{y_{2}} & \dots & A\vec{y_{n}} \\
\end{bmatrix}
= \begin{bmatrix}
\lambda_{1} \vec{y_{1}} & \lambda_{2}\vec{y_{2}} & \dots \lambda_{n}\vec{y_{n}}
\end{bmatrix}$$Why don't we write this as another matrix-matrix product? Let's define a **Diagonal** matrix $\Lambda$, whose entries on the diagonal are the eigenvalues of A.
$$
\Lambda =
  \begin{bmatrix}
    \lambda_{1} & & \\
    & \ddots & \\
    & & \lambda_{n}
  \end{bmatrix}
$$
Then the following products are equal:
$$AS=S\Lambda$$
Note the placement of the matrix $\Lambda$. We want to scale each eigenvector by its eigenvalue only, so we need to right multiply S by the matrix $\Lambda$.

If S is invertible, then we can do the following
$$A = S\Lambda S^{-1}$$
This is known as spectral decomposition, or diagonalization. It is useful to understand how the matrix A transforms vectors that are applied to it. It's used in many domains such as signal processing or numerical analysis.

Let's consider the following expression $A\vec{x}$. We rewrite it as the equivalent expression $S\Lambda S^{-1}\vec{x}$. How does each matrix in the diagonalization contribute to the overall product?
- $S^{-1}$ projects x along each of the eigenvectors of A.
- $\Lambda$ scales x along the eigenvectors by a factor. This factor is the eigenvalues of each of the previous eigenvectors.
- $S$ "reverts" the transformation, aligning the vector back in $R^n$.
This process is typically known as analysis-scaling-synthesis for each part respectively. The names of each step arise from signal processing, commonly used in Fourier Analysis.

Diagonalization makes computation of matrix powers and inverses much simpler:
$$A^2=S\Lambda S^{-1}S\Lambda S^{-1} = S\Lambda^2S^{-1}$$
$$A^{-1}=(S\Lambda S^{-1})^{-1}=(S^{-1})^{-1}\Lambda^{-1}S^{-1}=S\Lambda^{-1}S^{-1}$$
where the entries of $\Lambda^{-1}$ is equal to $\frac{1}{\lambda_{i}}$


Can we always diagonalize a matrix? Another way to ask this question: can we always invert S?

The answer is no. Sometimes we have repeated eigenvalues and eigenvectors. For example,
$$A=\begin{bmatrix}
1 & 3 \\
0 & 1
\end{bmatrix}$$
has a repeated eigenvalue of 1, and its associated eigenvector is $$\begin{bmatrix}
1 \\
0
\end{bmatrix}$$
We can't actually construct S to be invertible. This is known as a "defective" matrix.

However, if A is symmetric ($A=A^T$), then you can always diagonalize A. Additionally, the matrix S (and by proxy the eigenvectors of A) will be orthogonal.

Let's see this point in action with the $K_2$ matrix:
$$K_{2} = \begin{bmatrix}
2 & -1 \\
-1 & 2 \\
\end{bmatrix}$$
$$S = \begin{bmatrix}
1 & 1 \\
1 & -1
\end{bmatrix}$$
Actually, we're going to make S an orthonormal basis by making the vectors length 1. You'll see why this might be helpful in a bit.
$$S = \frac{1}{\sqrt{ 2 }}\begin{bmatrix}
1 & 1 \\
1 & -1
\end{bmatrix}$$
$$\Lambda = \begin{bmatrix}1 & 0 \\ 0 & 3\end{bmatrix}$$
Important note: The order which you set the eigenvalues and eigenvectors actually does not matter. It is extremely important that you are consistent in your notation, though. Whatever you set as the first eigenvalue in $\Lambda$ must match the first eigenvector in $S$.

Anyways, you can see that $SS^{T} = S^TS = I$, so this property holds for an orthonormal basis:
$$S^T = S^{-1}$$
This makes diagonalization much easier to compute:
$$A = S\Lambda S^T$$
This is part of an important theorem to linear algebra called spectral theorem:
	If A is a symmetric matrix, then A can be diagonalized as  $A=S\Lambda S^T$ where $S^T=S^{-1}$, and all eigenvalues are real.

If you know that S is an orthonormal basis, what can you determine about $S\vec{x}=\vec{z}$? You can show that S will preserve all lengths in space (this is known as an isometry):
$$||\vec{z}||^2 = \vec{z}^Tz= (S\vec{x})^T(S\vec{x}) = \vec{x}^TS^TS\vec{x}=\vec{x}^T\vec{x} = ||x||^2$$
In this case, S will actually apply a rotation of 45 degrees, and can be also expressed with this matrix:
$$S=\begin{bmatrix}
\cos \theta & \sin \theta \\
\sin \theta & -\cos \theta
\end{bmatrix}$$
where $\theta=\frac{\pi}{4}$. Generally, rotation and reflection matrices will always be isometric.

Let's switch to $K_{4}$ and analyze it.
$$K_{4}=\begin{bmatrix}
2 & -1 & 0 & 0 \\
-1 & 2 & -1 & 0 \\
0 & -1 & 2 & -1 \\
0 & 0 & -1 & 2
\end{bmatrix}$$
$$K_{4}\vec{y}=\lambda \vec{y}$$
If you recall, the matrix $K_{4}$ comes from doing $-u''(x)=\lambda u(x)$. So we have a similar problem of finding eigenvalues and eigenvectors, but we're looking at functions. Additionally, $K_{4}$ represents fixed-fixed boundary conditions, so $u(0)=u(1)=0$.

Now, we could just solve this differential equation directly, but solving these by hand is pretty tedious. How else could we solve this?

Recall from differential equations the following facts:
$$u'(x)=\lambda u(x) \to u(x) = Ce^{\lambda x}$$
$$-u''(x)=k^2u(x) \to u(x) = A \sin(kx) + B\cos(kx)$$
(Aside: if $u(0)=u(1)=0$, then you can show that you can never have an exponential solution)

Going back to our original problem, we can conclude that $\lambda=k^2$, and $u(x)$ will be equal to the solution above in 2nd derivatives. Let's plug in our boundary conditions and see what happens:

$$u(0)=A\sin(0)+B\cos(0) \to 0=B$$
$$u(1)=A\sin(k)=0, k = n\pi, n\in Z$$ (Z is all integers)
Then we can conclude
$$\lambda_{n} = n^2\pi^2$$
$$u_{n}(x)=A\sin(n\pi x)$$
This is the solution for the continuous version of the eigenvalue equation. It might be easier to visualize what this solution space looks like:
#drawing 

This means that the functions that fit in the boundary conditions are very specific oscillations. The process is known as quantization in physics.
#clarify 
We'll need this context for later lectures when we look at time dependent systems.

### Orthogonality of Functions
The definition is this:

$$\int_{0}^1u_{m}(x)u_{n}(x)dx=0$$The eigenfunctions we found above are all orthogonal. Proof would require some some trigonometry to figure this out.

So we did all this work for a general case. Let's connect this back to the $K_{4}$ matrix:

How do we compute the eigenvectors of $K_4$? Well, we have a set of eigenfunctions, so we can use those to get the entries. For example,
- $\vec{y}_{1}$ can be generated from computing $u_{1}(x)$ at equal intervals:
$$\vec{y}_{1}=\begin{bmatrix}\sin\left( \frac{\pi}{5} \right) \\ \sin\left( \frac{2\pi}{5} \right) \\ \sin\left( \frac{3\pi}{5} \right) \\ \sin\left( \frac{4\pi}{5} \right) \\ \end{bmatrix}$$
- $\vec{y_{2}}$ can be found from a similar procedure:
$$\vec{y}_{2}=\begin{bmatrix}\sin\left( \frac{2\pi}{5} \right) \\ \sin\left( \frac{4\pi}{5} \right) \\ \sin\left( \frac{6\pi}{5} \right) \\ \sin\left( \frac{8\pi}{5} \right) \\ \end{bmatrix}$$
As it will turn out, these will be exact values and not numerical approximations to some order of error. Each associated eigenvalue will have the value
$$\lambda_{i} = 2-2\cos(k\pi h) = 4\sin^2\left( \frac{k\pi h}{2} \right)$$
To finish up, we go back to the S matrix from before:
$$S=\begin{bmatrix}
\vec{y_{1}}| & \vec{y_{2}}|\dots|\vec{y_{n}}
\end{bmatrix}$$
We can actually compute S entry by entry with the following formula:
$$S_{jk}=\sin(jk\pi h)$$where h is the interval space chosen. Although it's been omitted this whole time, use the constant A to make S an orthonormal basis, or whatever.