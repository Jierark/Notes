This lecture we will be looking at eigenvalues and eigenvectors. These are super important in modeling dynamic systems, where many things could be changing over time. A common example is protein folding - the interest is to look at how a given strand of protein might fold due to the various forces acting on the atoms and molecules comprising it. The system will continue to change until it reaches a threshold of minimum energy, which marks stability.

For simplicity, consider a special case: a linear system that is time invariant
$$\frac{d\vec{u(t)}}{dt} = A\vec{u(t)}$$ where $\vec{u(t)} = \begin{bmatrix}u_{1}(t) \\ u_{2}(t) \\ \dots \\ u_{n}(t)\end{bmatrix}$
By time invariant, we mean that the matrix A is constant and does not depend on time.

How do we commonly solve these? Eigenvalues and eigenvectors.

# Eigenvalues and Eigenvectors
Let's look at the following equation:
$$A\vec{y}=\lambda \vec{y}$$
We say that $\lambda$ is an eigenvalue when it satisfies the relationship above. The vector $\vec{y}$ that allows this to work is it's associated eigenvector. Another way to say this is that the matrix A scales the vector $\vec{y}$ by some scalar factor $\lambda$, drawing from the fact that matrices represent some transformation of the vector. Now, a trivial solution to this equation would be $\vec{y} = \vec{0}$, but we do not consider $\vec{0}$ as an eigenvector. However, 0 CAN be an eigenvalue, and it has large implications on the system.

Here's a property of eigenvalues and eigenvectors: What happens if we repeatedly apply A to this?
$$A^2\vec{y} = (AA)\vec{y} = A(A\vec{y}) = A(\lambda \vec{y}) = \lambda A\vec{y} = \lambda^2\vec{y}$$
The eigenvalues will scale accordingly depending on how many repeated multiplications you do. Additionally, one can prove that the following is true:

$$A^{-1}\vec{y} = \frac{1}{\lambda}\vec{y}$$
Okay, let's go back to our original problem:
$$\frac{d\vec{u}(t)}{dt} = A\vec{u}(t)$$
What if we set $\vec{u} = \vec{y}$? That wouldn't do anything, because $\frac{d\vec{y}}{dt} = 0$.

