**Finite Differences**

Goal: going back and forth between -u"(x) = f(x)

# Some More Special Matrices
Suppose we have the following system:
![[Pasted image 20250922144955.png]]
This is a variation of the problem we've been studying, except the last spring is cut (put another way, the hook constant is 0 here).

If we do the analysis from before, we'll have the following:
$$A^TCA = \begin{bmatrix}
1 & -1 & 0 & 0 \\
0 & 1 & -1 & 0 \\
0 & -1 & 1 & 0 \\
\end{bmatrix}
\begin{bmatrix}
1 & 0 & 0 & 0 \\
0 & 1 & 0 & 0 \\
0 & 0 & 1 & 1 \\
0 & 0 & 0 & 0
\end{bmatrix}
\begin{bmatrix}
1 & 0 & 0 \\
-1 & 1 & 0 \\
0 & -1 & 1 \\
0 & 0 & -1
\end{bmatrix}
$$
Simplified, we get this matrix:
$$\begin{bmatrix}
2 & -1 & 0 \\
-1 & 2 & -1 \\
0 & -1 & 1
\end{bmatrix}$$
This is a special matrix called $H_3$. This matrix, along with K, is another common matrix that comes up when discretizing boundary value problems and partial differential equations.

Ok, now consider this system:
![[Pasted image 20250922145039.png]]

