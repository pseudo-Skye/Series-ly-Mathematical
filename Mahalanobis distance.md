# Mahalanobis distance in finance
In finance, the financial turbulence index could potentially be a measure that captures the degree of uncertainty, fluctuations, or disruptions in financial markets. The measure of the financial turbulence index is based on the **Mahalanobis distance**. To grasp the concept of Mahalanobis distance, it's crucial to first delve into the geometric significance of linear transformations, eigenvalues, and eigenvectors. You can find a comprehensive [tutorial](https://www.3blue1brown.com/topics/linear-algebra) that investigates these fundamental topics. Next, we must familiarize ourselves with the notion of a [covariance matrix](https://www.cuemath.com/algebra/covariance-matrix/) which describes the correlation between variables. 

## Linear transformation and covariance: the Cholesky factor
Imagine we have a dataset that represents a circle within a 2D space. Within this context, we have the capacity to manipulate this circle by applying scaling and rotation transformations. These transformations can be elegantly understood through the lens of eigenvalues and eigenvectors.

![image](https://github.com/pseudo-Skye/StudyNotes/assets/117964124/81a0a69b-1979-4828-b700-56627a323848)

The combined scaling and rotation operation can be mathematically represented as $T = RS$, where $S$ represents the scaling function, and $R$ embodies the rotation function. If we introduce parameters like the rotation angle $\theta$, as well as the scaling factors $s_x$ and $s_y$, we can express $S$ and $R$ as:

$$
S = \begin{bmatrix}s_x & 0 \\\ 0 & s_y \end{bmatrix}, R=\begin{bmatrix} \cos (\theta) & -\sin (\theta) \\\ \sin (\theta) & \cos (\theta)\end{bmatrix}
$$

Given the dataset $D$, after the transformation $T = RS$, the dataset becomes $D^\prime = TD$, where the original dataset has covariance $\Sigma$, and the dataset $D^\prime$ has covariance $\Sigma^\prime$. We can then write the representation of the covariance $\Sigma^\prime$ by its eigenvectors and eigenvalues as:

$$
\begin{split}
\Sigma^\prime V &= VL \\
&= \begin{bmatrix}\vec{v_1} & \vec{v_2} \end{bmatrix} \begin{bmatrix}l_1 & 0 \\\ 0 & l_2 \end{bmatrix}
\end{split}
$$

where $\[\vec{v_1} \\ \vec{v_2}\]$ describes the direction of rotation $R$, and matrix $L$ describes the scaling matrix $S$ as it scales the eigenvectors along its direction. It is important to note that $R$ is an orthogonal matrix, and $S$ is a diagonal matrix. Thus, we have $R^{-1} = R^T$ and $S = S^T$. The property of $S$ is self-evident, and the **property of the orthogonal $R$ can be proved by**:

$$
R^T R = \begin{bmatrix}\vec{v_1} & \vec{v_2} \end{bmatrix}^T \begin{bmatrix}\vec{v_1} & \vec{v_2} \end{bmatrix}  = \begin{bmatrix}\vec{v_1}^T \\\ \vec{v_2}^T \end{bmatrix} \begin{bmatrix}\vec{v_1} & \vec{v_2} \end{bmatrix} = \begin{bmatrix}\vec{v_1}^T\vec{v_1} & \vec{v_1}^T \vec{v_2} \\\ \vec{v_2}^T \vec{v_1} & \vec{v_2}^T \vec{v_2}\end{bmatrix} = \begin{bmatrix}1 & 0 \\\ 0 & 1\end{bmatrix} = I
$$

$$
(R^T R) R^{-1} = R^T (RR^{-1}) = I R^{-1} = R^T I = R^T = R^{-1}
$$

The covariance matrix $\Sigma^\prime$ can be represented as:

$$
\Sigma^\prime = VLV^{-1} = V \sqrt{L} \sqrt{L} V^{-1} = RSSR^{-1} = RSS^TR^T = RS(SR)^T = TT^T
$$

The key insight is that the transformation $RS$ applied to the original dataset $D$ and the representation $RS(SR)^T$ of the covariance matrix are equivalent. This is because **the covariance matrix measures how data points vary with respect to each other, and this variation is captured by the combined transformation $RS$**. 

It is important to know that the transformation matrix $T$ here is named as the **Cholesky factor**. It is usually written as $\Sigma^\prime = TT^T$. Geometrically, the Cholesky matrix transforms uncorrelated variables into variables whose variances and covariances are given by $\Sigma^\prime$. You can go the other way by taking the inversed matrix $T^{-1}$ to transform the correlated variables into uncorrelated ones. Thus, to decorate the variables and standardize the distribution, we can apply $D = T^{-1}(D^\prime- \mu^\prime)$ to the correlated dataset. 

## Euclidean distance and linear transformation
Consider a circle centered at the origin with a radius of 1. The Euclidean distance (ED) of every data point on the circle to the origin is 1, which can be expressed as 

$$
X^TX = (IX)^TIX = X^TI^TIX = 1
$$

where $X^T = \[x_1, x_2\]$. Now, for a circle with its center at $\mu^T = \[\mu_1, \mu_2\]$, the ED is represented as

$$
(X-\mu)^T(X-\mu) = (X-\mu)^TI^TI(X-\mu)
$$

If we apply a specific transformation matrix $L$ to the original space of the circle, which involves scaling its radius and rotation, the ED of a data point in the new space is given by: 

$$
(X-\mu)^TL^TL(X-\mu)
$$

This transformation causes the original circle to become an ellipse, the same as shown in the above figure. 

Thus, consider a tilted ellipse after transformation which represents the correlated dataset $Y$, we can decorrelate and standardize it by applying the Cholesky factor as $X = L^{-1}(Y-\mu)$, the ED of $X$ then can be calculated as:

$$
X^TX = [L^{-1}(Y-\mu)]^T L^{-1}(Y-\mu) = (Y-\mu)^T \left(L^{-1}\right)^T L^{-1}(Y-\mu) \tag{1}
$$

Here, we will use several matrix identities such as $(AB)^T = B^TA^T$, $(AB)^{-1} = B^{-1}A^{-1}$, and $\left(A^T\right)^{-1} = \left(A^{-1}\right)^T$. The relationship between inverse and transpose can be proved by:

$$
\left(L^{-1}\right)^T = \left(L^{-1}\right)^T \left(L^T \left(L^T\right)^{-1}\right) = \left(\left(L^{-1}\right)^T L^T\right) \left(L^T\right)^{-1} = \left(LL^{-1}\right)^T \left(L^T\right)^{-1} = I\left(L^T\right)^{-1} = \left(L^T\right)^{-1}
$$

Thus, we have $\left(L^{-1}\right)^T L^{-1} = \left(L^T\right)^{-1} L^{-1} = \left(LL^T\right)^{-1}$ continue the above calculation of Eq.(1) by:

$$
X^TX = (Y-\mu)^T\left(LL^T\right)^{-1}(Y-\mu) = (Y-\mu)^T \Sigma_Y^{-1}(Y-\mu)
$$

This is equation $X^TX = (Y-\mu)^T \Sigma_Y^{-1}(Y-\mu)$ is called the **squared Mahalanobis distance**. The Mahalanobis distance is a way to measure distances between data points, but it's unique because it considers both the variance (how data spreads out) and the relationships (correlations) between different features.

To simplify it, imagine you want to measure distances between data points. Mahalanobis distance first transforms the data to make it easier to compare by removing any correlations and making all the variables have the same scale. Then, it calculates distances between these transformed data points just like you would with regular distances (like the Euclidean distance).

## Financial turbulence index

$$
\text{turbulence}_t=\left(y_t-\mu\right)^{\prime} \Sigma^{-1}\left(y_t-\mu\right) \in \mathbb{R}
$$

where $y_t \in \mathbb{R}^n$ denotes the stock returns for the current time period $t$, $\mu \in \mathbb{R}^n$ denotes the average  of history returns, and $\Sigma \in \mathbb{R}^{n \times n}$ denotes the covariance of historical returns. 
