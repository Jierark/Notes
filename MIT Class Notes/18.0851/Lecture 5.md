This lecture, we are going to see a very useful tool to help us solve various systems and differential equations: delta functions.

Let's revisit our hanging bar problem from before. Previously, we mapped the gravitational force as $f(x)=\rho(x)g$.
![[Pasted image 20250930145510.png]]
Ideally, we would like to put a point mass at the point a, representing the mass at that point. As a result, we'll use a different function to represent the scenario:
$$f(x) = m\delta(x-a)$$
You can interpret this as a translation of the function $\delta$ by some amount a. If we set m = 1, the final equation we will be looking at is
$$-u''(x) = \delta(x-a)$$ with our usual boundary conditions $u(0) = u(1) = 0$.

The way we will do this is by approximating the mass around the point a:
![[Pasted image 20250930145559.png]]

Let's represent the function as $\delta_\epsilon(x-a)$. Eventually, we'll take a limit as $\epsilon \to 0$. What are some properties of $\delta_{\epsilon}$?
$$\int_{0}^{1} \delta_\epsilon(x-a)dx = 1$$ only when a is between 0 and 1. This should make sense: the only "area" you have under this curve is exactly at point a.
Let's try something more interesting:
$$\int_{0}^{1} \delta_\epsilon(x-a)v(x)dx $$where $v(x)$ is a continuous or smooth function. This quantity will be approximately equal to v(a) if $a\in[0,1]$. You can prove the error of $O(\epsilon)$ by using a Taylor series, similar to the previous lecture. Again, this makes sense: we only have a nonzero integral at the point a, with it's height being determined by v(a) plus some error in $\epsilon$.

Now, let's take our limit from before.
$$\lim_{ \epsilon \to 0 } \int_{0}^{1} \delta_\epsilon(x-a)v(x)dx = v(a) = \int_{0}^1\delta(x-a)v(x)dx$$This is what we will take as our definition of delta functions. In engineering, we express delta in the following way:
$$(\delta:v\to v(a))$$
Note that this definition only holds if our "test function" v is a continuous function; v doesn't have any physical representation, but it just helps us write functions (whatever that really means). These are also known as a Dirac Delta or Point Impulse, because we're taking the entire load and concentrating it at a specific point. You'd see this commonly in quantum mechanics. 

Let's look at some of its properties regarding integration:

Integrating $\delta(x)$ gives the function S(x): a step function with the following characteristics:
$$S(x) = \begin{cases}
0 & x < 0 \\
1 & x \geq 0
\end{cases}$$
Integrating S(x) gives the function R(x): a ramp function with the following characteristics:
$$R(x) = \begin{cases}
0 &  x < 0 \\
x & x\geq 0
\end{cases}$$

Since we have these integrals, we can also look at some derivatives as well. We'll have the following property, then:
$$\delta(x) = \frac{d}{dx}S(x) = \frac{d^2}{dx^2}R(x)$$
This drawing summarizes what we concluded about these functions:
![[Pasted image 20250930145759.png]]
So let's go back to our original problem: 
$$-u''(x) = \delta(x-a)$$$$u'(x)=-S(x-a)+C$$
$$u(x)=-R(x-a)+Cx+D$$
The constants are determined by your initial conditions. Additionally, notice that the equation for $u(x)$ contains a particular solution $-R(x-a)$ and a homogenous solution $Cx+D$.

Let's plug in our initial conditions.
$$u(0) = -R(0) + C(0) + D$$
$$D=0$$
$$u(1) = 0 = -R(1-a)+C(1)$$
$$0=-(1-a)+C$$
$$C = 1-a$$
Our final solution is $$u(x) = -R(x-a)+(1-a)x$$
So we've got a well-behaved solution using this $\delta$. Let's graph what $u(x)$ looks like:
![[Pasted image 20250930145959.png]]
Interesting note: the slope from 0 to a is 1-a, and then it becomes -a afterwards.

