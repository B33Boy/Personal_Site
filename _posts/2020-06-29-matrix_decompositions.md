---
title: Matrix Factorizations
layout: post
summary: A brief guide to matrix factorizations including Eigendecomposition, Singular Value Decomposition (SVD), LU and QR
author: Abhi Patel
date: '2020-06-29'
category: misc
thumbnail: "/assets/img/posts/svd.jpg"
---


### [LU Decomposition](https://github.com/B33Boy/Data-Driven-Algorithms-Collection/blob/master/Matrix%20Factorizations/LU%20Decomposition.ipynb)

When working with linear systems $Ax=b$ where A is a large matrix, it is often computationally expensive to find $A^{-1}$ to solve for $x$ (i.e. $x=A^{-1}b$). LU decomposition simplifies gaussian elimination.

We want to break A into lower (L) and upper triangular matrices (U) such that $A = LU$. When row reducing, we keep track of coefficients that are used to multiply rows as values for the lower triangular matrix, while the original array becomes upper triangular.

$$
A = \begin{bmatrix}
  l_{1,1} & ... & 0 \\
  \vdots & \vdots & \vdots \\  
  l_{1,n} & ... & l_{n,n} \\
\end{bmatrix}
\begin{bmatrix}
  u_{1,1} & ... & u_{n,1} \\
  \vdots & \vdots & \vdots \\  
  0 & ... & u_{n,n} \\
\end{bmatrix}
$$

We often see [partial pivoting](https://en.wikipedia.org/wiki/LU_decomposition#LU_factorization_with_partial_pivoting) used in practice which incorporates a third permutation matrix such that $PA = LU$. Partial pivoting is used to reduce round-off errors. Partial pivoting is the process of swapping the rows such that the first element at the top will have the largest magnitude.

Once we have matrices $L$ and $U$:
<center>
We want to solve for $x$ in $Ax=b$ <br>
Let $A=LU$ <br>
$LUX=b$  <br>
Let $Ux=y$ <br>
So $Ly=b$ <br>
Solve for $y$ <br>  
Substitute $y$ in $Ux=y$ <br>  
Solve for $x$ <br>  

</center>

### [QR Decomposition](https://github.com/B33Boy/Data-Driven-Algorithms-Collection/blob/master/Matrix%20Factorizations/QR%20Decomposition.ipynb)

##### QR Decomposition
If a $m \times n$ matrix $A$ contains linearly independent column vectors, then we can express $A$ as $A = QR$ where $Q$ is an $m \times n$ matrix with orthonormal column vectors and $R$ is a $n \times n$ upper triangular matrix.

We can represent this as two matrices:

$$
Q = \begin{bmatrix}
  q_{1,1} & ... & q_{n,1} \\
  \vdots & \vdots & \vdots \\  
  q_{1,n} & ... & q_{n,n} \\
\end{bmatrix}
$$
Representing the orthonormal vectors that form a basis in $R^n$

$$
R = \begin{bmatrix}
    \langle {a_{1}, q_{1}} \rangle & ... & \langle {a_{n}, q_{1}} \rangle \\
    \vdots & \vdots & \vdots \\  
    \langle {a_{1}, q_{n}} \rangle & ... & \langle {a_{n}, q_{n}} \rangle \\
\end{bmatrix}
$$
Representing the inner products that are multiplied with the basis vectors to form $A$

But because $q_{j}$ is orthogonal to $u_{j}, ..., u_{n}$, we are left with an upper triangular matrix:
$$R = \begin{bmatrix}
    \langle {a_{1}, q_{1}} \rangle & \langle {a_{2}, q_{1}} \rangle & ... & \langle {a_{n}, q_{1}} \rangle \\
    0 & \langle {a_{2}, q_{2}} \rangle & ... & \langle {a_{n}, q_{2}} \rangle \\
    \vdots & \vdots & & \vdots \\  
    0 & 0 & ... & \langle {a_{n}, q_{n}} \rangle \\
\end{bmatrix}
$$

To find an orthonormal basis from a set of vectors, we use the Gram-Schmidt process.

##### Gram-Schmidt Process for finding Orthonormal Basis
In many algorithms, it is useful to orthonormalize a set of vectors. Given that the set of vectors $\lbrace u_{1}, ... ,u_{n} \rbrace$ forms a basis for vector space $V$, a set of vectors $\lbrace v_{1}, ... ,v_{n} \rbrace$ can be constructed such that it can form an orthonormal base for $V$:  


Set $v_{1} = u_{1}$; First basis vector $e_{1} = \frac{v_{1}}{\lVert v_{1} \rVert}$ <br>

$v{2} = u{2} - proj_{v1}u{2} = u_{2} - \frac{\langle u_{2},v_{1}\rangle}{\lVert v_{1} \rVert^{2}}v_{1}$; Second basis vector $e_{2} = \frac{v_{2}}{\lVert v_{2} \rVert}$ <br>

$v{3} = u{3} - proj_{v1}u{3} - proj_{v2}u{3} = u_{3} - \frac{\langle u_{3},v_{1}\rangle}{\lVert v_{1} \rVert^{2}}v_{1} - \frac{\langle u_{3},v_{2}\rangle}{\lVert v_{2} \rVert^{2}}v_{2}$; Third basis vector $e_{3} = \frac{v_{3}}{\lVert v_{3} \rVert}$ <br>

Continue the pattern until $e_{n}$ basis vectors are found


### [Eigendecomposition](https://github.com/B33Boy/Data-Driven-Algorithms-Collection/blob/master/Matrix%20Factorizations/Eigendecomposition.ipynb)
Eigendecomposition of $A = V \lambda V^{-1}$ where $\lambda$ is a diagonal matrix of eigenvalues, and $V$ is an eigenvector matrix.

If V is orthogonal, then we can represent $V$ as $Q$ and the eigendecomposition $A = Q \Lambda Q^{T}$.

### [Singular Value Decomposition](https://github.com/B33Boy/Data-Driven-Algorithms-Collection/blob/master/Matrix%20Factorizations/Singular%20Value%20Decomposition.ipynb)
Singular Value Decomposition (SVD) is $A = U \Sigma V^{\*} $ where U and V* are unitary (i.e. conjugate transpose equals its inverse), and $\Sigma$ is a diagonal matrix of singular values (i.e. eigenvalues square rooted($\Sigma = \sqrt{λ}$))

There are three forms: Full, Reduced, and Truncated

![SVD forms](https://d3i71xaburhd42.cloudfront.net/6265915841382cf7ccaa41880838490a7f91bc1b/3-Figure3-1.png)



My implementations can be found [here](https://github.com/B33Boy/Data-Driven-Algorithms-Collection/tree/master/Matrix%20Factorizations).
