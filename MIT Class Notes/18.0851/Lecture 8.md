Positive Definite Matrices came about due to physics. When studying kinetic systems, Energy can't really be a negative quantity, and the matrices used in studying these systems had some interesting properties that could be extended to other fields.

Let's look at this simple spring-mass system:
![[Pasted image 20251028234344.png]]
The internal force acting on this spring comes from Hooke's law:
$$w=ce$$where e is the elongation or compression of the spring.

If we wanted to measure the energy of the spring, we would integrate the force over the distance. This comes from work/energy being equal to force * distance.
$$E_{spring} = \int w(e)de$$
$$=\int ce de$$
$$= \frac{1}{2}ce^2$$
If we had a general system of springs, the potential energy of the system is 
$$E=\sum_{i} \frac{1}{2}c_{i}e_{i}^2 > 0$$This quantity is always positive unless all the elongations are 0. We can rewrite this quantity as a matrix because those are easier to work with:
Define $\vec{e}$ as a vector representing the elongations of all springs, and $C$ is a diagonal matrix with the Hook constants of each spring on the diagonal. Then we can rewrite the equation for potential energy:
$$E=\frac{1}{2}\vec{e}^TCe$$
Recall in the first lecture how we went from displacements of masses to elongations and that process. We can apply this same process here:
$$E=\frac{1}{2}\vec{u}^TA^TCA\vec{u}$$
$$E=\frac{1}{2}\vec{u}^TKu$$Notice that this is always > 0 unless $\vec{u}=\vec{0}$. This is the definition for a positive definite matrix (without the 1/2).
Some other equivalent statements of a positive definite matrix are:
- All the pivots  > 0
- All the upper-left determinants > 0. By this, we mean that if you take any square matrix in the upper left region and compute its determinant, then it will always be > 0.
- All the eigenvalues > 0.
A small note: we are going to talk almost exclusively about symmetric matrices for the rest of this Lecture. This will simplify a few points. It's also usually baked into the definition of a positive definite matrix.

Let's look at another example. Suppose I have this matrix and vector:
$$P = \begin{bmatrix}
1 & 2 \\
2 & c
\end{bmatrix},
\vec{x}=\begin{bmatrix}
x_{1} \\
x_{2}
\end{bmatrix}$$
For this matrix to be positive definite, c has to be chosen in a specific way:
$$f(\vec{x}) = x_{1}^2+4x_{1}x_{2}+cx_{2}^2 > 0$$
If we complete the square, then we get the following result:
$$(x_{1}+2x_{2})^2+(c-4)x_{2}^2 > 0$$
This sets two specific cases for c:
- If $c > 4$, then we know this matrix is positive definite: $f(\vec{x}) > 0$ unless $x = \vec{0}$
- If $c < 4$, then the matrix isn't positive definite, meaning we can find $\vec{x}\neq \vec{0}$ such that $f(\vec{x})<0$.

What happens when $c = 4$? Then we have a new kind of matrix: positive semidefinite. The definition is similar to positive definite matrices, except that the condition is relaxed to include 0.
- If c = 4, then $f(\vec{x}) = \vec{0}$ when $x_{1}=-2x_{2}$. Here's something interesting about the matrix P when $c=4$:
$$P=\begin{bmatrix}
1 & 2 \\
2 & 4
\end{bmatrix} = 
\begin{bmatrix}
1 \\
2
\end{bmatrix}
\begin{bmatrix}
1 & 2
\end{bmatrix} = M^TM$$With this, we can come up with another test for positive semi-definite matrices:
$$x^TPx=x^TM^TMx=(Mx)^TMx=\lvert Mx \rvert \geq 0$$
If a matrix K can be factored into two equivalent matrices $M, M^T$, then the inequality above should hold. 
If $$M\vec{x}=\vec{0}$$ only when $$\vec{x}=\vec{0}$$then K is positive definite, and M has a zero null space.

Let's revisit some previous matrices from before:

The matrices $K_{n}, T_{n}$ are positive definite matrices. Let's see how $K_n$ is positive definite:
$$K_{3} = \begin{bmatrix}2 & -1 & 0 \\ -1 & 2 & -1 \\ 0 & -1 & 2\end{bmatrix}$$
We can split the matrix into a matrix A: 
$$A = \begin{bmatrix}
1 & -1 & 0 \\
0 & 1 & -1 \\
0 & 0 & 1
\end{bmatrix}$$
$$K_{3} = A^TA = \begin{bmatrix}
1 & 0 & 0 \\
-1 & 1 & 0 \\
0 & -1 & 1
\end{bmatrix}
\begin{bmatrix}
1 & -1 & 0 \\
0 & 1 & -1 \\
0 & 0 & 1
\end{bmatrix}$$
$K_{3}$ is positive semi-definite at least. To prove positive definite, we look at the null space of A.
$$A\vec{x}=\begin{bmatrix}
1 & -1 & 0 \\
0 & 1 & -1 \\
0 & 0 & 1
\end{bmatrix}
\begin{bmatrix}
x \\
y \\
z \\
\end{bmatrix} = 
\begin{bmatrix}
0 \\
0 \\
0
\end{bmatrix}$$
This is only true when $x=y=z=0$, so A has no null space. That means $K_{3}$ is positive definite.

