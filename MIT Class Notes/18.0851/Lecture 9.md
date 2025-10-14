The problem setting is this: we are developing a game in Roblox (I know, bear with me). We have some particles that we are trying to move in a specific way, but find that they keep blowing up to infinity and aren't following the intended behavior. Luckily, we are mathematicians and can figure out why this is the case using what we have been learning over the last few weeks.

The particles have some forces applied to it and are modeled by the following differential equation:
$$m \frac{d^2\vec{x}}{dt^2} = F\left( \vec{x},\frac{d\vec{x}}{dt} \right) = -b \frac{d\vec{x}}{dt} - \nabla V(x)$$The first quantity represents friction, and the second quantity represents potential energy, which depends on the system you are trying to model;
Gravity uses the potential $V(x) = \frac{Gm_{0}m}{||\vec{x}-\vec{x_{0}}||}$
Springs use the potential $V(x) = \frac{1}{2}kx^2$
In our setting, we use this rough idea of a potential function:![[Pasted image 20251013172539.png]]
The goal is to keep the particles behind the player model at all times.

We model this as the following first order system:
$$\dot{\vec{x}} = \vec{v}$$
$$\dot{\vec{v}} = \frac{1}{m}F(\vec{x},\vec{v})$$
# Forward Euler Method
We could solve this directly to get the solution, but there's a better way we can go about solving this system: drawing from finite differences.

Suppose we have $u(t_{n}), u(t_{n-1}),\dots,u(0)$ and want to find $u(t_{n+1})$. We apply the forward difference scheme and get the result
$$f(u(t_{n}),t_{n})=\frac{du}{dt}(t_{n})\approx \frac{u(t_{n+1})-u(t_{n})}{h}$$
Solving for $u(t_{n+1})$ results in the approximation
$$u(t_{n+1}) \approx u(t_{n})+h \cdot f(u(t_{n}),t_{n})$$
In our example, the approximations become
$$\vec{x}_{n+1} = \vec{x}_{n} + h \cdot \vec{v}_{n}$$
$$\vec{v}_{n+1} = \vec{v}_{n} + \frac{h}{m} \cdot F(\vec{x}_{n},t_{n})$$
Since we're working in 3D, there are 6 total equations contained in this system.

When analyzing these systems, we are interesting in two properties:
- Convergence: as $h \to 0$, we should approach the continuous solutions
- Stability - $|u(t)| \leq M \to |u_{n}| \leq M': M,M' > 0$ (the values should be bounded and not grow to infinity as t goes to infinity)

Let's look at another example to understand these properties. Consider the typical spring-mass system with the equations:
$$\ddot{u}+u=0$$
$$\dot{u}=v$$
$$\dot{v}=-u$$
The solution to this system is of the form $u(t)=A\cos t+B\sin t$.
The forward Euler method results in
$$u_{n+1}=u_{n}+hv_{n}$$
$$v_{n+1}=v_{n}-hu_{n}$$
We express this as a matrix-vector product:
$$\begin{bmatrix}
u_{n+1} \\
v_{n+1}
\end{bmatrix}
=\begin{bmatrix}
1 & h \\
-h & 1
\end{bmatrix}
\begin{bmatrix}
u_{n} \\
v_{n}
\end{bmatrix}$$
How do we understand stability? We can rewrite the equation above in terms of the initial conditions $u_{0},v_{0}$ and repeated matrix products. Define the matrix above as $G_{FE}$. The equation above is then rewritten as
$$\begin{bmatrix}
u_{n+1} \\
v_{n+1}
\end{bmatrix}
=G_{FE}^n\begin{bmatrix}
u_{0} \\
v_{0}
\end{bmatrix}$$
Stability depends on how the matrix $G_{FE}^n$ grows. Let's look at that:

Diagonalize the matrix, becoming $G_{FE}^n=S\Lambda^nS^{-1}$. Stability means this matrix stays bounded, or it converges to 0.

Firstly, we know that the matrix isn't symmetric. The eigenvalues may be complex. That means we want the following to hold for the system to be stable:
$$|\lambda_{1,2}| < 1 \to |\lambda_{1,2}^n| = |\lambda_{1,2}|^n<1$$
Note that this is a strict bound and does not include why. The matrix $$\begin{bmatrix}
1 & 1 \\
0 & 1
\end{bmatrix}$$
serves as a counter example for why the eigenvalues cannot be 1.

