Reference book: Computational Science and Engineering, Gilbert Strang (2007), 2nd edition.
Is available on libgen btw (some mirrors won't have it)

# A Special Matrix K
Consider the following system. You have 3 masses, all connected by springs from floor to ceiling. 

# INSERT DRAWING HERE

How does the system react if we move the masses around? Some of the springs will elongate, others will compress. Mathematically, we want to find the values $u_{1}, u_{2}, u_{3}$ from $f_1, f_2, f_3$, where $u_{i}$ is the displacement of spring i, and $f_{i}$ is the sum of forces acting on each mass.

Okay, let's start with some simple equations. Let's define the elongation/strain of each spring as such:
$e_{1} = u_{1}$
$e_{2} = u_{2} - u_{1}$
$e_{3} = u_{3} - u_{2}$
$e_{4} = -u_{3}$
Note that a positive value represents an elongation, while a negative value represents a compression.

It makes more sense to rewrite this as a matrix equation:
$$\begin{gather}
\begin{bmatrix} e_{1} \\ e_{2} \\ e_{3} \\ e_{4} \end{bmatrix}
= \begin{bmatrix}
1 & 0 & 0 \\
-1 & 1 & 0 \\
0 & -1 & 1 \\
0 & 0 & -1
\end{bmatrix} 
\begin{bmatrix}
u_{1} \\
u_{2} \\
u_{3}
\end{bmatrix}
\end{gather} $$
Let's call that matrix A. Now, we're going to draw from physics at the forces.

Take a typical spring. If you pull on it and then let go, you will notice that it will compress and reestablish equilibrium, and vice versa if you compress it. This is basically Hooke's Law, which states that the force of a spring being displaced from equilibrium is $F_{spring} = -kx$, where x is the displacement of the spring, and k is the "stiffness" of a spring.

In terms of a matrix-vector equation, we can model these forces with the following equation
$$\begin{gather}
\begin{bmatrix}
w_{1} \\
w_{2} \\
w_{3} \\
w_{4}
\end{bmatrix} = 
\begin{bmatrix}
c & 0 & 0 & 0 \\
0 & c & 0 & 0 \\
0 & 0 & c & 0 \\
0 & 0 & 0 & c
\end{bmatrix} 
\begin{bmatrix}
e_{1} \\
e_{2} \\
e_{3} \\
e_{4}
\end{bmatrix}
\end{gather}$$
Let's call that matrix C.
(for simplicity, we're going to assume the springs are all the same. I also don't know where the signs went, so we'll just stick it in c even though that's completely wrong)

If you look at the system, you will notice there should be a third force on all of these: gravity. You will also notice that this system is not moving. What does this mean? The sum of forces acting on each mass is 0. So, for each mass, we'll have the following equation for each, representing all forces acting on it:
$w_{i} - w_{i+1} = m_{i}g$
Over all masses, we have the following system of equations:
$$\begin{gather}
\begin{bmatrix}
1 & -1 & 0 & 0 \\
0 & 1 & -1 & 0 \\
0 & 0 & 1 & -1
\end{bmatrix}
\begin{bmatrix}
w_{1} \\
w_{2} \\
w_{3} \\
w_{4} \\
\end{bmatrix} =
\begin{bmatrix}
m_{1}g = f_{1} \\
m_{2}g = f_{2}\\
m_{3}g = f_{3}
\end{bmatrix}
\end{gather}
$$
Hey, we've come full circle. Also, that matrix looks familiar. It's actually just $A^T$. 

To recap where we are,
- We started with equations of displacement.
- We can left multiply by the matrix A to reach elongations.
- We can left multiply that by the matrix C to reach the internal forces.
- We can left multiply that by the matrix $A^T$ to reach all forces.

Here's a special case: let's set c = 1, so the matrix C becomes the identity matrix I. What happens? We'll do a bunch of substitutions and see.
$$
\begin{gather}
A^T w = F \\
A^T C e  = F \\
A^T C A u = F \\
A^T A u = F
\end{gather}
$$
Let's actually compute $A^T A$ .
$$\begin{gather}
\begin{bmatrix}
1 & -1 & 0 & 0 \\
0 & 1 & -1 & 0 \\
0 & 0 & -1 & 1
\end{bmatrix}
\begin{bmatrix}
1 & 0 & 0 \\
-1 & 1 & 0 \\
0 & -1 & 1 \\
0 & 0 & -1
\end{bmatrix} = 
\begin{bmatrix}
2 & -1 & 0 \\
-1 & 2 & -1 \\
0 & -1 & 2
\end{bmatrix}
\end{gather} $$
This matrix is a special matrix known as $K_3$, or $K_n$ in the general case.
