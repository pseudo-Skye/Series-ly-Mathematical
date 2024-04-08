# Dynamic Time Warping
Dynamic Time Warping (DTW) is like a detective tool for finding similarities between two sequences, especially w**hen they have similar patterns but are a bit off in timing**. With DTW, we can **stretch**, **compress**, and **shift** one time series to make it match up with the rhythm of the other. This helps us discover hidden patterns and similarities that other methods might not catch, like measuring the distance between points (Euclidean distance).

I think starting from the [algorithm](https://www.youtube.com/watch?v=9GdbMc4CEhE) is a great way to learn DTW. Once you understand how it works in practice, diving into the mathematical equations will make more sense. This article will only discuss the mathematical equations related to DTW, which are often not explained clearly in other online resources. 

Formally, the DTW optimization problem writes:

$$
\text{DTW}\_q\left(x, x^{\prime}\right)=\min\_{\pi \in \mathcal{A}\left(x, x^{\prime}\right)}\left(\sum_{(i, j) \in \pi} d\left(x_i, x_j^{\prime}\right)^q\right)^{\frac{1}{q}}
$$

- **Time series $x$ and $x^{\prime}$**: two time series $x$ and $x^{\prime}$ of respective lengths $n$ and $m$. The element of each time series can be represented as $x_i$ and $x^{\prime}_j$. 
- **$q$-norm distance**: Similar to the L1 (Manhattan) distance for $q=1$ and the L2 (Euclidean) distance for $q=2$. So, you can think of it as controlling the type of distance metric used in the calculation of similarity between elements of the time series.
- **Alignment path $\pi$**: $\pi = \[\pi_0, \pi_1, \pi_2, ..., \pi_{K-1} \]$ is a sequence of $K$ index pairs, where $\pi_k = (i,j)$. $(i,j)$ means the $i$-th element of $x$ and the $j$-th element of $x^{\prime}$. There are three conditions it should satisfy:
    - $\pi_0=(0,0)$
    - $\pi_{K-1}=(n-1, m-1)$
    - $i_{k-1} \leq i_k \leq i_{k-1}+1$: At step $k$, the index $i_k$ of the first time series can either stay the same or increase by at most one from the previous step's index $i_{k-1}$. In other words, we can only **move forward or stay at the same position** for the first time series, allowing for a maximum increase of one index at each step.
    - $j_{k-1} \leq j_k \leq j_{k-1}+1$: Similarly, it applies the same constraint to the index $j_k$ of the second time series at step $k$. It ensures that we can only **move forward or stay at the same position** for the second time series, with a maximum increase of one index at each step.
 
Let's visualize this concept with examples:

<p align="center" width="100%">
<img width="60%" src = "https://github.com/pseudo-Skye/StudyNotes/assets/117964124/b7f22060-6f92-4f30-bcae-af157d6d4aab">
</p>

In the figure above, we see three alignment paths starting from (0,0). We need to ensure that each step moves either forward or stays in the same position, with a maximum movement of one step forward. Among these three alignment paths, **path 3** provides the minimum sum of distances along the path. Therefore, it is chosen as the solution for the DTW algorithm.
