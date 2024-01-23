# Math Ideas Look-Up Dictionary
I've come across some key math concepts in my machine learning journey, so I made this math dictionary to explain them. **Keepin' it fresh** — this dictionary's always getting the updates, and you may catch these math vibes sprinkled throughout my blog articles. 

## Quick Glance at Essential Concepts
1. [Numerical underflow and overflow](#1-numerical-underflow-and-overflow) 
2. [Portfolio mean and variance](#2-portfolio-mean-and-variance)

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
