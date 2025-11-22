The textbook for the class would typically cover higher-order Finite Elements and the beam equation, which considers systems of the form
$$-u''''(x)=f$$
It will not be covered here as the math does not change significantly from previous, but they are still interesting problems nevertheless.

This lecture will focus on the following equation
$$-\left( \frac{\delta^2u}{\delta x^2} + \frac{\delta^2u}{\delta y^2} \right) = f(x,y)$$
This is the Poisson equation, which comes up quite often. We will consider heat conduction here, but this can also be applied in hydraulics or voltage problems.

Consider the following diagram:
![[Pasted image 20251106145820.png]]This is a rough sketch of a stove plate. The center contains an electric heating element, which has some power $P = f(x,y)$ that is converted to heat. $T_0$ represents the ambient temperature in the environment. We are interested in how the temperature will be spread across the plate at equilibrium: $T = u(x,y) - T_{0}$.

As always, we apply the common framework that is oh so familiar to this class.
- Start with $u(x,y)$ #clarify what does this represent?
- The matrix A will be a gradient operator, so $A\vec{u} = \nabla \vec{u} = \begin{bmatrix} \frac{\delta u}{\delta x} \\ \frac{\delta u}{\delta y} \end{bmatrix}$
- $\vec{e}(x,y)$ represents the temperature gradient between different points.
- $\vec{w}(x,y) = c(x,y)\vec{e}(x,y)$ represents heat fluxes, with $c(x,y)$ representing thermal conductivity.
- Finally, we get to the balanced equation $A^T\vec{w}=\vec{f}$ where $A^T = \begin{bmatrix}-\frac{\delta u}{\delta x} & -\frac{\delta u}{\delta y}\end{bmatrix}$
For this to hold, we need the condition that $u(x,y) = 0$ on the boundary. In this case, the boundary is the edge of the stove plate. More on this later.

The balanced equation is of the form
$$A^T\vec{w} = \begin{bmatrix}
-\frac{\delta}{\delta x} & -\frac{\delta}{\delta y}
\end{bmatrix} \begin{bmatrix}
w_{1}(x,y) \\
w_{2}(x,y)
\end{bmatrix} = -\frac{\delta}{\delta x}w_{1} -\frac{\delta}{\delta y}w_{2} = \vec{f}$$
$$-(\frac{\delta}{\delta x}w_{1} +\frac{\delta}{\delta y}w_{2}) = \vec{f}$$
The expression on the left should look familiar for those who recall calculus. This is equivalent to the (negative) divergence of $\vec{w}$, noted as $-div(\vec{w}) = -\nabla \cdot \vec{w}$.

Now, we substitute the definitions of each equation using the framework:
$$-div(\vec{w})$$
$$=-div(c\vec{e})$$
$$=-div(c\nabla \vec{u}) = f$$
Equivalently,
$$-\nabla \cdot (c \nabla \vec{u}) = f$$
This general form is known as the Poisson equation. If $f=0$ then it is called the Laplacian equation. 

Now, onto the boundary definition.
![[Pasted image 20251117165520.png]]
The boundary $\Omega$ is defined as $u(x,y) = 0$ for $u(x,y) \in \Omega$. In this system, we would say that the temperature decreases outside the plate, until it reaches the boundary defined by $\delta \Omega$. 

As an example, consider $\Omega: x^2 + y^2 \leq 1$, with $u=0$ on $\Omega, c=1, f=-4$. Then $-\nabla \cdot \nabla \vec{u} = f$
We can simplify $\nabla \cdot \nabla = \frac{\delta}{\delta x^2} + \frac{\delta}{\delta y^2}$. This operator is known as $\nabla^2$ in physics, or the Laplacian ($\Delta$) in math.
This is solved by the function $u=x^2 + y^2 - 1$. You can validate that this solves the system. The visualization of this solution is the following graph:
![[Pasted image 20251117171141.png]]The partials represented by $\nabla u$ points in the direction of steepest ascent. This scheme is commonly used in gradient descent, which follows $-\nabla u$ to minimize $u(x,y)$. Interestingly, the MATLAB logo is also a solution to a specific problem.

Now, let's see how we arrive to the solution. We will apply finite differences and discretize the area into a series of points.
![[Pasted image 20251118141603.png]]
To calculate $\Delta u_{i,j}$, we need to consider the partial derivatives in x and y. Then we can combine them:
$$\Delta u_{i,j} = u_{xx} + u_{yy}$$
Using a centered difference, we can compute each term:
$$u_{xx} = \frac{1}{h^2}(-u_{i,j-1} + 2u_{i,j} - 1u_{i,j+1})$$
$$u_{yy} = \frac{1}{h^2}(-u_{i-1,j} + 2u_{i,j} - 1u_{i+1,j})$$
Here, we set our coordinate axis as $x=j,y=i$. You can choose the coordinate axis however you want, so long as you are consistent.

Then we can compute this entry in the resulting $K_{2D}U$ product as
$$\frac{1}{h^2}(-u_{i,j-1}-u_{i-1,j} + 4u_{i,j} - u_{i,j+1} - u_{i+1,j})$$
We can form the matrix $K_{2D}$ and vector U as the following:
![[Pasted image 20251118142153.png]]
Notice that $K_{2D}$ takes the form of a block matrix. Each block in the vector $U$ represents a row of points from the graph before. Additionally, the discretization of each axis of the grid into n segments produced $n^2$ unknowns. The matrix $K_{2D}$ will  be $n^2\times n^2$, which might be a problem to store in memory.