Last time, we started looking at finite elements, a way to approximate solutions of the differential equation
$$-\frac{d}{dx}\left( c(x) \frac{du}{dx} \right) = f(x)$$
with some boundary conditions.

Graphically, this will look like the following:
![[Pasted image 20251104155331.png]]
Where $\tilde{u}$ is our approximation.

Instead of solving it directly, we can weaken the problem with two steps.
1. We define some piecewise linear functions we can expand the function u around
2. We then symmetrize the equation using test functions
In the free-fixed scenario we were looking at, the differential equation contains the boundary conditions $u'(0)=0, u(1)=0$. For simplicity, we will also impose the conditions $f=1, c=1$.

It takes the simpler form
$$\int_{0}^1 c \frac{du}{dx} \frac{dv}{dx} dx = \int_{0}^1 fv \space dx$$
with boundary conditions $v(1)=0, u(1)=0$.

We choose the following piecewise functions $\phi_{i}$, represented as the graphs
![[Pasted image 20251104155812.png]]
Then, we can expand the unknown function u using the trial functions, taking the form 
$$u(x) = U_{0}\phi_{0}(x) + U_{1}\phi_{1}(x) + \dots + U_{4}\phi_{4}(x)$$
Now, we pick our test functions v as the piecewise functions $\phi_{0},\phi_{1},\dots,\phi_{4}$. Note that we do not include $\phi_{5}$, in either of our examples, as it would violate the boundary conditions.

