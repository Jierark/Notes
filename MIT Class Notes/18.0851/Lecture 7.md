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
$$A^2=S\Lambda S^{-1}S\Lambda S^{-1}$$