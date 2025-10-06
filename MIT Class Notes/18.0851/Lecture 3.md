First, some more linear algebra review. Mostly contains useful terms to remember.
# Useful Definitions
The **Span** of a set of vectors $\vec{x}, \vec{y}, \vec{z}$ is the set of vectors that can be reached by a linear combination of the three vectors. Mathematically, we express this as 
$\{a \vec{x} + b \vec{y} + c \vec{z} \in R^3\}$. It isn't necessarily all of R^3, just that the resulting vectors are in that space.
These three vectors form the **basis** of a **vector space** X if and only if:
	X = Span($\vec{x}, \vec{y}, \vec{z}$ ), and
	$\vec{x}, \vec{y}, \vec{z}$ are linearly independent. Linear independence means that $a \vec{x} + b \vec{y} + c \vec{z} = 0$ only when $a=b=c=0$. That is, no vector is a linear combination of the other two.
	The dimension of X is the number of basis elements (number of vectors that make up X).
The **Column Space** of a matrix A is all the vectors that can be reached by a linear combination of its columns. More formally, it's defined as:
	Col(A) = $\{ A \vec{x}: x \in R^n \}$
	If A is an invertible matrix, then it's column space will be $R^n$.

The **Null Space** of a matrix A is a vector that satisfies the following condition:
	Null(A) = $\vec{x}: A \vec{x} = \vec{0}$
	If A is an invertible matrix, then the null space will always be the 0 vector.