The eigenvalues of the matrix will end up being $1 \pm ih$, meaning $|\lambda_{\pm}| = 1 + h^2 > 1$. This means the system is unstable and will grow as $n \to \infty$. If you used this rule to update particle positions, you would find that they go flying all over the screen.

# Backward Euler
To address this instability, we change our timesteps to the following:
$$u_{n+1}=u_{n}+hv_{n+1}$$
$$v_{n+1}=v_{n}-hu_{n+1}$$This is the backward Euler method, or the "implicit" method. We now solve for the n-th term in terms of the n+1'th term:
$$u_{n} = u_{n+1}-hv_{n+1}$$
$$v_{n} = v_{n+1}+hu_{n+1}$$
Convert to matrix form:
$$\begin{bmatrix}
u_{n} \\
v_{n}
\end{bmatrix}=\begin{bmatrix}
1 & -h \\
h & 1
\end{bmatrix}
\begin{bmatrix}
u_{n+1} \\
v_{n+1}
\end{bmatrix}$$
Now, we just invert the matrix to get the updates for the n+1'th terms:
$$\begin{bmatrix}
u_{n+1} \\
v_{n+1}
\end{bmatrix}=\begin{bmatrix}
1 & -h \\
h & 1
\end{bmatrix}^{-1}
\begin{bmatrix}
u_{n} \\
v_{n}
\end{bmatrix}$$
We'll note this matrix as $G_{BE}$. Notice that it is the transpose of the previous matrix $G_{FE}$. That means the two matrices will share eigenvalues, simplifying the calculations a bit.
$$\lambda_{\pm} = 1 \pm ih$$
The inverse of a matrix has eigenvalues of $\frac{1}{\lambda}$, so the matrix $G_{BE}^{-1}$ has eigenvalues
$$\lambda_{\pm} = \frac{1}{1\pm ih}$$
This value is bounded, regardless of the value of h, so it is stable.

# Phase Planes
So what are these Euler methods actually doing? If you remember the phase plane, it describes the behavior of a differential equation across different points:
![[Pasted image 20251013232347.png]]
The Euler methods are an approximation of the derivative, using discrete time steps.
This is what forward Euler is doing to this system:
![[Pasted image 20251013232607.png]]
As you can see, this method takes large steps which are tangential to the circle.
As for Backward Euler, it looks like this:
![[Pasted image 20251013232635.png]]
These steps are taken such that the endpoint are tangential to the circle.

# Leap Frog/Verlet Method
This method is a combination of forward and backward Euler, with the following update rules:
$$u_{n+1}=u_{n}+hv_{n}$$
$$v_{n+1}=v_{n}-hv_{n+1}$$
This results in an interesting matrix equation to solve:
$$\begin{bmatrix}
1 & 0 \\
h & 1
\end{bmatrix}
\begin{bmatrix}
u_{n+1} \\
v_{n+1} \\
\end{bmatrix}=
\begin{bmatrix}
1 & h \\
0 & 1
\end{bmatrix}
\begin{bmatrix}
u_{n} \\
v_{n}
\end{bmatrix}$$
$$
\begin{bmatrix}
u_{n+1} \\
v_{n+1} \\
\end{bmatrix}=
\begin{bmatrix}
1 & 0 \\
h & 1
\end{bmatrix}^{-1}
\begin{bmatrix}
1 & h \\
0 & 1
\end{bmatrix}
\begin{bmatrix}
u_{n} \\
v_{n}
\end{bmatrix}$$
$$
\begin{bmatrix}
u_{n+1} \\
v_{n+1} \\
\end{bmatrix}=
\begin{bmatrix}
1 & h \\
-h & 1-h^2
\end{bmatrix}
\begin{bmatrix}
u_{n} \\
v_{n}
\end{bmatrix}$$
Call this matrix $G_{LF}$
This matrix has a determinant of 1. This implies the possible relationship between the eigenvalues as:
- $\lambda_{1},\lambda_{2}=1$
- $\lambda_{1}>1, \lambda_{2}<1=\frac{1}{\lambda_{1}}$
- Or $\lambda_{\pm}=\rho e^{\pm i\theta}$
If $h \leq 2$, then the eigenvalues are 1, and the phase plot looks something like this:
![[Pasted image 20251014003950.png]]
This operation preserves area, and is what we should use in the game script to handle the physics of the particles to keep them behind the player.