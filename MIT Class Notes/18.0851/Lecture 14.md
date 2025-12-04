In a [[MIT Class Notes/18.0851/Lecture 4|Previous Lecture]], we looked at finite differences, which allowed us to computationally solve differential equations. However, one issue we have is that finite differences generally carry an error term, which may be impactful depending on the system. Here, we introduce an alternative method to solve these equations, called finite elements. The general idea is as follows.

Let's suppose we want to solve the following differential equation
$$-\frac{d}{dx}\left( c(x) \frac{du}{dx} \right) = f(x), x \in [0,1]$$
This is a general equation that applies to many different systems.

In finite differences, we sample the function u at specific intervals to approximate the derivative:
![[Pasted image 20251103154518.png]]
In finite element schemes, we are instead sampling polynomials at each x, which allows it to extend to higher dimensions:
![[Pasted image 20251103154652.png]]
We will build up to the use of finite elements using analogies to the same $A^TCA$ framework we've seen many times. But first, we need to show how $A^T$ is analogous to $-\frac{d}{dx}$.
![[Pasted image 20251103154807.png]]

Recall the centered difference scheme. In particular, we can represent it as the following matrix
$$A_{c} = \begin{bmatrix}
0 & 1 & 0 & 0 & 0 \\
-1 & 0 & 1 & 0 & 0 \\
0 & -1 & 0 & 1 & 0 \\
0 & 0 & -1 & 0 & 1 \\
0 & 0 & 0 & -1 & 0
\end{bmatrix}$$
Additionally, recall the dot product between two vectors:
$$R^n:\vec{e}\cdot \vec{w} = \vec{e}^T\vec{w}=\sum_{i=1}^ne_{i}w_{i}$$
Suppose that $\vec{e}=A\vec{u}$. Then,
$$\vec{e}\cdot \vec{w}=(A\vec{u})^T\vec{w} = \vec{u}^TA^T\vec{w}$$
This will hold for all choices of $\vec{u},\vec{w}$. Consider this as an algebraic definition of the dot product. 

Here's a useful application of this. Remember the Kronecker delta vector? $\delta_{i}$ is a vector of all 0s, except for a 1 in the i'th entry. Then 
$$(A\delta_{i})\cdot\delta_{j} = A_{j,i}$$
$$=\delta_{i}(A^T\delta_{j})=(A^T)_{i,j}$$
Can we extend the definition of a dot product to a continuous situation? Yes, and this is called the inner product:
Given two functions $e(x),w(x)$, the inner product over the interval $[0,1]$is defined as
$$<e,w> = \int_{0}^1e(x)w(x)dx$$
Set $e(x)=\frac{du}{dx}$ as done above. Then,
$$<\frac{du}{dx},w> = \int_{0}^1 \frac{du(x)}{dx}w(x)dx$$
Don't cancel out the dx's here. It would make 0 sense from a calculus perspective. To simplify this integral, we apply integration by parts:
$$=\int_{0}^1u(x)\left( -\frac{dw}{dx} \right)dx + [uw]_{0}^1$$
Where $[uw]_{0}^1 = u(1)w(1)-u(0)w(0)$. If it is 0, then this simplifies:
$$<u, -\frac{dw}{dx}>$$ which is roughly analogous to $\left( \frac{d}{dx} \right)^T$.

Consider the boundary values that we have seen in the past. In a fixed-fixed scenario,
$u(0)=u(1)=0$, meaning $[uw]_{0}^1 = 0$. Then, $\left( \frac{d}{dx} \right)^T = -\frac{d}{dx}$.

If it is free-fixed, then $u(1)=\frac{du}{dx}(0)=0$, and
$$\int_{0}^1 \frac{du}{dx}wdx =\int_{0}^1u(x)\left( -\frac{dw}{dx} \right)dx + u(1)w(1)-u(0)w(0)$$
Consider the $[uw]_{0}^1$  term. The left part is 0 directly from the boundary condition. The right side #TODO watch recording

We can also see this behavior from the A matrix representing free-fixed.
#TODO watch recording

Finally, we arrive at the definition of finite elements, weak form:

The free-fixed version is of the form
$$-\frac{d}{dx}\left( c(x) \frac{du}{dx} \right) = f(x)$$
with the boundary conditions
$$\frac{du(0)}{dx}=0, u(1)=0$$
To solve this, we can integrate over the boundary, using a test function $v(x)$.
$$\int_{0}^1v(x)\left( -\frac{d}{dx}\left( c(x) \frac{du}{dx} \right) \right) dx=\int_{0}^1v(x)f(x)$$
Using integration by parts, the left side becomes
$$\int_{0}^1c(x) \frac{du(x)}{dx} \frac{dv(x)}{dx}dx + c(1) \frac{du(1)}{dx}v(1) - c(0) \frac{du(0)}{dx} v(0)$$
Since $\frac{du(0)}{dx}=0$, the last term goes to 0. We can make the second term go to 0 by choosing our test function v such that $v(1)=0$.

The weak form finally is
$$\int_{0}^1c(x) \frac{du(x)}{dx} \frac{dv(x)}{dx} dx = \int_{0}^1f(x)v(x)dx$$
Where v is differentiable such that $v(1)=0$.

An example of this is the Galerkin or Rayleigh-Ritz construction.

Consider the following function $\phi(x)$, which is a piecewise linear function. It linearly increases up to $(x_{j},1)$ then linearly decreases to 0 at the next x point.

![[Pasted image 20251104144004.png]]
We approximate the function $u$ in the following way:
$$u(x) \approx U_{0}\phi_{0}(x)+U_{1}\phi_{1}(x)+\dots+U_{n-1}\phi_{n-1}(x)$$
This is an expansion in the trial functions. We choose the test functions as $v(x) = \phi_{0},\phi_{1},\dots,\phi_{n-1}$

The weak form then becomes the following summation:
$$\int_{0}^1c(x)\left( \sum _{i=0}^{n-1}U_{i} \frac{d\phi_{i}(x)}{dx} \right) \frac{d\phi_{j}(x)}{dx} dx = \int_{0}^1f(x)\phi_{j}(x)dx$$
There are n unknowns we are solving for, which are the $U_i$ constants. There are also n equations, represented by each of the $\phi_{j}$ terms. In the next lecture, we will see that we can equivalently express this as a matrix equation of the form KU=F.