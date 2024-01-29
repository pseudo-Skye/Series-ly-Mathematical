# Math Ideas Look-Up Dictionary
I've come across some key math concepts in my machine learning journey, so I made this math dictionary to explain them. **Keepin' it fresh** — this dictionary's always getting the updates, and you may catch these math vibes sprinkled throughout my blog articles. 

## Quick Glance at Essential Concepts
1. [Numerical underflow and overflow](#1-numerical-underflow-and-overflow) 
2. [Portfolio mean and variance](#2-portfolio-mean-and-variance)
3. [Properties of symmetric matrix](#3-properties-of-symmetric-matrix)
4. [Precision matrix](#4-precision-matrix)

## 1. Numerical underflow and overflow
Numerical underflow and overflow are issues that arise when working with extremely small or large numbers in a computer's finite precision representation.

In likelihood or probability distribution calculation, the log density of a probability distribution provides a more numerically stable way to represent the likelihood of observing a particular data point. In this case, taking the logarithm helps avoid **numerical underflow**, especially when dealing with products of probabilities. Numerical underflow arises when working with extremely small numbers in a computer's finite precision representation. For example, if you were to directly compute $10^{-50}$ without taking the logarithm, it would be a very small number, potentially close to zero. In a computer's finite precision, this could lead to underflow, where the number becomes too small to be represented accurately.

```ruby
# Direct computation without logarithm
small_prob = 10**(-50)
print(small_prob)
# Output: 0.0 (underflow)

# Logarithmic representation
log_small_prob = math.log10(10**(-50))
print(log_small_prob)
# Output: -50.0
```

Similarly, there is another case called **numerical overflow**. If you were to directly compute $10^{50}$ without taking the logarithm would yield a very large number. In a computer, this could lead to overflow, where the number becomes too large to be accurately represented. 

```ruby
# Direct computation without logarithm
large_prob = 10**(50)
print(large_prob)
# Output: OverflowError: (34, 'Result too large')

# Logarithmic representation
log_large_prob = math.log10(10**(50))
print(log_large_prob)
```

## 2. Portfolio mean and variance
Suppose we have two assets, Asset 1 (A) and Asset 2 (B). The return ratio of each asset across $n$ days can be denoted as: $A = \\{x_1, x_2, ... , x_n\\}$ and $B = \\{y_1, y_2, ... , y_n\\}$. The **average** return of each asset is denoted as $\bar{x}$ and $\bar{y}$, the **variance** of each asset is marked as $\sigma_a$ and $\sigma_b$, and the **covariance** between the two assets is $\sigma_{ab}$. Say that we split our investment into two parts, where $w_a$ is the ratio we invest in asset A and $w_b$ is the ratio we invest in asset B, and $w_a + w_b = 1$. 

In this case, we can firstly obtain the **mean** $\bar{r}$ of our total return ratio  as:

$$
\bar{r} = E(r) = \frac{1}{n}\sum r_i = \frac{1}{n}\sum (w_a x_i + w_b y_i) = \frac{1}{n}\sum (w_a x_i)+ \frac{1}{n}\sum (w_b y_i) = w_a\bar{x}+w_b\bar{y}
$$

To calculate the **variance** $\sigma_r$ of our return ratio , we have:

$$
\sigma_r = E\[(r-\bar r)^2\] = \frac{1}{n}\sum (w_a x_i + w_b y_i - w_a\bar{x}+w_b\bar{y})^2 = \frac{1}{n}\sum \[w_a^2(x_i-\bar{x})^2\] + \frac{1}{n}\sum \[w_b^2(y_i-\bar{y})^2\] +  \frac{1}{n}\sum \[2w_a w_b(x_i-\bar{x})(y_i-\bar{y})\] = w_a^2\sigma_a + w_b^2\sigma_b + 2w_a w_b \sigma_{ab}
$$

**This quadratic form of equation can be written as a matrix**: 

$$
w^{\prime} \Sigma w=\left\[\begin{array}{ll}
w_a & w_b
\end{array}\right]\left\[\begin{array}{ll}
\sigma_{a} & \sigma_{ab} \\
\sigma_{ab} & \sigma_{b}
\end{array}\right\]\left\[\begin{array}{l}
w_a \\
w_b
\end{array}\right\]
$$

Thus, to expand this formula to $s$ assets, we can have the **portfolio variance** $\text{Var}(r)$ as:

$$
\text{Var}(r)=w^{\prime} \Sigma w=\left\[\begin{array}{llll}
w_1 & w_2 & \ldots & w_s
\end{array}\right\]\left\[\begin{array}{cccc}
\sigma_{1,1} & \sigma_{1,2} & \ldots & \sigma_{1, s} \\
\sigma_{2,1} & \sigma_{2,2} & \ldots & \sigma_{2, s} \\
\vdots & \vdots & \ddots & \vdots \\
\sigma_{n, 1} & \sigma_{n, 2} & \ldots & \sigma_{s, s}
\end{array}\right]\left\[\begin{array}{c}
w_1 \\
w_2 \\
\vdots \\
w_s
\end{array}\right\]
$$

## 3. Properties of symmetric matrix

### (1) Eigenvectors corresponding to distinct eigenvalues are orthogonal
Given a square matrix $A$, for any vector $\vec{x}$ and $\vec{y}$, we have:

$$
(A\vec{x})\cdot\vec{y}= (A\vec{x})^T\vec{y} = (\vec{x})^T A^T \vec{y} = (\vec{x})^T (A^T \vec{y}) = \vec{x} \cdot(A^T\vec{y})
$$

Now, if we have the matrix $A$ to be symmetric where $A = A^T$, and **suppose it has two different eigenvalues** as $\mu$ and $\lambda$, and the corresponding eigenvectors as $\vec{x}$ and $\vec{y}$, we have:

$$
\mu(\vec{x} \cdot \vec{y}) = (A\vec{x})\cdot\vec{y} = \vec{x} \cdot(A^T\vec{y}) = \vec{x} \cdot(A\vec{y}) = \vec{x} \cdot(\lambda\vec{y}) = \lambda(\vec{x} \cdot \vec{y})
$$

As $\mu \neq \lambda$, we have $\vec{x} \cdot \vec{y} = 0$. This proof can be extended to any other pairwise eigenvectors of matrix $A$ if it has more than 2 eigenvectors. Thus, **eigenvectors of a symmetric matrix with distinct eigenvalues are orthogonal**.

### (2) Symmetric matrix always has eigenvalues
To prove this property, we need to prove that $\text{det}(A-\lambda I) = 0$ always has at least one solution for the eigenvalue $\lambda$. First, you need to acquaint yourself with the concept of [Laplace expansion](https://www.statlect.com/matrix-algebra/Laplace-expansion-minors-cofactors-adjoints#:~:text=The%20Laplace%20expansion%20is%20a,its%20signed%20minors%2C%20called%20cofactors.). If we define $B = A-\lambda I$ as a $K \times K$ matrix (with $K \geq 2$ ), denote by $C_{i j} = (-1)^{i+j}|M_{ij}|$ the cofactor of an entry $B_{i j}$, then, for any row $i$, the following row expansion holds:

$$
\text{det}(B)=\sum_{j=1}^K B_{i j} C_{i j}
$$

Similarly, for any column $i$, the following column expansion holds:

$$
\text{det}(B)=\sum_{i=1}^K B_{i j} C_{i j}
$$

It is said that based on the cofactor(Laplace) expansion (*the proof is relatively hard so I will skip, you can give a try...*), we can let $f(\lambda)=\text{det}\left(A-\lambda I\right)$ be its characteristic polynomial. Then $f(\lambda)$ is a polynomial of degree $n$. Moreover, $f(\lambda)$ has the form

$$
f(\lambda)=\text{det}\left(A-\lambda I_n\right) =(-1)^n \lambda^n+(-1)^{n-1} \text{Tr}(A) \lambda^{n-1}+\cdots+\text{det}(A) 
$$

where $\text{Tr}(A)$ is the trace of the square matrix $A$, and the number $\text{Tr}(A)$ is obtained by **summing the diagonal entries** of $A$. By the fundamental theorem of algebra, for a polynomial equation of degree $n$, $f(\lambda) = 0$ possesses precisely $n$ roots. Thus, we can rewrite the above equation as:

$$
f(\lambda)=(-1)^n\left(\lambda-\lambda_1\right)\left(\lambda-\lambda_2\right) \cdots\left(\lambda-\lambda_n\right)
$$

### (3) Eigendecomposition of real symmetric matrices (spectral decomposition)
As we have proved that real symmetric matrix always has eigenvalues and eigenvectors, we can have:

$$
\begin{split}
AV &= VL \\
AV &= \begin{bmatrix}\vec{v_1} & \vec{v_2} & ... &\vec{v_n} \end{bmatrix} \begin{bmatrix}\lambda_1 & 0 & ... & 0 \\\ 0 & \lambda_2 & ... & 0 \\\ 0&0&...&\lambda_n  \end{bmatrix} \\
A&= VLV^{-1}
\end{split}
$$

## 4. Precision matrix
The precision matrix or concentration matrix is the matrix **inverse** of the covariance matrix, which is denoted as $P = \Sigma^{-1}$. For univariate distributions, the precision is defined as the reciprocal of the variance, which is denoted as $p = \frac{1}{\sigma^2}$. 

The key property of the precision matrix is that its zeros tell you about conditional independence. $P_{ij} = 0$ if and only if $X_i$ and $X_j$ are **conditionally independent** given all other coordinates of $X$. Here is an example from a research paper showing how a precision matrix corresponds to a graph. 

<p align="center" width="100%">
<img width="60%" src = "https://github.com/pseudo-Skye/Series-ly-Mathematical/assets/117964124/b591d82e-c74e-461f-b5cd-13c491623aec">
</p>









