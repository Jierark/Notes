This lecture we will be looking at eigenvalues and eigenvectors. These are super important in modeling dynamic systems, where many things could be changing over time. A common example is protein folding - the interest is to look at how a given strand of protein might fold due to the various forces acting on the atoms and molecules comprising it. The system will continue to change until it reaches a threshold of minimum energy, which marks stability.

For simplicity, let's look at a special case: a linear system that is time invariant
$$\frac{d\vec{x(t)}}{dt} = A\vec{x(t)}$$ where $\vec{x(t)} = \begin{bmatrix}x_{1}(t) \\ x_{2}(t) \\ \dots \\ x_{n}(t)\end{bmatrix}$
By time invariant, we mean that the matrix A is constant and does not depend on time.

How do we commonly solve these? Usually with eigenvalues and eigenvectors (hope you remember your differential equations)

# Eigenvalues and Eigenvectors
Let's look at the following equation:
$$A\vec{y}=\lambda \vec{y}$$
We say that $\lambda$ is an eigenvalue when it satisfies the relationship above. The vector $\vec{y}$ that allows this to work is it's associated eigenvector. Another way to say this is that the matrix A scales the vector $\vec{y}$ by some scalar factor $\lambda$, drawing from the fact that matrices represent some transformation of the vector.

Here's a property of eigenvalues and eigenvectors: What happens if we repeatedly apply A to this?
$$A^2\vec{y} = (AA)\vec{y} = A(A\vec{y}) = A(\lambda \vec{y}) = \lambda A\vec{y} = \lambda^2\vec{y}$$
The eigenvalues will scale accordingly.