Again, we do a similar analysis and get the following matrix C:
$$C_{4} = \begin{bmatrix}
2 & -1 & 0 & -1 \\
-1 & 2 & -1 & 0 \\
0 & -1 & 2 & -1 \\
-1 & 0 & -1 & 2
\end{bmatrix}$$
This is a **circulant**/**cyclic** matrix. It has a special property where each row and column is a cyclic shift of the previous one.
This is a common matrix in signal processing.

What are some properties of C that interest us?

Suppose we wanted to solve $C_{4}\vec{u}=\vec{f}$. We're going to run into some problems here because C is singular. How can we tell this just from the matrix? Try coming up with a vector in the nullspace of C. Looks like  $\begin{bmatrix}1 \\ 1 \\ 1 \\ 1\end{bmatrix}$  works.

This brings a whole slew of consequences.
- C is singular
- C has a determinant of 0
- C has no inverse.
This means the equation $C_{4}\vec{u} = \vec{f}$ can't be solved, unless f satisfies the compatibility condition: $\vec{f}^Tx=0$, (sum of components is 0) meaning this system can't change through time (this might need more clarification).
However, any solution we find that satisfies the compatibility condition won't be unique. We have something in the nullspace which won't change the solutions. Physically, we can rotate the system above and the solution will still be the same.

# The Hanging Bar Problem
Suppose we have this system:
![[Pasted image 20251001141055.png]]

We are interested in the displacement u of each point x. There is some amount of mass that is acting at each point which wants to cause the system to sag. So
$$f(x) = \rho(x)g$$where $\rho(x)$ is a density function, and g is the gravity constant of 9.81 m/s^2 .

Same as before, we want to relate the force f to the displacements u. Notice that this problem is the same as the springs from before, but in a continuous setting.
$$u(x) \to e(x)$$
Let's think about the elongations. Assume that these displacements are uniform and rigid. This won't change the elastic properties of the material. We'll be more interested in the relative changes around these various points of u. Drawing from calculus, we're looking at derivatives. So $$e(x) = u'(x)$$
Similar to before, we relate elongations to internal forces using the C matrix like before:
$$w(x) = Ce(x)$$Instead of C being spring constants, they're more like compressibility constants, which depends on the material being used. C can also be a function of x itself, but for now we'll assume a constant value of 1.

Okay, now let's go from internal forces to total force:
![[Pasted image 20251001141138.png]]
We have 3 forces:
$w(x)$ pulls upwards
$w(x+\Delta x)$ pulls downwards
$\rho (x)g\Delta x = f(x)\Delta x$ is gravity pulling downwards.
These should sum to 0, so we get the equation
$$w(x) - w(x+\Delta x) = f(x)\Delta x$$
Let's rewrite this a bit:
$$f(x) = -\frac{w(x+\Delta)x-w(x)}{\Delta x}$$
This should look familiar. Taking a limit as $\Delta x$ approaches 0, we get 
$$f(x) = -w'(x)$$
Connecting it to u, we have
$$f(x) = -\frac{d}{dx}\left( C * \frac{du}{dx} \right) = -c u''(x)$$

Let's summarize what we have:
![[Pasted image 20251001141233.png]]

We can draw some analogies to our spring system. $A$ is the derivative, and $A^T$ is...the transpose of a derivative?

Our overall system looks like $f(x) = -cu''(x), x \in [0,L]$ For simplicity, set L and c = 0.

Then the system we have is a **Boundary Value Problem**
$$\{ -u''(x)=f(x), x \in [0,1] \}$$
We also need to account for the boundary conditions here. There's many terms for this: fixed-fixed, Zero displacement, Dirichlet boundary conditions, etc.
$$u(0) = u(1) = 0$$
This might look weird to those who have taken a differential equation class. Think about it this way:

Usually we would specify **initial conditions**, something similar to x(0)=0,x'(0)=0 where we were looking at time-dependent differential equations x(t). Note here that we're looking at u(x): equations which depend on the position x. In these problems, we specify **boundary conditions**, the behavior of the system at specific endpoints. Depending on these boundary conditions, we might have to do different computations to get the overall behavior of the system. These will also relate back to the matrices $K_{n},T_{n},B_{n},C_{n}$ that we saw previously.

This is a simple solution to solve by hand. We're going to solve this using finite difference here to determine it.

Let's say that f = 1. Then $$u(x) = \frac{1-x^2}{2}$$
Let's plot this equation, and discretize this plot with a grid.
![[Pasted image 20251001142854.png]]

We're going to define a step size h = $\Delta x$ = $\frac{1}{n+1}$, where n is the number of samples you want to take.

What's a good approximation of $u'(x)$ at some point x? We can draw from calculus and use the difference quotient to approximate derivatives. Here, we'll use x and the next closest point $x+\Delta x$
$$u'(x) \approx \frac{u(x+\Delta x) - u(x)} {\Delta x} = \Delta_{F}u(x)$$
This is called the **Forward Difference**. We can similarly define a **Backward Difference**
$$\Delta_{B}u(x) \approx u'(x) \approx \frac{u(x)-u(x-\Delta x)}{\Delta x}$$
These independently are a little lopsided. We can  get a more accurate difference by finding a **Centered Difference**
$$\Delta_{C}u(x) = \frac {u(x+\Delta x) - u(x - \Delta x)}{2\Delta x}$$
So, how can we quantify how "accurate" our approximations are? We'll say that these differences are an approximation of the derivative, up to some certain order of accuracy. For example,
$$u'(x) = \Delta_{F}u(x) + O(\Delta x)$$
Hopefully you remember how big O notation works.
The backward differences is similar:
$$u'(x) = \Delta_{C}u(x) + O(\Delta x)$$
The centered difference is nice in that the error is smaller:
$$u'(x) = \Delta_{C}u(x) + O((\Delta x)^2)$$
We say that the centered difference has **Second order accuracy**, whereas the forward and backward differences have **First order accuracy**

In practice, second order accuracy is more preferable to first order since $\Delta x$ is very small anyways.

We're going to see why these errors are correct using a Taylor expansion. Taylor expansions are really useful for computing function values at points very close to each other.

$$u(x+\Delta x) = u(x) + u'(x)\Delta x + \frac{1}{2}u''(x)(\Delta x)^2 + O((\Delta x)^3)$$ That term at the end is where the "..." usually sits, just written more formally. Technically, you would need the function to be infinitely differentiable for this to converge, but in practice we don't need that many derivatives, just enough for this order of accuracy to be nonzero.

$$u(x-\Delta x) = u(x) -u'(x)\Delta x + \frac{1}{2}u''(x)(\Delta x)^2 + O((\Delta x)^3)$$
Let's subtract these equations:
$$u(x+\Delta x) - u(x + \Delta x) = u'(x)2\Delta x + O((\Delta x)^3)$$Divide this equation by $2\Delta x$ to get our answer
$$\frac{u(x+\Delta x)-u(x-\Delta x)}{2\Delta x} = u'(x)+O((\Delta x)^2)$$
In general, this is how you prove the accuracy of finite difference schemes using Taylor expansions.

# Approximating 2nd Derivatives
So far we have the following derivatives, slightly rewritten in a way that computers usually see it:
$$\Delta_{F}u = \frac{u_{i+1}-{u_{i}}}{h}$$$$\Delta _{B}u = \frac{u_{i}-u_{i-1}}{h}$$
$$\Delta_{C}u=\frac{u_{i+1}-u_{i-1}}{2h}$$
How can we approximate $u''(x)$ with these? Let's just apply another difference, similar to above:
$$u''(x) \approx \frac{u'(x+\Delta x)-u'(x)}{\Delta x}$$
$$=\frac{\frac{u(x+\Delta x)-u(x)}{\Delta x} - \frac{u(x)-u(x-\Delta x)}{\Delta x}}{\Delta x}$$
$$u''(x)\approx \frac{u(x+\Delta x)-2u(x)+u(x-\Delta x)}{(\Delta x)^2} = D_{2}u(x)$$
If we apply the same approximation argument as we did above,
$$u''(x) = D_{2}u(x) + O((\Delta x)^2)$$

In a discrete form, we can rewrite this as
$$(D_{2}u)_{i} = \frac{u_{i+1}-2u_{i}+u_{i-1}}{h^2}$$
Recall our boundary value problem:
$$f(x)=-u''(x)$$
Do those coefficients look familiar? Yep, there's the distinct -1 2 -1 that we saw in the K matrix.

So what was the point of all this work? Typically, we start with $f(x)$ for some number of x. Symbolically, we want to find $u(x)$, but numerically we don't need the whole function. We can do what we did above to get an approximate solution, which is usually good enough. Let's see how this works.

We have $f_i = f(u_{i})$ for some points $u_1\dots u_{4}$. We now want to find those $u_{i}$:
$$\frac{1}{h^2}\begin{bmatrix}
2 & -1 & 0 & 0 \\
-1 & 2 & -1 & 0 \\
0 & -1 & 2 & -1 \\
0 & 0 & -1 & 2 \\
\end{bmatrix}
\begin{bmatrix} 
u_{1} \\
u_{2} \\
u_{3} \\
u_{4}
\end{bmatrix}=
\begin{bmatrix}
f_{1} \\
f_{2} \\
f_{3} \\
f_{4}
\end{bmatrix}$$
More succintly,
$$\frac{1}{h^2}K_{4}\vec{u}=\vec{f}$$
Remember, this is the fixed-fixed case. This is the case whether the system is a bunch of masses on springs or a discretized set of points on a hanging bar. You can even tell from the corners that both ends are fixed. Let's suppose they weren't. Then, you would account for that by changing the ends and dimensions of the matrix K.

# Hanging Beam
Let's now consider a fixed-free system (a hanging beam)
![[Pasted image 20251001144000.png]]

Doing the analysis before, we'll find the same equation
$$-u''(x) = f(x)$$
But we need to make some slight adjustments for the boundary conditions:
- Top left corner stays the same, so we leave it as is
- The bottom right corner changes, since we don't have an internal force acting at that point: $$w(1) = 0$$
- Equivalently, $$cu'(1) =0$$
- So we now need to change the boundary condition at x=1 to $u'(1)=0$ as opposed to what we had before.
What does this change? Well, we don't have a forward derivative at x=1, so
$$\frac{u_{4}-u_{3}}{h^2}=0$$ (Technically should be h in the denominator, but the bottom part doesn't change anything since it equals 0 anyways)
Now we can reassemble the matrix:
$$\frac{1}{h^2}\begin{bmatrix}
2 & -1 & 0 & 0 \\
-1 & 2 & -1 & 0 \\
0 & -1 & 2 & -1 \\
0 & 0 & -1 & 1
\end{bmatrix}
\begin{bmatrix}
u_{1} \\
u_{2} \\
u_{3} \\
u_{4}
\end{bmatrix}$$
Now we can identify this is a fixed-free system because of the 2 in the upper left corner and the 1 in the lower right corner. We're building up to how to interpret that special collection of matrices we've been seeing repeatedly. $K_{n}$ describes a fixed-fixed system, and $T_{n}$ describes a free-fixed system.