The unknowns are the values of $U_{0},U_{1},\dots,U_{4}$. The equations that solve each one uses one of the test functions $v = \phi_{j},j=0,\dots,4$
Plugging these into the weak form, we now have the following relations:
$$\frac{du}{dx} = \sum_{i=0}^4 U_{i}\phi'_{i}(x)$$
$$\frac{dv}{dx} = \phi'_{j}(x)$$
$$v=\phi_{j}(x)$$
$$\int_{0}^1 \sum_{i=0}^4 U_{i}\phi'_{i}(x)\phi'_{j}(x)dx = \int_{0}^1f\phi_{j}dx$$
We can simplify this a little bit by moving the summation outside (also using f =1)
$$ \sum_{i=0}^4 \int_{0}^1U_{i}\phi'_{i}(x)\phi'_{j}(x)dx = \int_{0}^1\phi_{j}dx$$
Note that this is now a system of these equations, defined over all values of $j$. We can equivalently express these as the matrix equation
$$KU=F$$
$U$ is a vector containing our unknowns. We can directly compute F, using the fact that integrals are areas under the curve. Using $\Delta x=\frac{1}{5}$ (the interval spacing),
$$F = \begin{bmatrix}
\frac{\Delta x}{2} \\
\Delta x \\
\Delta x \\
\Delta x \\
\Delta x
\end{bmatrix} $$
Now, we need to determine the value of $\phi'_{j}(x)$. Let's just look at $j=1$, as the other functions will follow a similar pattern:
$$\phi'_1(x) = \begin{cases}
\frac{1}{\Delta x}, x \in \left[ 0, \frac{1}{5} \right] \\
-\frac{1}{\Delta x}, x \in \left[ \frac{1}{5}, \frac{2}{5} \right]
\end{cases}$$
It linearly increases up and then linearly decreases. The x interval is $\Delta x = \frac{1}{5}$, so the slope makes sense. For other values of j, simply adjust the bounds as needed, and note that for j=0, there is only a negative component (it starts at 1 and then goes to 0).
Now we can slowly construct the matrix K. We do this element by element:
$$K_{0,0} = \int_{0}^{1/5} (\phi'_{0}(x))^2dx = \Delta x\left( \frac{1}{\Delta x} \right)^2 = \frac{1}{\Delta x}$$
(To work this out by hand, recall that $\phi'$ is the derivative, or just the value of the slope. Pset 8 has a question that involves this, so it's good to understand how to perform this. It's really just calculus at it's core)
$$K_{1,0} = \int_{0}^{1/5} \phi'_{0}(x)\phi'_{1}(x)dx = \Delta x\left( -\frac{1}{\Delta x} \right)\left( \frac{1}{\Delta x} \right) = -\frac{1}{\Delta x}$$
$$K_{1,1} = \int_{0}^{1/5}(\phi'_{1}(x))^2dx + \int_{1/5}^{2/5}(\phi'_{1}(x))^2dx = 2 \Delta x\left( \frac{1}{(\Delta x)^2} \right) = \frac{2}{\Delta x}$$
The rest of the entries will follow a similar construction. Eventually, the resulting matrix will become
$$K = \frac{1}{\Delta x}\begin{bmatrix}
1 & -1 & 0 & 0 & 0 \\
-1 & 2 & -1 & 0 & 0 \\
0 & -1 & 2 & -1 & 0 \\
0 & 0 & -1 & 2 & 1 \\
0 & 0 & 0 & -1 & 2
\end{bmatrix}$$
This is a familiar matrix which describes free-fixed systems that we encountered many lectures ago: $T_5$.
Notice the difference between this and finite differences. 
$$F = \begin{bmatrix}
\frac{\Delta x}{2} \\
\Delta x \\
\Delta x \\
\Delta x \\
\Delta x
\end{bmatrix} \text{versus}
\space f = \begin{bmatrix}
1 \\
1 \\
1 \\
1 \\
1 \\
\end{bmatrix}$$
As it turns out, the solution using finite elements $\tilde{u}(x)$ is chosen such that
$$\max_{x}|u(x)-\tilde{u}(x)| = O((\Delta x)^2)$$
Finite elements achieve 2nd order accuracy, a much stronger one than the 1st order accuracy of finite differences. Additionally, this framework can be applied to higher dimensions, while preserving good accuracy. The approximations become higher-dimensional shapes, such as triangles or some tetrahedrons.

So far, we have done the following

We took the strong problem
$$-\frac{d}{dx}(c(x) \frac{du}{dx}) = f(x)$$
And converted it to the weaker form
$$\int c \frac{du}{dx} \frac{dv}{dx} dx = \int fvdx$$
each with some boundary conditions.

In a Linear Algebra context, the rough equivalents are taking the problem
$$A^TCAu=f$$
and converting it to the weaker problem
$$(Au)^TCAu = v^Tf \space \space \space\forall v$$
This can be solved using the minimization problem
$$\min_{u} \frac{1}{2}u^TA^TCAu - v^TF$$
which has the continuous analogy of
$$\min_{u} \frac{1}{2}\int c(x) \left( \frac{du}{dx} \right)^2 dx - \int f(x)v(x)dx$$

Going back to the example before, what if we made our intervals irregular?
![[Pasted image 20251106144716.png]]
Instead of a fixed $\Delta x$, each interval now has it's own specific spacing defined by $e_{ij}$.

As it turns out, finite elements make this easier to handle. We just have to consider each interval separately:
$$\min_{u}\int_{e_{01}} + \int_{e_{12}} + \int_{e_{23}} + \dots$$
Let's look at the $e_{12}$ interval.
$$\int_{e_{12}}c \frac{du}{dx} \frac{du}{dx} = \int_{e_{12}}c(U_{1}\phi'_{1} + U_{2}\phi_{2}')(U_{1}\phi'_{1} + U_{2}\phi_{2}')dx$$
We represent this as a quadratic matrix-vector product
$$\begin{bmatrix}
U_{1} & U_{2}
\end{bmatrix}
\begin{bmatrix}
\int \phi_{1}'\phi_{1}'c & \int \phi_{1}'\phi_{2}'c \\
\int \phi_{1}'\phi_{2}'c & \int \phi_{2}'\phi_{2}'c
\end{bmatrix}
\begin{bmatrix}
U_{1} \\
U_{2}
\end{bmatrix}$$
This corresponds to the small element matrices that make up K.
$$K = \begin{bmatrix}
1 & -1 & 0 & 0 & 0 \\
-1 & 2 & -1 & 0 & 0 \\
0 & -1 & 2 & -1 & 0 \\
0 & 0 & -1 & 2 & 1 \\
0 & 0 & 0 & -1 & 2
\end{bmatrix}$$
Each 2x2 block can be thought of as a "sum" of the block matrix 
$$\begin{bmatrix}
1 & -1 \\
-1 & 1
\end{bmatrix}$$ that are superimposed on top of each other. If that makes sense.