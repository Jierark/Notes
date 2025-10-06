Let's recall the K matrix from before:
$$
K_{3} = \begin{bmatrix}
2 & -1 & 0 \\
-1 & 2 & 1 \\
0 & -1 & 2
\end{bmatrix}
$$
What are some characteristics of this matrix?
- It's symmetric, so ${K_3}^T = K_{3}$, or $K_{ij} = K_{ji}$ 
- It's tridiagonal. the 3 diagonals of K contain nonzero entries, with everywhere else being 0. This is especially true when K is in larger dimensions, and it additionally means K is sparse.
	- A handy note: the equation $K u = f$ can be solved way faster here than if K was a dense matrix.
- The values along each diagonal is constant. This is known as a **Toeplitz Matrix**.
Some less obvious properties:
- K is positive definite
- K is invertible
- The null space of K is the 0 vector.
- K is a full rank matrix.
Let's back up a bit. I just threw a bunch of terms at you, hoping you remembered what they meant. We need to do some review of linear algebra and catch you up to speed on what you need to know.

# Linear Algebra Review
Suppose I have this graph with 3 defined vectors $\vec{u},\vec{v},\vec{w}$.

![[Pasted image 20251001135025.png]]

Let's suppose I had an unknown vector $\vec{b}$ . Can I find $x_{1}, x_{2}, x_{3}$ such that $x_{1}\vec{u} + x_{2}\vec{v} + x_{3}\vec{w} = \vec{b}$?

First, we can convert this into an equal matrix equation using a matrix A, consisting of u, v, and w as its basis.
$$
\begin{bmatrix}
1 & 0 & 0 \\
-1 & 1 & 0 \\
0 & -1 & 1
\end{bmatrix}
\begin{bmatrix}
x_{1} \\
x_{2} \\
x_{3}
\end{bmatrix}
= \begin{bmatrix}
b_{1} \\
b_{2} \\
b_{3}
\end{bmatrix}
$$
$$
\begin{bmatrix}
x_{1} \\
x_{2} - x_{1} \\
x_{3} - x_{2}
\end{bmatrix} = 
\begin{bmatrix}
b_{1} \\
b_{2} \\
b_{3}
\end{bmatrix}
$$
This system is solvable, so let's do that. (don't overthink it)
$$\begin{flalign}
&x_{1} = b_{1} & \\
&x_{2} = b_{2} + b_{1} & \\
&x_{3} = b_{3} + b_{2} + b_{1}
\end{flalign}

$$
That's right, we can also rewrite this as another matrix equation:
$$
\vec{x} = 
\begin{bmatrix}
1 & 0 & 0 \\
1 & 1 & 0 \\
1 & 1 & 1
\end{bmatrix}
\begin{bmatrix}
b_{1} \\
b_{2} \\
b_{3}
\end{bmatrix}
$$
That matrix, let's call it S, and the matrix  A before it seem to be related, no? 
S is actually the matrix inverse of A. In general, these equations are equivalent:
$A \vec{x} = \vec{b}$
$\vec{x} = A^{-1}\vec{b}$
Does this always work? Nope. Let's consider the following system:
$\vec{u} = \begin{bmatrix}1 \\ -1 \\ 0\end{bmatrix}, \vec{v} = \begin{bmatrix}0 \\ 1 \\ -1 \end{bmatrix}, \vec{w} = \begin{bmatrix}-1  \\ 0 \\ 1\end{bmatrix}$
We create the matrix C and define the matrix-vector equation as before:
$$
\begin{bmatrix}
1 & 0 & -1 \\
-1 & 1 & 0 \\
0 & -1 & 1
\end{bmatrix}
\begin{bmatrix}
x_{1} \\
x_{2} \\
x_{3}
\end{bmatrix} = 
\begin{bmatrix}
b_{1} \\
b_{2} \\
b_{3}
\end{bmatrix}
$$
Did you notice? $\vec{u}, \vec{v}, \vec{w}$ are coplanar! As a result, $\vec{w} = -\vec{u} - \vec{v}$ . These three vectors are linearly dependent (one can be expressed as a linear combination of the others). There's a few properties that arise from this:
- The matrix C does not have an inverse.
- C has a nonzero null space
- $C \vec{x} = \vec{b}$ has an infinite number of solutions, provided that $\vec{b}$ is in the column space of C.
## Column and Null Spaces
For a given matrix A, you can define some useful properties:
- The **column space** of A is the all vectors it can reach by a linear combination of its vectors. Put another way, Col(A) = $\{A \vec{x} : \vec{x} \in R^n\}$, the set of vectors such that it can be expressed as a matrix-vector product in $R^n$.
	- Essentially, the column space represents where a matrix A can map any given vector.
	- The dimension of the column space is called its rank.
- The **null space** of A, Null(A) is all vectors such that $A \vec{x} = \vec{0}$.
	- Put another way, vectors in the Null space of A will all map to 0.
These can tell you a lot about a matrix and it's related matrix-vector equation.
Oh, here's some other interesting properties:
- Col($A^T$) will be perpendicular to Null(A)
- Null($A^T$) will be perpendicular to Col(A)

Given a square matrix A, all of these statements are equivalent and true. If one is true, they all are.
- $A \in R^{n*n}$ is invertible
- $A \vec{x} = \vec{b}$ has a unique solution
- The columns of A are linearly independent.
- A is full rank (rank(A) = n)
- Null(A) = 0
- Col(A) = $R^n$
- det(A) $\neq$ 0
- 0 is not an eigenvalue of A.
Don't worry if you don't understand some of these statements for now. We will visit some of these terms in future lectures.