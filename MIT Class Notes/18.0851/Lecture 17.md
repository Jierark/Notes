# Fourier Series
Consider a $2\pi$ periodic function $f(x) = f(x+2\pi)$. Suppose it has an extremely complex waveform (maybe it's representing a sound or something similar). How could you go about doing some amount of analysis on it? It would be difficult to do it on the baseline signal.

Instead, we can express it as an infinite sum of sines as cosines: $$\frac{a_{0}}{2} + \sum_{n=1}^\infty a_{n} \cos(nx) + \sum_{n=1}^\infty b_{n} \sin (nx)$$
This is the Fourier Series of f. It has a basis of $\{\cos(nx),\sin(nx)\}$. How do we get here?

Recall the Eigendecomposition of a matrix. Given a real, symmetric matrix K, K can be rewritten as the following product of matrices
$$K = V \Lambda V^T$$ where
- $V$ is a matrix whose columns represent the eigenvectors of $K$
- $\Lambda$ is a diagonal matrix with the eigenvalues of $K$ as its values.
- The eigenvectors of $K$ are orthogonal and can be made normal (length 1), satisfying the following
	- $VV^T = I \iff v_{i}^Tv_{j}=\begin{cases}0 & i \neq j \\ 1 & i=j\end{cases}$

Connecting this to the Fourier Basis, we want to solve the expression $\frac{d^2}{dx^2}u$ with periodic conditions $u(x)=u(x+2\pi)$. We could discretize the system using a real, symmetric matrix K like before. With a Fourier Series, we can skip the discretization and model $u$ with the Fourier Basis. The Fourier basis are now the "eigenfunctions", and they will also form an orthogonal basis. How do we know this?

# Orthogonality of Functions
Two functions are orthogonal if their inner product is 0. In mathematical notation, this is represented as
$$<f(x),g(x)> = \int_{-\pi}^{\pi}f(x)g(x) dx = 0$$
Think of this as the dot product of two infinitely long vectors. Now, we need to show that $\{\cos(nx),\sin(nx)\}$ is an orthogonal basis.
$$<\cos(nx),\sin(mx)> = \int_{-\pi}^{\pi}\cos(nx)\sin(mx)dx$$
This is always true by definition: sine and cosine will always be orthogonal to each other. One can show this by a symmetry argument:
![[Pasted image 20251118151248.png]]
Red represents cosine, and orange represents sine.

Cosine is an even function: $f(x) = f(-x)$, while sine is an odd function: $f(x) = -f(-x)$. If we multiply even and odd, we get odd. This means the integral above will have to equal 0.

Now, we need to prove orthogonality between the cosines and between the sines. The proofs are similar, so let's look at the cosines:
$$<\cos (nx),\cos(mx)> = \int_{-\pi}^{\pi}\cos(nx)\cos(mx)dx$$
To solve this, we use the following trig identities:
$$\cos(a+b) = \cos a\cos b + \sin a\sin b$$
$$\cos(a-b) = \cos a\cos b - \sin a\sin b$$
If we add them and solve, we get
$$\cos a\cos b=\frac{1}{2}(\cos(a+b) + \cos(a-b))$$
Then we substitute this into our integral:
$$=\frac{1}{2}\int_{-\pi}^\pi \cos((n+m)x) + \cos((n-m)x)dx$$
This has three possible solutions:
- If $n=m$, then the integral evaluates to $\pi$
- If $n \neq m$,then the integral evaluates to 0.
- If $n=m=0$, then the integral evaluates to $2\pi$. This does not matter for orthogonality, but it does factor into the Fourier Series as one of the coefficients. Note that this is only true for cosine and not sine.

To compute the coefficients of the Fourier basis, we can utilize orthogonality:
$$<\cos kx,f(x)> = \frac{a_{0}}{2} <\cos kx,1> + \sum_{n=1}^\infty a_{n}<\cos kx,\cos nx> + \sum_{n=1}^\infty b_{n}<\cos kx,\sin mx>$$
That last summation is 0 by orthogonality. The first term can be included in the summation by starting at $n=0$. So we can rewrite the right side:
$$<\cos(kx),f(x) = a_{k}<\cos kx,\cos kx>$$
$$a_{k} = \frac{1}{<\cos kx,\cos kx>} \int_{-\pi}^\pi \cos (kx)f(x)dx$$
A similar result comes for the sine coefficients. The general formula then is
$$a_{n}=\frac{1}{\pi}\int_{-\pi}^\pi \cos(nx)f(x)dx$$
$$b_{n}=\frac{1}{\pi}\int_{-\pi}^\pi \sin(nx)f(x)dx$$
# A Square Wave
![[Pasted image 20251118155202.png]]
Consider the following function of a square wave. We want to approximate this with a Fourier Series, which would need to be the form
$$SW(x)=b_{1}\sin(x)+b_{2}\sin(2x)+b_{3}\sin(3x)$$
It will look something like this:
![[Pasted image 20251118155704.png]]
To get our coefficients, we compute
$$b_{k}=\frac{1}{\pi}\int_{-\pi}^\pi \sin(kx)SW(x)dx$$
$$=\frac{2}{\pi}\int_{0}^\pi \sin(kx)dx = \frac{2}{\pi}-\frac{1}{k}\cos(kx)|_{0}^\pi$$
$$b_{k}=\frac{2}{\pi k}(-\cos (k\pi)+1)$$
$$b_{k} = \begin{cases}
0 & k  & \text{is even} \\
\frac{4}{\pi k} & k & \text{is odd}
\end{cases}$$
Then $SW(x) = \frac{4}{\pi}\left( \frac{1}{1}\sin x + \frac{1}{3}\sin 3x + \frac{1}{5} \sin 5x + \dots \right)$

Another example: Suppose we have the following graph:
![[Pasted image 20251118160322.png]]
This is a train of delta functions. #TODO finish this

# Parseval's Theorem
Suppose you have the eigendecomposition of $K = V \Lambda V^T$ and some vector $\vec{a}$. Recall that the product $K\vec{a}$ can be thought of as three operations using the eigendecomposition.

$V^T\vec{a}$ finds the coefficients of $\vec{a}$ using the eigenvectors of $K$ as a basis. Then,
$$||a|| = \sqrt{ \sum_{i}a_{i}^2 }$$
$$=\sqrt{ \vec{a}^T\vec{a} }$$
$$=\sqrt{ \vec{a}^TV^TV\vec{a} }$$
We know that $V^TV = I$, so this change in basis does not change the length of the vector.

We can apply a similar idea to Fourier series. The "Length" of $f(x)$ is defined as  $$\sqrt{ <f(x),f(x)>} = \sqrt{ \int_{-\pi}^\pi (f(x))^2 dx }$$
Taking x to be very small, this is approximately equal to the sum of squares of the coefficients:
$$\approx a_{0}^2 + a_{1}^2 + a_{2}^2 +\dots + b_{1}^2 + b_{2}^2$$
We do need to formulate an equivalent idea of normalization first.

Recall the following facts: $$\int_{-\pi}^\pi \cos^2(kx) dx = \begin{cases}
\pi & k>0 \\
2\pi  & k = 0
\end{cases}$$
$$\int_{-\pi}^\pi \sin^2(kx)dx = \pi$$
Then the normalized Fourier Basis is expressed as 
$$\left\{ \frac{1}{\sqrt{ 2\pi }}, \frac{1}{\sqrt{ \pi }} \cos kx, \frac{1}{\sqrt{ \pi }}\sin kx \right\}$$
Now, we can formally define Parseval's Theorem as the following:
$$||f(x)||^2 = <f(x),f(x)>^2 = \frac{\pi}{2}a_{0}^2 + \pi(a_{1}^2 + a_{2}^2 + \dots) + \pi(b_{1}^2 + b_{2}^2 + \dots)$$
To get a sense of how it can be used, we can apply it to the Square Wave:
$$\int_{-\pi}^\pi SW(x)^2 dx = \int_{-\pi}^\pi 1 dx = 2\pi$$
Then, 
$$2\pi = \pi + \frac{16}{\pi}\left( 1^2 + \frac{1}{3^2} + \frac{1}{5^2} + \dots \right)$$
$$\frac{\pi^2}{8}=1^2 + \frac{1}{3^2}+ \frac{1}{5^2} + \dots$$
This gives an interesting approximation of $\pi^2$.
