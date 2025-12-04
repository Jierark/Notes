To review the last few lectures:

A Fourier series is the expansion of a function as

$$f(x) = \frac{\pi}{2}\sum_{k\in \mathbf{Z}}c_{k}e^{ikx}$$ where 
$$c_{k} = \int_{-\pi}^\pi f(x)e^{-ikx}dx$$
The Discrete Fourier Transform is the following

Given a vector $\vec{f}$,
$$c_{k} = \sum_{j=0}^{n-1} f_{j}e^{-i2\pi}$$
#clarify check sign
To reform the components of $\vec{f}$,
$f_{j} = \frac{1}{N} \sum_{k=0}^{n-1} c_{k}e^{2\pi ijk/N}$

<!-- Examples of different functions as Fourier Series-->

# Matrices and the Inverse Fourier Transform
Recall from above the inverse Fourier Transform:

$$f_{j} = \frac{1}{N}\sum w^{jk}c_{k}$$
Here, we introduce the root of unity $$w = e^{2\pi i/N}$$
Then, we can equivalently rewrite this as the following matrix-vector product:
$$\vec{f} = \frac{1}{N}S\vec{c}$$
where
$$S_{j,k} = w^{j,k}$$

A concrete example for N=4:

$$w = e^{i\pi/2} = i$$
The matrix $S$ will take the following form:
$$S = \begin{bmatrix}
1 & 1 & 1 & 1\\
1 & w & w^2 & w^3 \\
1 & w^2 & w^4 & w^6 \\
1 & w^3 & w^6 & w^9
\end{bmatrix}$$
To go from $\vec{f}$ to $\vec{c}$,
$$\vec{c} = \bar{S}^T\vec{f}$$
Note the use of the complex conjugate as opposed to the inverse that would normally be used in eigendecomposition. Using the fact that $\bar{w} = -w$,
$$\bar{S}^T = \bar{S} = \begin{bmatrix}
1 & 1 & 1 & 1 \\
1 & -w & w^2 & -w^3 \\
1 & w^2 & w^4 & w^6 \\
1 & -w^3 & w^6 & -w^9
\end{bmatrix}$$
Let's look at a few examples:

$$\vec{f}=\begin{bmatrix}
1 \\
0 \\
0 \\
0
\end{bmatrix}$$
(An impulse).
Then $\vec{c}=\bar{S}\vec{f} = \begin{bmatrix}1 \\ 1 \\ 1 \\ 1\end{bmatrix}$

For a different impulse $\vec{f}=\begin{bmatrix}0 \\ 1 \\ 0 \\ 0\end{bmatrix}$, $\vec{c}=\begin{bmatrix}1 \\ -w \\ w^2 \\ -w^3\end{bmatrix} = \begin{bmatrix}1 \\ e^{-i\pi/2} \\ e^{i\pi} \\ e^{-3i\pi/2}\end{bmatrix}$

$$\vec{f} = \begin{bmatrix}
1 \\
1 \\
1 \\
1
\end{bmatrix} $$
has coefficients of
$$\vec{c}=\begin{bmatrix}
4 \\
0 \\
0 \\
0
\end{bmatrix}$$
This is based on rules of complex exponentials. Terms will cancel out.
$$\vec{f}=\begin{bmatrix}
1 \\
w \\
w^2 \\
w^3
\end{bmatrix}$$
transforms to $$\vec{c} = \begin{bmatrix}
0 \\
4 \\
0 \\
0
\end{bmatrix}$$
# Properties of Manipulating Fourier Series
Here's a few useful properties of manipulating Fourier Series

$$\frac{d}{dx}* = *ik$$
$$x-a = * e^{-ika}$$
$$*e^{ixa} = k-a$$
Orthogonality: $$\frac{1}{N}\bar{S}^TS = \frac{1}{N}S \bar{S}^T = I$$
Conservation of Energy can be derived from this: $$||\vec{f}||^2 = \frac{1}{N}||\vec{c}||^2$$
The idea is that going from $\vec{f}$ to $\vec{c}$ is really a rotation.

# Convolution
Given two functions $f(x),g(x), x \in [-\pi, \pi]$, what does $f(x)g(x)$ look like?

