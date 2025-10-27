Least Squares

Suppose we have the following set of points
![[Pasted image 20251018205718.png]]Our goal is to find a polynomial which fits these set of points. Sometimes, it's a line of the form $f(t) = C + Dt$. Or we could look for a cubic function $g(t) = C + Dt + Et^2 + Ft^3$. If we have more points than unknowns, then we will need to find an approximation.

Framing this as a matrix-vector form we want to solve the following system for a linear approximation:
$$\begin{bmatrix}
1 & t_{1} \\
1 & t_{2} \\
1 & t_{3} \\
1 & t_{4} \\
1 & t_{5} \\
\end{bmatrix}
\begin{bmatrix}
C \\
D
\end{bmatrix} = 
\begin{bmatrix}
b_{1} \\
b_{2} \\
b_{3} \\
b_{4} \\
b_{5}
\end{bmatrix}$$
We won't be able to solve this system exactly, so we'll approximate this and use an error term to see how far off we are:

$A\vec{u}$ computes approximate labels, with an error $\vec{e} = A\vec{u} - \vec{b}$, defined as the residual. The goal is to minimize the squared errors of these approximations: $||A\vec{u} - \vec{b}||^2$, with our choice of $\vec{u}$. This quantity is always positive, so there is a way to minimize this quantity. The squared factor is important, too. You'll see why.

Let's examine this closer:
$$||A\vec{u}-\vec{b}||^2$$
$$=(A\vec{u}-\vec{b})^T(A\vec{u}-\vec{b})$$
$$=\vec{u}^TA^TA\vec{u} - \vec{b}^TA\vec{u} - \vec{u}^TA^T\vec{b} + \vec{b}^T\vec{b}$$
$$=\vec{u}^TA^TA\vec{u} - 2 \vec{u}^TA^T\vec{b} + \vec{b}^T\vec{b}$$
Note that this matrix $A^TA$ must be a positive definite matrix.

An aside: how do we compute $\nabla_{\vec{u}}E=0$? The notion depends on different fields.

Let's rewrite the equation above in index form:
$$E = \sum_{i,j} u_{i}(A^TA)_{ij}u_{j} - 2 \sum_{i}u_{i}(A^T\vec{b})_{i} + \sum_{i} b_{i}^2$$
Now, how do we compute $\frac{\delta u_{i}}{\delta u_{k}}$? Let's use the following definition (it will look similar to anyone working with partial derivatives):
$$\frac{\delta u_{i}}{\delta u_{k}}=\begin{cases}
1 & \text{if i=k} \\
0  & \text{otherwise}
\end{cases}$$
This means we can simplify summations of the form $\sum_{i}\delta_{ik}(\text{stuff})_{i} = (\text{stuff})_{k}$
We also need this following definition:
$$\frac{\delta}{\delta u_{k}}{u_{i}u_{j} = \frac{\delta u_{i}}{\delta u_{k}}u_{j} + \frac{\delta u_{j}}{\delta u_{k}}u_{i}}$$
$$=\delta_{ik}u_{j}+\delta_{ij}u_{i}$$
(basically product rule)

Using these pieces, we can evaluate the derivative:
$$\frac{\delta E}{\delta u_{k}} = \sum_{i,j}\delta_{ik}u_{j} + \delta_{ij}u_{i}(A^TA)_{ij} - 2\sum_{i}\delta_{i,k}(A^Tb)_{i}$$
$$=\sum_{j}u_{j}(A^TA)_{k,j} + \sum_{i}u_{i}(A^TA)_{k,i} - 2(A^Tb)_{k}$$
Since $A^TA$ is symmetric, we can reverse the orders of summarizations and combine them.
$$=2(A^TA\vec{u})_{k}-2(A^Tb)_{k}=0$$ This is per index k. We can replace them with vectors (and also drop the factor of 2).
$$2(A^TA\vec{u} - A^T\vec{b})=\vec{0}$$
The solution is the form
$$\vec{u}=(A^TA)^{-1}A^T\vec{b}$$

Solving for a 3rd degree polynomial is the same method, as long as the matrix $A$ and the vector $\vec{u}$ in the correct terms. For more complex curves, this computation is quite inefficient. Other methods are generally used in these situations (such as gradient descent).

Here's another way to arrive at this solution.

We know that $A\vec{u}=\vec{b}$ most likely can't be solved exactly. We interpret the columns of $A$ as vectors $\vec{a}_{1},\vec{a}_{2}$: $A=\begin{bmatrix}\vec{a}_{1} | \vec{a}_{2}\end{bmatrix}$.
![[Pasted image 20251018215400.png]]The vector $\vec{p} = A\vec{u}$ represents of projection of $\vec{b}$ onto the column space of A.
$$\vec{p} = \vec{a}_{1}u_{1} + \vec{a}_{2}u_{2}$$ To minimize the error $||\vec{e}|| = ||A\vec{u}-\vec{b}||$, we want $\vec{e}$ to be orthogonal to the plane spanned by the column space of A. That means we want the following equations to hold:
$$\vec{a}_{1}\vec{e}=0,$$
$$\vec{a}_{2}\vec{e}=0$$
We can rewrite this:
$$A^T\vec{e}=0$$
$$A^T(A\vec{u}-\vec{b})=0$$
which arrives back at the same solution as before.

Something interesting to note:
$\vec{p}$ (the projection of b) = $A\vec{u}_{best}$
$=A(A^TA)^{-1}A^T\vec{b}$
The matrix $P=A(A^TA)^{-1}A^T$ defines the projection of a vector onto the column space of A.

Suppose the matrix A is invertible. Then this equation can be simplified:
$$\vec{u}=((A^TA)^{-1}A^T\vec{b})$$
$$=A^{-1}A^{T^{-1}}A^T\vec{b}$$
$$=A^{-1}\vec{b}$$
Which is the typical solution we had for systems of equations.