There needs to be some time component to u for this derivative to hold. So let's assume for now the following:
$$\vec{u}(t) = c(t)\vec{y}$$
This is considered the "normal mode". c(t) will contain our time-dependent coefficients. So let's plug this into our derivative and see what happens:
$$\frac{d\vec{u}}{dt} = \frac{dc(t)}{dt}\vec{y}$$
$$A\vec{u}(t) = Ac(t)\vec{y} = c(t)A\vec{y} = c(t)\lambda \vec{y}$$
So we can conclude from this the following relationship:
$$\frac{dc(t)}{dt} = \lambda c(t)\vec{y}$$
The solution to this differential equation is the exponential function:
$$c(t) = e^{\lambda t}$$
So our final equation is the following:
$$\vec{u}(t)=e^{\lambda t}\vec{y}$$
The exponential function has multiple possible behaviors, depending on the eigenvalues. Here's a plot which showcases the possible states:
![[Pasted image 20251001150552.png]]
Let's take a look at the complex solutions. Suppose that $\lambda=a+ib$. Using Euler's rule, we can split the exponential e into a sine/cosine pair:
$$e^{\lambda t} = e^{(a+ib)t} = e^{at}e^{ibt}$$
$$=e^{at}(\cos(bt)+i\sin(bt))$$
This should look familiar from a differential equation class (Euler's Formula. Might be important to remember)

Okay, we have the tools to solve some interesting systems. Let's consider this system:
![[Pasted image 20251001151100.png]]

We have the following equations to define this system:
$$\dot{x}(t)=v(t)$$
$$\dot{v}(t)=\frac{1}{m}F(x(t))$$where F(x) = $-\frac{k}{m}x$, the spring force.
Just to note, the dot notation means we are looking at derivatives with respect to time.
We can model this system using vectors. Let's define $\vec{u}(t)$ in the following way:
$$\vec{u}(t)=\begin{bmatrix}
x(t) \\
v(t)
\end{bmatrix}=\begin{bmatrix}
u_{1}(t) \\
u_{2}(t)
\end{bmatrix}$$
Then our differential equation becomes the matrix equation of
$$\frac{d\vec{u}(t)}{dt}=\begin{bmatrix}
0 & 1 \\
-\frac{k}{m} & 0
\end{bmatrix}
\vec{u}(t)$$
We will also need to define an initial condition $x(0) = x_{0}$, and let's call that matrix A.
(If you need clarification on where the matrix come from, each row of the vectors represents each of the differential equations that we know).

We can solve this system by using eigenvalues and eigenvectors. Let's build the tools and intuition to determine how we do this.

Go back to our equation of eigenvalues and eigenvectors:

$$A\vec{y}=\lambda \vec{y}$$
Rearrange these terms a bit:
$$A\vec{y}-\lambda \vec{y}=\vec{0}$$
$$(A-\lambda I)\vec{y}=\vec{0}$$
Here's a question: is this easy to solve?

Well, we have the trivial solution of $\vec{y}=\vec{0}$, but this is not a very interesting solution. Reinterpret the equation above. It should look somewhat familiar...

This equation tells us that $\vec{y}$ is in the null space of the matrix $A-\lambda I$, so we want a nontrivial null space to have meaningful solutions.

Let's suppose that $\lambda$ is not an eigenvalue. Then the matrix $A-\lambda I$ is invertible, and we have a unique and trivial solution to the null space which is $\vec{0}$. 

If $\lambda$ is an eigenvalue, then the matrix is singular. Now, we are going to have a harder time solving this system since we have a null space. How do we find our eigenvalues, anyways?
In typical settings, we would solve $\det(A-\lambda I) = 0$ to find our eigenvalues, but in practice, there are ways to find a certain number of eigenvalues.

In this problem the determinant we look to solve is $\lambda^2 + \frac{k}{m} = 0$. Our roots are imaginary, and $\lambda = \pm i\sqrt{ \frac{k}{m} }$.

We can redefine the quantity $\sqrt{\frac{k}{m}}$ as the value $\omega$, representing angular frequency. As an aside, $\omega = 2\pi f$ where f is in hertz.

Okay, we have our eigenvalues. Now, we need to find our associated eigenvectors.

(Some of the notation in this section might get confusing. We're going to combine both the solutions into one just to save on writing space).

To get our eigenvectors, we need to solve the matrix-vector equation
$$\begin{bmatrix}
\mp i\omega  & 1 \\
-\omega^2 & \mp i\omega
\end{bmatrix}
\vec{y}_{\pm} = \vec{0}$$
Notice that the matrix is singular. Row 2 is a multiple of row 1 by a factor of $\mp i\omega$. So we only need to solve the equation
$$\mp i\omega y_{1_{\pm}} + y_{2_{\pm}} = 0$$
We'll get to the solution $$y_{2_{\pm}} = \pm i\omega y_{1_{\pm}}$$
Our eigenvector has the form
$$\vec{y}=\begin{bmatrix}
y_{1_{\pm}} \\
\pm i\omega y_{1_{\pm}}
\end{bmatrix}
=y_{1_{\pm}}\begin{bmatrix}
1 \\
\pm i\omega 
\end{bmatrix}$$
where $y_{1_{\pm}}$ is a constant value.

Ok, let's recap what we have.
$$\vec{u}(t) = c_{\pm}(t)y_{\pm}(t)$$where $c_{\pm}(t) = e^{\lambda_{\pm}(t)}$, and $y_{\pm}(t)$ are the associated eigenvector for each eigenvalue.
Our final equation for the spring system above is the following:
$$\vec{u}(t)=e^{\pm i\omega t}\begin{bmatrix}
1 \\
\pm i\omega 
\end{bmatrix}$$
You might be concerned that there are imaginary components to the solution, but it turns out that these aren't a big deal because you can apply superposition in the following way:
$$\vec{u}(t)=a_{+}\vec{u}_{+}(t) + a_{-}\vec{u}_{-}(t)$$
Eventually, using the initial conditions of x(0), v(0), you would need to solve the following matrix equation:
$$\begin{bmatrix}
1 & 1 \\
i\omega & -i\omega
\end{bmatrix}
\begin{bmatrix}
a_{+} \\
a_{-} \\
\end{bmatrix} = \begin{bmatrix}
x(0) \\
v(0)
\end{bmatrix}$$
This still holds, even if A is time-dependent. However, this will not hold if either of the following conditions hold:
- There is nonlinearity in the system
- The derivative depends on a constant vector $\begin{bmatrix}\alpha \\ \beta \\ \end{bmatrix}$
# Core Properties of Eigenvalues and Eigenvectors
Here is a list of properties that will be useful to keep in mind, especially for the next lecture:

- If $A=A^T$ (symmetric), then the following properties will hold:
	- All of A's eigenvalues are real numbers.
	- All of A's eigenvectors are orthogonal.
	- There are exactly n eigenvectors.

- Consider the matrix A = $\begin{bmatrix}1 & 1 \\ 0 & 1\end{bmatrix}$ (known as a Jordan Block)
	- A only has an eigenvalue of 1, and it's associated eigenvector is $\vec{y}=\begin{bmatrix}1 \\ 0\end{bmatrix}$
- The trace of A is equal to the sum of its eigenvalues.
- The determinant of A is the product of its eigenvalues, and it is also equal to the product of the pivots. A useful property for 2x2 matrices.
- Some eigenvalues can be repeated, having a higher algebraic multiplicity. This affects the total number of eigenvectors you will have.