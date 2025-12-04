The final topic to cover is 2D finite elements. 1D settings, we used hat functions at different points for finite elements. These approximations will take higher dimensional shapes or meshes instead, a value of 1 at some point, and linearly going to 0 at neighboring points (these are tent functions).

There are a wide range of meshes to choose from, each one having varying levels of accuracy. We will focus on simple, tent meshes here, but other choices include bilinear and quadratic meshes, which are more complicated.

As before, the problem we want to solve is the Poisson equation
$$\begin{cases}
-\Delta u=f & (x,y) \in \Omega \\
u = 0 & (x,y) \in \delta \Omega
\end{cases}$$
Here, we consider the boundary of a disc of radius 1.

In order to do our finite elements, we are going to need two lists to track some information:
- P is a list of points of the mesh, represented as $$P=\begin{bmatrix}
x_{1} & x_{2} & x_{3} & \dots \\
y_{1} & y_{2} & y_{3} & \dots \\
\end{bmatrix}$$
- T is a list that indicates which points belong to a specific triangle. As an example,
$$T = \begin{bmatrix}
1 & 1 & 2 & \dots \\
3 & 2 & 4 & \dots \\
4 & 4 & 5 & \dots
\end{bmatrix}$$
These represent the following example mesh below:
#drawing 

Now for our basis functions.
![[Pasted image 20251203162236.png]]Let's look at a concrete example to get an idea of what these look like. This triangle will have 3 basis functions $\phi_{0},\phi_{1},\phi_{2}$(not pictured, but it's defined in the same way as the others, just centered at (1,0)).

Note that this triangle has an area of $\frac{1}{2}$. This will be important later.

Like in the 1D case, we are going to solve a weakened form of the problem, following the same framework from before:
![[Pasted image 20251203162426.png]]
The weak form of the problem will be
$$\int \int_{\Omega} (-\Delta u(x,y) v(x,y))dx dy = \int \int_{\Omega} f(x,y)v(x,y) dx dy$$
We simplify the left part using a similar idea to integration by parts
$$=\int \int(-div \nabla u)v(x,y)dxdy$$
The divergence operator will become a gradient operator on v:
$$=\int \int \nabla u\nabla v dxdy - \oint_{\delta \Omega} (\nabla u \cdot \vec{n}) v dl$$
This formulation is Green's First Identity. It's a common tool used in electromagnetism.

On the right, we have a line integral over the bounds of the disc. We will set this to be 0 by setting conditions on the test function v as usual. Note that we can't expect $\nabla u \cdot \vec{n}$ to be 0 here based on our boundary conditions.

The weakened form of this problem is now to find $u(x,y)$ such that $u=0$ on $\delta \Omega$ and
$$\int \int_{\Omega}(\nabla u \cdot \nabla v) dx dy = \int \int_{\Omega} f v dx dy$$
for all $v$ such that $v = 0$ on the boundary $dl$.

Suppose the boundary condition was $\frac{du}{dn} = \nabla u \cdot \vec{n} = 0$? Then we won't need conditions on the test function $v$, as the line integral goes to 0 anyways.

# Galerkin Construction
Our weakened problem is the equation
$$\int \int_{\Omega}(u_{x}v_{x} + u_{y}v_{y}) dxdy = \int \int_{\Omega}fvdxdy$$
(Note that I have rewritten the dot product before in terms of the actual partial derivatives).

We now apply the Galerkin method to solve this:
1) We do a trial expansion of $u(x,y) = \sum_{i} U_{i}\phi_{i}(x,y)$
2) Our test functions become $v(x,y) = \phi_{j}(x,y)$, choosing only those that obey the Dirichlet boundary conditions.

This now forms a linear system of the form
$$KU=F$$
Where K is our stiffness matrix. Each row of this system corresponds to the equation
$$\sum_{i} K_{i,j}U_{i} = F_{j}$$
for the index j.

We can equivalently express K as a sum of element matrices in the following way:
$$K=\sum_{e}K_{e}$$where
$$(K_{e})_{i,j} = \int \int_{e} ((\phi_{i})_{x} (\phi_{j})_{x} + (\phi_{i})_{y} (\phi_{j})_{y}) dx dy$$
e represents the boundary of the triangle, ie the 3 points that make it up.

Our choice of meshes makes this calculation relatively simple, if not tedious. The right hand side of the system becomes
$$F_{j} = \int \int_{\Omega} f(x,y) \phi_{j}(x,y) dx dy$$
Using the triangle from above, we can compute an example entry in the K matrix:
$$\phi_{0}(x,y) = a + bx + cy$$
Solving for each variable:
$$\phi_{0}(0,0)=1=a$$
$$\phi_{0}(1,0)=0=a+b \to b = -1$$
$$\phi_{0}(2,1) = 0 = a + 2b + c \to c = 1$$
Then,
$$\phi_{0}(x,y) = 1 - x + y$$
$$K_{e} = \begin{bmatrix}
(K_{e})_{00} & (K_{e})_{01} & (K_{e})_{02} \\
(K_{e})_{10} & (K_{e})_{11} & (K_{e})_{12} \\
(K_{e})_{20} & (K_{e})_{21} & (K_{e})_{22}
\end{bmatrix}$$
Let's compute a few of these entries:
$$(K_{e})_{00} = \int \int_{e} ((\phi_{0})_{x}^2 + (\phi_{0})_{y}^2)dxdy $$
$$=((-1)^2 + (1)^2)\cdot \text{Area of triangle e}$$
Since these partials turn out to be constant, the double integral simplifies to the area of the triangle.
$$=\frac{2}{2} = 1$$
We'll need
$$\phi_{1} = x-2y$$
for the next part. This equation is derived in the same fashion that $\phi_{0}$ was derived.
$$(K_{e})_{01} = \int \int_{e} ((\phi_{0})_{x}(\phi_{1})_{x} + (\phi_{0})_{y}(\phi_{1})_{y})dxdy$$
$$=\frac{(-1\cdot 1 + 1 (-2))}{2} = -\frac{3}{2}$$
Similarly,
$$(K_{e})_{11} = \frac{5}{2}$$
We can start filling in this element matrix:
$$K_{e} = \begin{bmatrix}
1 & -\frac{3}{2} & ? \\
-\frac{3}{2} & \frac{5}{2} & ? \\
? & ? & ?
\end{bmatrix}$$
Notice two things about this matrix so far:
- This matrix is symmetric
- This matrix is positive definite
The symmetric part is obvious. We can determine it is positive definite based on the fact that the determinant of the upper left 2x2 matrix we have is positive. This also means we can conclude a few more properties of the matrix:
- $\vec{x}^TK_{e}\vec{x}>0$ unless $\vec{x}=\vec{0}$
- The eigenvalues are all real and positive
These will turn out to be true for the whole matrix K.

The rest of the derivation is done in the same way, repeated over all the triangles used for the mesh. As shown, this is quite tedious to compute by hand. Luckily, there have been many years of optimization to construct and solve these schemes.

## And that's it for this class!