At the end, $\delta$ is a tool that just helps with taking a derivative of step functions. 

This construction we have is Green's function. It's a very useful tool for solving some differential equations, especially ones with partial derivatives. It's formally defined as
$$G(x,a) = \begin{cases}
(1-a)x & x<a \\
(1-x)a & x>a
\end{cases}$$This function is interestingly symmetric, meaning $G(x,a) = G(a,x)$

So why is it important to us?

Let's recall what we had before. We were trying to solve
$$-u''(x) = \delta(x-a) + B.C.$$We know the solution to this form is Green's function
$$u(x) = G(x,a)$$
Now, how would we do this over the entire system? Here's what our end goal is:$$-u''(x)=f(x) + B.C.$$
Can we go from this equation to the one above using a delta function?

Let's consider that f(x) is a set of superpositions over all samples: we could evaluate what force acts on each point and then sum it all up to get the total force. Equivalently, we can rewrite f(x) as
$$\int_{0}^1\delta(x-a)f(a)da$$
To solve over the entire system, we can use superposition to do this:
$$u(x)=\int_{0}^{1}G(x,a)f(a)da$$
(Evaluating f(x) at multiple points)

Alright, let's jump back to the discrete world.
![[Pasted image 20251001150057.png]]
Here, we say that there is an impulse at point a. If we discretize this a bit, we'll get
![[Pasted image 20251001150117.png]]
Notice that this value doesn't go to infinity. It stays at 1. This vector we have is a familiar one: it is a one-hot encoding. They're also known as Kronecker deltas and take the form 
$$\delta_{i,j} = \begin{cases}
1 & i=j \\
0 & i \neq j
\end{cases}$$
So we wanted to solve $K\vec{u} = \vec{f}$ using impulses. What does this system look like?
We replace f with each possible $\delta_{ij}$ values and solve for its corresponding $u_{i}$. If we consider them all at once, we get something of the form
$$K\begin{bmatrix}
u_{0} & u_{1} & \dots & u_{n}
\end{bmatrix} = 
\begin{bmatrix}
1 & 0 & \dots & 0 \\
0 & 1 & \dots & 0 \\
\dots & \dots & \dots & \dots \\
0 & 0 & \dots & 1
\end{bmatrix}$$
We're now looking to solve a huge matrix-matrix product that equals the Identity matrix. You can see from observation that the matrix $U$ (that big matrix with entries $u_i$) must be equal to $K^{-1}$. 

A general note - it's very useful to be able to go back and forth between index notation and matrix notation. It's especially important when you start dealing with really complex systems and calculations. We'll do this for our problem above:

We want to solve $(Ku)_{i} = f_{i}$, solving for the i'th column of the matrix U (this i need to double check)
We can rewrite these as
$$\sum_{j}K_{i,j}u_{j} = \sum _{j}\delta_{i,j}f_{j}$$
We get to the same solution as before:
$$U=K^{-1}f$$ Mostly this is a superposition argument.

One conclusion to draw here: Green's function is equivalent to the matrix inverse in a continuous setting. You could actually see a few functions as a result:

$$K_{4} = \begin{bmatrix}
2 & -1 & 0 & 0 \\
-1 & 2 & -1 & 0 \\
0 & -1 & 2 & -1 \\
0 & 0 & -1 & 2
\end{bmatrix}$$
$$K_{4}^{-1} = \frac{1}{5}\begin{bmatrix}
4 & 3 & 2 & 1 \\
3 & 6 & 4 & 2 \\
2 & 4 & 6 & 3 \\
1 & 2 & 3 & 4
\end{bmatrix}$$
- $K_4^{-1} = (K_4^{-1})^T$ because of two things:
	- K is symmetric: $K_4 = K_{4}^T$
	- G was a symmetric function.
- If we plot any of the columns in $K_4^{-1}$, you would see the general shape that G takes on.