The null space has some important implications in various systems. One case is in differential equations, where you might have two parts of a solution to the system due to the null space (but that's for another time).

$\vec{u}\cdot \vec{v}$ is the **dot product** of two vectors. Compute sum of pairwise multiplication of entries
- The norm of a vector $||\vec{v}||$ is the dot product of a vector onto itself. Or you can just take a square root of sum of squares of components
- Two vectors are orthogonal if their dot product is 0. If their norms are both 1, then they are considered "orthonormal"
- Geometrically, a dot product can be thought of as the projection of u onto v, scaled by their lengths. 

The **Tensor Product** of two vectors is defined as such:
$\vec{x} \otimes \vec{y} = \vec{x}(\vec{y})^T$ The resulting matrix is an n * n matrix A with the following properties:
- A is a rank 1 matrix
- The column space is $\vec{x}$
- Each component can be found as such: $A_{i,j} = x_{i}y_{j}$

# Orthogonality
Two vectors are orthogonal if $\vec{u} \cdot \vec{v} = 0$. There is no projection of either vector onto the other. If $\lvert \vec{u} \rvert = \lvert \vec{v} \rvert = 1$ , then they are considered orthonormal.

Suppose that U is an orthonormal basis for some vector space, with the following values:
$$\begin{bmatrix} \frac{2}{3} & -\frac{1}{3} & \frac{2}{3} \\ \frac{2}{3} &  \frac{2}{3} & -\frac{1}{3}\\ -\frac{1}{3} & \frac{2}{3} & \frac{2}{3} \end{bmatrix} = \begin{bmatrix}\vec{u} & \vec{v} & \vec{w} \end{bmatrix}$$
We're looking to solve the equation $U \vec{x}= \vec{b}$ for some b. This turns out to be very simple: each component of x can be found by taking a dot product of the basis vectors with the vector b:
$x_{1} = \vec{u} \cdot \vec{b}$, $x_{2} = \vec{v} \cdot \vec{b}$, $x_{3} = \vec{w} \cdot \vec{b}$
Let's take another look at those solutions. We can rewrite those as another matrix-vector product:
$$\vec{x} = \begin{bmatrix} \vec{u}^T \\ \vec{v}^T  \\ \vec{w}^T \end{bmatrix} \vec{b}$$
What do you notice? That matrix there is actually just $U^T$. This means that for an orthogonal matrix U (note that it doesn't need to be orthonormal), 
$$U^{-1} = U^T$$
Another result of this property is
$$UU^T = U^TU = I$$ The proof for this is easy to show for $U^TU$ since you know U is an orthogonal matrix and you are computing a dot product of components. The proof for $UU^T$ is more difficult because you are computing a tensor product between the components.

# LU Decomposition
Let's go back to our spring system from previous lectures.
![[Pasted image 20251001140717.png]]
Let's say we want to solve the system where $f=\begin{bmatrix}1 \\ 1 \\ 1 \end{bmatrix}$
$$K_{3}\vec{u} = \begin{bmatrix}
2u_{1}-u_{2} \\
-u_{1} + 2u_{2} - u_{3} \\
-u_{2}+2u_{3}
\end{bmatrix}
= \begin{bmatrix}
1 \\
1 \\
1
\end{bmatrix}$$
The classical way we solve these equations is using an augmented matrix and reducing it to row-echelon form.
$$\begin{bmatrix}
2 & -1 & 0 & |1 \\
-1 & 2 & -1 & |1 \\
0 & -1 & 2 & |1
\end{bmatrix}
\to \begin{bmatrix}
2 & -1 & 0 & |1 \\
0 & \frac{3}{2} & -1 & |\frac{3}{2} \\
0 & -1 & 2 & |1
\end{bmatrix} \left( row_{2} \gets row_{2} + \frac{1}{2} row_{1} \right)
$$$$\to \begin{bmatrix}
2 & -1 & 0 & |1 \\
0 & \frac{3}{2} & -1 & |\frac{3}{2} \\
0 & 0 & \frac{4}{3} & | 2 
\end{bmatrix}
\left( row_{3} \gets row_{3} + \frac{2}{3}row_{2} \right)$$

Then you can do back-substitution and get the solution to the system.
With this form, we can get some other properties of matrices.
- For an upper triangular matrix, the determinant is the product of the pivots - the pivots are the elements on the diagonal.
- The determinant is also the product of the eigenvalues. If you have a determinant of 0, this also means at least one of your eigenvalues is equal to 0.
Remember the interaction between vectors and matrices?
$K_{3}\vec{x}$ is a linear combination of the columns of $K_{3}$, whereas $\vec{x}^TK_{3}$ is a linear combination of the rows of $K_3$.
So the row operations we just did can be equally written as a matrix. Let's see how we do that:

$row_{2}(new) = \frac{1}{2}row_{1}(K_{3}) + row_{2}(K_{3})$
Notice that $row_1$ doesn't change across both matrices. We can rewrite this as such:
$row_{2}(new) = \frac{1}{2}row_{1}(new) + row_{2}(K_{3})$
$row_{2}(K_{3}) = row_{2}(new) - \frac{1}{2}row_{1}(new)$
Putting it into a matrix, we get:
$K_3 = \begin{bmatrix}1 & 0 & 0 \\ -\frac{1}{2} & 1 & 0 \\ 0 & 0 & 1\end{bmatrix} \begin{bmatrix}2 & -1 & 0 \\ 0 & \frac{3}{2} & -1 \\ 0 & -1 & 2\end{bmatrix}$
You can check that these are still equivalent.

Now, we repeat the process in the third row:
$row_{3}(new) = \frac{2}{3}row_{2}(new) + row_{3}(K_{3})$
$row_{3}(K_{3}) = row_{3}(new) - \frac{2}{3}row_{2}(new)$

$K_3 = \begin{bmatrix}1 & 0 & 0 \\ -\frac{1}{2} & 1 & 0 \\ 0 & -\frac{2}{3} & 1\end{bmatrix} \begin{bmatrix}2 & -1 & 0 \\ 0 & \frac{3}{2} & -1 \\ 0 & 0 & \frac{4}{3}\end{bmatrix}$
This is our LU decomposition of K. L is a lower diagonal matrix that "encodes" our row operations, while U is an upper diagonal that "encodes" the result of the row operations.

Decomposing matrices this way allows us to compute certain quantities much easier than before. For example, the determinant of $K_3$ is the product of the determinants of L and U, which are easy to compute. L has a determinant of 1, and the determinant of U is the product of the entries on the diagonal. 

You could also decompose U into two more matrices D, $L^T$, but this is an exercise in the homework and was not mentioned during lecture. LU decomposition is the process by which we would solve matrix-equation systems by hand.