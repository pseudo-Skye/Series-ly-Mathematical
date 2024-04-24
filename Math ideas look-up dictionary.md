# Math Ideas Look-Up Dictionary
I've come across some key math concepts in my machine learning journey, so I made this math dictionary to explain them. **Keepin' it fresh** — this dictionary's always getting the updates, and you may catch these math vibes sprinkled throughout my blog articles. 

## Quick Glance at Essential Concepts
1. [Numerical underflow and overflow](#1-numerical-underflow-and-overflow) 
2. [Portfolio mean and variance](#2-portfolio-mean-and-variance)
3. [Properties of symmetric matrix](#3-properties-of-symmetric-matrix)
4. [Precision matrix](#4-precision-matrix)
5. [Logarithm rules](#5-logarithm-rules)
6. [The evidence lower bound (ELBO)](#6-the-evidence-lower-bound-elbo)
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

## 5. Logarithm rules
### (1) Change of base rules: $`\log_a b = \frac{\ln b}{\ln a}`$

**Proof**:

$$
\begin{split}
b &= a^{\log_a b} = \left(e^{\ln a}\right)^{\log_a b} = e^{\ln a \log_a b} \\
b &= e^{\ln b} = e^{\ln a \log_a b} \\
\ln b &= \ln a \log_a b \\
\log_a b &= \frac{\ln b}{\ln a}
\end{split}
$$

### (2) $`\log_a x \leq x-1,~x>0$ and $a>0,~a\neq 1`$ ? No, only when $`a=e`$.

&emsp; We can firstly define $f(x) = \log_a x-x+1$, we have $f'(x) = \frac{1}{x\ln a}-1$, set $f'(x) = 0$, we have $x = \frac{1}{\ln a}$, then we need to discuss **the impact of $a$ with repsect to the value of $f'(x)$**. 

&emsp; (a) **When $0 < a < 1$**

&emsp; We have $\ln a < 0$ and $x = \frac{1}{\ln a} < 0$. For $x>0 \rightarrow f'(x) < 0 \rightarrow f(x) \downarrow$, the maximum value of $f(x)$ happens when $x \rightarrow 0$, which means $f(x) = \log_a x-x+1 \rightarrow \infty$. 

&emsp; So, when $0< x \leq 1$, $f(x) = \log_a x-x+1 \geq 0$ and $\log_a x \geq x-1$; 

&emsp; and when $x > 1$, $f(x) = \log_a x-x+1 < 0$ and $\log_a x < x-1$

&emsp; Example of $f(x) = \log_{\frac{1}{2}} x-x+1$: 

<p align="center" width="100%">
<img width="30%" src = "https://github.com/pseudo-Skye/Series-ly-Mathematical/assets/117964124/7a0d3226-b9b0-42c1-9b87-bcb2a0915bcd">
</p>


&emsp; (b) **When $a > 1$**

&emsp; We have $\ln a > 0$ and $x = \frac{1}{\ln a} > 0$. So $0 < x < \frac{1}{\ln a} \rightarrow f'(x) > 0 \rightarrow f(x) \uparrow$; $x > \frac{1}{\ln a} \rightarrow f'(x) < 0 \rightarrow f(x) \downarrow$. Thus, the maximum value of $f(x)$ happens when $x = \frac{1}{\ln a}$, which is given by:

$$
f(x)_\text{max} = f(x = \frac{1}{\ln a}) = \log_a \frac{1}{\ln a} - \frac{1}{\ln a} + 1 = \log_a 1 - \log_a \ln a - \frac{1}{\ln a} + 1 = - \log_a \ln a - \frac{1}{\ln a} + 1,~a>1
$$ 

&emsp; Based on the logarithm change of base rules, we have:

$$
f(x)_\text{max} = f(x = \frac{1}{\ln a}) = -\frac{\ln \ln a}{\ln a} - \frac{1}{\ln a} + 1 = -\frac{\ln \ln a + 1}{\ln a} + 1 = -\frac{\ln \left(e\ln a\right)}{\ln a} + 1,~a>1
$$

&emsp; Similarly, we can obatin $g(x) = e\ln x -x$, and follow the same process, we have $g(x)\_\text{max} = g(x=e) = 0$, thus $e\ln x -x \leq 0$ and $e\ln x \leq x$. So for the above equation, we have $\ln \left(e\ln a\right) \leq \ln a$, and thus $\frac{\ln \left(e\ln a\right)}{\ln a} \leq 1$, which means $f(x)_\text{max} \geq 0$. This means, only when $a=e$ we will have $\ln x \leq x-1,~x>0$.

&emsp; Example of $f(x) = \log_{10} x-x+1$: 

<p align="center" width="100%">
<img width="30%" src = "https://github.com/pseudo-Skye/Series-ly-Mathematical/assets/117964124/cf74a9d3-fa89-48d5-bc3c-e68812743ece">
</p>

&emsp; Example of $f(x) = \ln x-x+1$: 

<p align="center" width="100%">
<img width="30%" src = "https://github.com/pseudo-Skye/Series-ly-Mathematical/assets/117964124/6a31a304-c544-4b26-81bf-800ecba78892">
</p>

### (3) Concavity of log function
Based on the [Jensen's Inequality](https://en.wikipedia.org/wiki/Jensen%27s_inequality), the log function is a concave function, where 

$$
\begin{split}
f(\mathbb{E} \[x \]) &\geq \mathbb{E}\[f(x)\] \\
\log (\sum \omega_i x_i) &\geq \sum \omega_i \log x_i
\end{split}
$$

## 6. The evidence lower bound (ELBO)
The Evidence Lower Bound (ELBO) is a general concept used in the context of probabilistic models with hidden latent representations, like **variational autoencoders and diffusion models**. It provides a lower bound on the **log-likelihood** of the observed data under the model. Before talking about ELBO, it is important to understand [information entropy](https://github.com/pseudo-Skye/Series-ly-Mathematical/blob/main/Information%20entropy.md#information-entropy). The **evidence $p(x;\theta)$** refers to **the marginal likelihood of the observed data under the model**. Given the latent representation $h$ and the **approximation** $q(h|x)$ of a true distribution $p(h|x)$, we have:

$$
\log p(x) = \log \left(\sum_h p(x,h) \right) = \log \left(\sum_h q(h|x)\frac{p(x,h)}{q(h|x)} \right)
$$

Based on the [concavity of log function](#3-concavity-of-log-function), we can have:

$$
\log p(x) = \log \left(\sum_h q(h|x)\frac{p(x,h)}{q(h|x)} \right) \geq \sum_h q(h|x) \log \frac{p(x,h)}{q(h|x)} = \sum_h q(h|x)\log p(x,h) - \sum_h q(h|x)\log(h|x) = \mathbb{E}_q \[ \log \frac{p(x,h)}{q(h|x)} \]
$$

The right-hand side of the above is known as **ELBO**: 

$$
\text{ELBO} :=  \sum_h q(h|x) \log \frac{p(x,h)}{q(h|x)} = \mathbb{E}_q \[ \log \frac{p(x,h)}{q(h|x)} \]
$$

Thus, $\log p(x) \geq \text{ELBO}$, and when we **correctly model the latent representation $h$ by the observed $x$** ($`p(h|x) = q(h|x)`$), the ELBO will approaching $\log (x)$. The proof of equivalence is given by: 

$$
\begin{split}
\text{ELBO} &= \sum_h q(h|x)\log p(x,h) - \sum_h q(h|x)\log(h|x) \\
&= \sum_h p(h|x)\log \[p(h|x)p(x)\] - \sum_h p(h|x)\log(h|x) \\
&= \sum_h p(h|x)\log p(h|x) + \sum_h p(h|x)\log p(x) - \sum_h p(h|x)\log p(h|x) \\
&= \sum_h p(h|x)\log p(x) = \log p(x) \sum_h p(h|x) = \log p(x)
\end{split}
$$

Intuitively, the **difference** between $\log(x)$ and ELBO is $\log(x) - \text{ELBO} = D_{KL}(q(h|x) || p(h|x))$, which evaluates **how well we approximate $q(h|x)$ to the true distribution $p(h|x)$**. This difference can also be proved by:

$$
\begin{split}
D_{KL}(q(h|x) || p(h|x)) &= \sum_h q(h|x) \log \frac{q(h|x)}{p(h|x)} = \sum_h q(h|x) \log \frac{q(h|x)p(x)}{p(x,h)} \\
&= \sum_h q(h|x) \[ \log q(h|x) + \log p(x) - \log p(x,h)\] \\
&=  \sum_h q(h|x) \log \frac{q(h|x)}{p(x,h)} +  \sum_h q(h|x) \log p(x) \\
&= -\sum_h q(h|x) \log \frac{p(x,h)}{q(h|x)} + \log p(x) = \log p(x) - \text{ELBO}
\end{split}
$$

- The [ELBO in VAE](https://github.com/pseudo-Skye/Series-ly-Mathematical/blob/main/VAE.md#loss-function-of-vae-evidence-lower-boundelbo) is given by: 

$$
\begin{aligned}
\text{ELBO} &= \mathbb{E}\_q \[ \log \frac{p(x,h)}{q(h|x)} \] \\
&= \mathbb{E}\_q \[ \log \frac{p(x|h) p(h)}{q(h|x)} \] = \sum_h q(h|x) \log p(x|h) - \sum\_h q(h|x) \log \frac{q(h|x)}{p(h)} = \mathbb{E}\_{h \sim q_\phi(h|x)} \log p_\theta(x|h)-D_{\mathrm{KL}}\left(q_\phi(h|x) || p_\theta(h)\right) \\
&= \mathbb{E}\_q \[ \log \frac{p(h|x) p(x)}{q(h|x)} \] = \sum_h q(h|x) \log p(x) - \sum_h q(h|x) \log \frac{q(h|x)}{p(h|x)} = \log p_\theta(x) - D_{\mathrm{KL}}\left(q_\phi(h|x) || p_\theta(h|x)\right) \\
& = -L_{\mathrm{VAE}}(\theta, \phi) 
\end{aligned}
$$

&emsp; where $q_\phi$ is the encoder function and $p_\theta$ is the decoder function. 

- The ELBO in [Diffusion](https://github.com/pseudo-Skye/Time-Matters/blob/main/financial%20trading/Dva%20(CIKM%2023).md#diffusion-in-time-series) is given by:

$$
\begin{aligned}
p(x,h) &:= p_\theta (\mathbf{x}\_{0:T}) = p\left(\mathbf{x}\_T\right) \prod_{t=1}^T p_\theta\left(\mathbf{x}\_{t-1} | \mathbf{x}\_t\right), \quad p\_\theta\left(\mathbf{x}\_{t-1} | \mathbf{x}\_t\right):=\mathcal{N}\left(\mathbf{x}\_{t-1} ; \boldsymbol{\mu}\_\theta\left(\mathbf{x}\_t, t\right), \mathbf{\Sigma}\_\theta\left(\mathbf{x}\_t, t\right)\right) \\
q(h|x) &:= q(\mathbf{x}\_{1:T}|\mathbf{x}\_0) =  \prod_{t=1}^T q\left(\mathbf{x}\_t | \mathbf{x}\_{t-1}\right), \quad q\left(\mathbf{x}\_t \mid \mathbf{x}\_{t-1}\right):=\mathcal{N}\left(\mathbf{x}\_t ; \sqrt{1-\beta_t} \mathbf{x}\_{t-1}, \beta_t \mathbf{I}\right) \\
\text{ELBO} &= \mathbb{E}\_q \[ \log \frac{p(x,h)}{q(h|x)} \] = \mathbb{E}\_q \[ \log \frac{p_\theta (\mathbf{x}\_{0:T})}{q(\mathbf{x}\_{1:T}|\mathbf{x}\_0)} \] = \mathbb{E}\_q \[\log p(\mathbf{x}\_T) + \sum_{t=1}^T \log \frac{p_\theta\left(\mathbf{x}\_{t-1} | \mathbf{x}\_t\right)}{q\left(\mathbf{x}\_t | \mathbf{x}\_{t-1}\right)} \] \\
&= -L_{\mathrm{Diffusion}}(\theta) 
\end{aligned}
$$