Let's break each one down into its Fourier Expansion (using $n=3$):
$$f(x)=\frac{1}{2\pi}\sum_{k=0}^3 c_{k}e^{ikx}$$
$$g(x) = \frac{1}{2\pi} \sum_{k=0}^3 d_{k}e^{ikx}$$
Then
$$f(x)g(x) = \sum_{l=0}^?h_{l}e^{ilx}$$
For $l=0$,
$$h_{0} = c_{0}d_{0}$$
$$e^{i0x} = e^{i0x}e^{i0x}$$
from the original summation.
Now for $l=1$,

$$e^{1ix} = e^{1ix} e^{0ix} \space \text{or} \space e^{0ix}e^{1ix}$$
The corresponding coefficient would be $$h_{1}=c_{1}d_{0} + c_{0}d_{1}$$
The general form of this is
$$h_{l} = c_{0}d_{l} + c_{1}d_{l-1} + c_{2}d_{l-2} + \dots + c_{l}d_{0} = \sum_{k=0}^3 c_{k}d_{l-k}$$
The summation term is what we call convolution, represented as $(c*d)_{l}$. These values are the Fourier coefficients for $f(x)g(x)$. These are commonly used in signal processing.

This product looks like the following:
$$\begin{bmatrix}
h_{0} \\
h_{1} \\
h_{2} \\
h_{3} \\
h_{4} \\
h_{5} \\
h_{6}
\end{bmatrix} = \begin{bmatrix}
c_{0} & 0 & 0 & 0 \\
c_{1} & c_{0} & 0 & 0 \\
c_{2} & c_{1} & c_{0} & 0 \\
c_{3} & c_{2}  & c_{1} & c_{0} \\
0 & c_{3} & c_{2} & c_{1} \\
0 & 0 & c_{3} & c_{2} \\
0 & 0 & 0 & c_{3}
\end{bmatrix}
\begin{bmatrix}
d_{0} \\
d_{1} \\
d_{2} \\
d_{3}
\end{bmatrix}$$
The bounds of this sum will be 0 to 6. The matrix takes the form of a Toeplitz matrix. Finite differences are also a kind of convolution.

# Convolution in Discrete Settings
We now move to the discrete setting, where functions become fixed length discrete vectors. The framework we will settle on is the following:
![[Pasted image 20251203145020.png]]
The Fourier expansions will take the following form:
$$f_{j}= \sum_{k=0}^{N-1}w^{jk}c_{k}$$
$$g_{j} = \sum_{k'=0}^{N-1}w^{jk'}d_{k'}$$
where the root of unity is $w=e^{2\pi i/N}$
Then $f_{j}g_{j}$ will take the form of $$f_{j}g_{j} = \sum_{l=0}^{N-1}w^{jl}h_{l}$$
Let's use the example N=4.
$l=0$:
$$h_{0} = c_{0}d_{0} \to w^{j0}\cdot w^{j 0} = w^{j 0}$$
This is obvious from before, but in a discrete setting, this also works:
$$w^{j1} \cdot w^{j 3} = e^{2\pi i/4}.e^{3\cdot 2\pi i/4} = e^{2\pi i} = 1$$
Other combinations of power adding to 4 also work. Then,
$$h_{0} = c_{0}d_{0} + c_{1}d_{3} + c_{2}d_{2} + c_{3}d_{1}$$ These combinations add to 0 mod 4(N).
Similarly,
$$h_{1}=c_{0}d_{1} + c_{1}d_{0} + c_{2}d_{3} + c_{3}d_{2}$$
and so on...
We are looking only for combinations of k, k' such that k + k' = l mod N
$$\begin{bmatrix}
h_{0} \\
h_{1} \\
h_{2} \\
h_{3}
\end{bmatrix} = \begin{bmatrix}
c_{0} & c_{3} & c_{2} & c_{1} \\
c_{1} & c_{0} & c_{3} & c_{2} \\
c_{2} & c_{1} & c_{0} & c_{3} \\
c_{3} & c_{2} & c_{1} & c_{0}
\end{bmatrix} \begin{bmatrix}
d_{0} \\
d_{1} \\
d_{2} \\
d_{3}
\end{bmatrix}$$
The matrix is now a circulant matrix, and this is quite different than before.
More compactly, the formula becomes
$$h_{l} = \sum_{k=0}^{N-1}c_{k}d_{l-k (mod N)}$$
This is known as cyclic convolution, represented by the following:
$$(c ⊛d)_{l}$$