Commonly, you'll see this symbol in optimization for positive definite: 
$$K_{3}\succ 0$$
Positive semi-definite uses the symbol
$$Q \succeq 0$$
The matrices $B_{n}, C_{n}$ are positive semidefinite. Let's see an example with $B_3$
$$B_{3}=\begin{bmatrix}
1 & -1 & 0 \\
-1 & 2 & -1 \\
0 & -1 & 1
\end{bmatrix} = \begin{bmatrix}
1 & 0 \\
-1 & 1 \\
0 & -1 \\
\end{bmatrix}
\begin{bmatrix}
1 & -1 & 0 \\
0 & 1 & -1
\end{bmatrix}$$
The factorization exists so the matrix is positive semidefinite. Now, check the null space of the factored matrix:
$$\begin{bmatrix}
1 & -1 & 0 \\
0 & 1 & -1
\end{bmatrix}
\begin{bmatrix}
x \\
y \\
z
\end{bmatrix}=
\begin{bmatrix}
0 \\
0
\end{bmatrix}$$
From observation, the vector $\begin{bmatrix}1 \\ 1 \\ 1\end{bmatrix}$ is in the nullspace of the matrix.
Note: generally, short/wide matrices have a null space. In a system of equations, generally it signals underspecification so there are multiple or no solutions to the system.

Another way to see that $B_{3}$ is positive semi-definite is to look at its eigenvalues. 0 is an eigenvalue of B, and the other eigenvalues are $\geq 0$, so it has to be positive semi-definite.

Let's look at the diagonalization of $B_{3}$
$B_3 = S \Lambda S^T$ ($S^T$ because $B_3$ is symmetric).
If we apply the definition of positive semi-definite here, then we get
$$x^TS\Lambda S^Tx$$
$$=(S^Tx)^T\Lambda S^Tx$$
Define the variable $c=S^Tx$. Then,
$$B_{3}=c^T\Lambda c = \sum_{i=1}^n \lambda_{i}c_{i}^2 \geq 0$$
If $\lambda_{i} \geq 0$, then $c^T\Lambda c \geq 0$. If you know your matrix is positive semi-definite, then the definition above will hold.

# A Real World Example
Suppose we had a geothermal system consisting of the Earth and two reservoirs.
![[Pasted image 20251011213136.png]]Define k as the rate of heat exchange between blocks, and $c_{i}$ is the thermal capacity of each block. Using Newton's Law of Cooling, we can define the change in temperatures of the systems:

$$\frac{dT_{1}}{dt} = -k(T_{1}-T_{2})$$
$$\frac{dT_{2}}{dt}=-k(T_{2}-T_{1})-k(T_{2}-T_{3})$$
$$\frac{dT_{3}}{dt}=-k(T_{3}-T_{2})$$
For simplicity, we are going to assume a time-dependence only.

Let's put this in a matrix equation:
$$
\frac{d}{dt}\begin{bmatrix}
T_{1} \\
T_{2} \\
T_{3}
\end{bmatrix} = -k
\begin{bmatrix}
1 & -1 & 0 \\
-1 & 2 & -1 \\
0 & -1 & 1
\end{bmatrix}
\begin{bmatrix}
T_{1} \\
T_{2} \\
T_{3}
\end{bmatrix}$$
The matrix used is $B_{3}$, and we know that it is positive semidefinite.

The normal mode of this, ie. the solution, is of the form
$$\vec{u}=f(t)\vec{y}$$where $\vec{y}$ is an eigenvector of the matrix $B_{3}$, and $f(t) = f(0)e^{-k\lambda t}$. Knowing that $B_{3}$ is a positive semi-definite matrix, we know that all of its eigenvalues are either 0 or positive, and one of them must be 0:
$$\lambda_{1}\geq 0$$
$$\lambda_{2} \geq 0$$
$$\lambda_{3} = 0$$
We can determine that this system will experience some exponential decay based on the first two eigenvalues, and the temperatures of the system will decay to some equilibrium values.

Recall the problem setup for the $K_{2}$ matrix:
![[Pasted image 20251013162106.png]]
$$K_{2} = \begin{bmatrix}
c_{1}+c_{2} & -c_{2} \\
-c_{2} & c_{2}+c_{3}
\end{bmatrix}$$
This matrix is positive definite (spring constants can't be negative anyways). The differential equation that defines this system is
$$m\frac{d^2u}{dt^2} = f - K\vec{u}$$
and it is solved by the normal mode $$u(t) = \cos(\omega t)\vec{y}$$To get the value of $\omega$, we solve $-m\omega^2+\lambda=0 \to \lambda = m\omega^2$. These eigenvalues are always positive, implying exponential growth. This means this will oscillate forever. We are able to discern all this information without actually doing any math.