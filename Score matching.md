# Decoding the Logic of Score Matching: An Energy-Based Solution
Score matching techniques and energy-based functions have puzzled me for quite some time. In this blog, I aim to simplify and share my insights with you. We'll explore the fundamentals of score matching and its interaction with energy-based functions in a straightforward manner. Additionally, I'll discuss the close relationship between score matching and denoising autoencoder, adding a layer of practicality to our exploration. **At the conclusion of this blog, I've crafted a visual pipeline illustrating how score matching interacts with denoising autoencoders, providing you with a clear roadmap to grasp the concept.**

**Score-matching (SM)**([original paper](https://jmlr.org/papers/volume6/hyvarinen05a/hyvarinen05a.pdf)) takes advantage of the **energy function**, which is the basis of **energy based model (EBM)**. EBM is a form of the generative model imported directly from statistical physics to learning, and the energy function is imported from the [Boltzmann distribution (Gibbs distribution)](https://en.wikipedia.org/wiki/Boltzmann_distribution).  

Assume we observe a random vector $\mathbf{x} \in \mathbb{R}^n$, which is assigned an energy $E(\mathbf{x})$, the probability of finding the system in a particular state $p(\mathbf{x}; {\theta})$ based on its energy is given by Boltzmann distribution:

$$
p(\mathbf{x} ;  {\theta})=\frac{1}{Z(\theta)} \exp (-E(\mathbf{x} ;  {\theta}))
$$

Note that $p(\mathbf{x} ;  {\theta})$ represents a **scalar value** that describes the probability of the system being in the state described by the random vector $\mathbf{x}$ according to the Boltzmann distribution. Also, $E(\mathbf{x};{\theta})$ could be a **scalar energy value** resulting from a linear combination or a nonlinear transformation of the components of $\mathbf{x}$. After the parameters ${\theta}$ of the probability density are learned, we can sample from this distribution to generate new samples of $\mathbf{x}$. The **partition function** $Z({\theta})=\int_{\mathbf{x}\in \mathbb{R}^n} \exp (-E(\mathbf{x} ;  {\theta})) d\mathbf{x}$ is a normalization factor that ensures the probabilities calculated by the Boltzmann distribution add up to 1. The **score** is the gradient of the log density ([why use log density](https://github.com/pseudo-Skye/Series-ly-Mathematical/blob/main/Math%20ideas%20look-up%20dictionary.md#1-numerical-underflow-and-overflow)) with respect to the data vector $\mathbf{x}$: 

$$
\psi(\mathbf{x} ;  {\theta})=\frac{\partial \log p(\mathbf{x} ;  {\theta})}{\partial \mathbf{x}}=\left(\begin{array}{c}
\frac{\partial \log p(\mathbf{x} ;  {\theta})}{\partial x_1} \\
\vdots \\
\frac{\partial \log p(\mathbf{x} ;  {\theta})}{\partial x_n}
\end{array}\right)=\left(\begin{array}{c}
\psi_1(\mathbf{x} ;  {\theta}) \\
\vdots \\
\psi_n(\mathbf{x} ;  {\theta})
\end{array}\right)=\nabla_{\mathbf{x}} \log p(\mathbf{x} ;  {\theta})
$$

## Explicit score matching
The purpose of SM is to learn ${\theta}$ so that $\psi(\mathbf{x} ;  {\theta})$ best matches the score of the true distribution $q(x)$ as $\frac{\partial \log q(\mathbf{x})}{\partial \mathbf{x}}$. The objective function to be minimized is written as:

$$
J_{ESM_q}(\theta)=\mathbb{E}_{q(\mathbf{x})}\left\[\frac{1}{2}\left\Vert\psi(\mathbf{x} ; \theta)-\frac{\partial \log q(\mathbf{x})}{\partial \mathbf{x}}\right\Vert^2\right\]
$$

This objective function is known as **explicit score matching (ESM)**. In ESM, the key assumption is that the score function can be computed explicitly for the given model. However, we do not have the explicit regression targets $\frac{\partial \log q(\mathbf{x})}{\partial \mathbf{x}}$. If $q(x)$ is unknown, we might face difficulties in directly applying ESM. However, this problem can be addressed by **implicit score matching (ISM)** as follows.

## Implicit score matching
Given 

$$
\begin{aligned}
J_{ESM_q}(\theta)&=\mathbb{E}_{q(\mathbf{x})}\left\[\frac{1}{2}\left\Vert\psi(\mathbf{x} ; \theta)-\frac{\partial \log q(\mathbf{x})}{\partial \mathbf{x}}\right\Vert^2\right\]\\
& = \int q(\mathbf{x})\left\[\frac{1}{2}\left\Vert \psi(\mathbf{x} ; \theta)\right\Vert ^2 + \frac{1}{2}\left\Vert \frac{\partial \log q(\mathbf{x})}{\partial \mathbf{x}} \right\Vert^2 - \left( \frac{\partial \log q(\mathbf{x})}{\partial \mathbf{x}}\right)^T \psi(\mathbf{x} ; \theta) \right\]d\mathbf{x}
\end{aligned}
$$


To minimize $J_{ESM_q}(\theta)$, as the second term here does not depend on $\theta$, the purpose becomes to minimize:

$$
\begin{aligned}
\int q(\mathbf{x})\left\[\frac{1}{2}\left\Vert \psi(\mathbf{x} ; \theta)\right\Vert ^2 - \left( \frac{\partial \log q(\mathbf{x})}{\partial \mathbf{x}}\right)^T \psi(\mathbf{x} ; \theta) \right\]d\mathbf{x}&= \frac{1}{2}\int q(\mathbf{x})\left\Vert \psi(\mathbf{x} ; \theta)\right\Vert ^2 d\mathbf{x} - \int q(\mathbf{x})\left( \frac{\partial \log q(\mathbf{x})}{\partial \mathbf{x}}\right)^T \psi(\mathbf{x} ; \theta) d\mathbf{x}\\
&= \frac{1}{2}\int q(\mathbf{x})\left\Vert \psi(\mathbf{x} ; \theta)\right\Vert ^2 d\mathbf{x} - \Sigma_i \int q(\mathbf{x}) \frac{\partial \log q(\mathbf{x})}{\partial x_i} \psi_i(\mathbf{x} ; \theta) d\mathbf{x}\\
&= \frac{1}{2}\int q(\mathbf{x})\left\Vert \psi(\mathbf{x} ; \theta)\right\Vert ^2 d\mathbf{x} - \Sigma_i \int q(\mathbf{x}) \frac{1}{q(\mathbf{x})} \frac{\partial q(\mathbf{x})}{\partial x_i} \psi_i(\mathbf{x} ; \theta) d\mathbf{x}\\
&= \frac{1}{2}\int q(\mathbf{x})\left\Vert \psi(\mathbf{x} ; \theta)\right\Vert ^2 d\mathbf{x} - \Sigma_i \int \frac{\partial q(\mathbf{x})}{\partial x_i}\psi_i(\mathbf{x} ; \theta) d\mathbf{x}
\end{aligned} 
$$

Now we only consider one component of the summation: 

$$
\int \frac{\partial q(\mathbf{x})}{\partial x_1} \psi_1(\mathbf{x} ; \theta) d\mathbf{x} = \int \left\[ \int \frac{\partial q(\mathbf{x})}{\partial x_1} \psi_1(\mathbf{x} ; \theta) dx_1 \right\] dx_2dx_3...dx_n
$$

We have:

$$
\begin{aligned}
\int \frac{\partial q(\mathbf{x})}{\partial x_1}\psi_1(\mathbf{x} ; \theta) dx_1 &= \int \left\[ \psi_1(\mathbf{x} ; \theta) \frac{\partial q(\mathbf{x})}{\partial x_1} \right\] dx_1 + \int \left\[q(\mathbf{x}) \frac{\partial \psi_1(\mathbf{x} ; \theta)}{\partial x_1} \right\] dx_1 - \int \left\[ q(\mathbf{x}) \frac{ \partial \psi_1(\mathbf{x} ; \theta)}{\partial x_1} \right\] dx_1\\
&= \int \left\[ \psi_1(\mathbf{x} ; \theta) \frac{\partial q(\mathbf{x})}{\partial x_1}  + q(\mathbf{x}) \frac{\partial \psi_1(\mathbf{x} ; \theta)}{\partial x_1} \right\] dx_1 - \int \left\[ q(\mathbf{x}) \frac{ \partial \psi_1(\mathbf{x} ; \theta)}{\partial x_1} \right\] dx_1 \\
&= \int \left\[ \frac{\partial q(\mathbf{x}) \psi_1(\mathbf{x} ; \theta) }{\partial x_1} \right\] dx_1 - \int \left\[ q(\mathbf{x}) \frac{ \partial \psi_1(\mathbf{x} ; \theta)}{\partial x_1} \right\] dx_1 \\
&= \left\[  q(\mathbf{x}) \psi_1(\mathbf{x} ; \theta) \right\]_{x_1 = -\infty}^{x_1 = \infty} - \int \left\[ q(\mathbf{x}) \frac{ \partial \psi_1(\mathbf{x} ; \theta)}{\partial x_1} \right\] dx_1
\end{aligned} 
$$

As we assume that $q(\mathbf{x}) \psi(\mathbf{x} ; \theta)$ goes to zero at infinity, we have 

$$
\begin{aligned}
\int \frac{\partial q(\mathbf{x})}{\partial x_1}\psi_1(\mathbf{x} ; \theta) dx_1 &= - \int \left\[ q(\mathbf{x}) \frac{ \partial \psi_1(\mathbf{x} ; \theta)}{\partial x_1} \right\] dx_1\\
-\int \frac{\partial q(\mathbf{x})}{\partial x_1} \psi_1(\mathbf{x} ; \theta) d\mathbf{x} &= \int \left\[\int \left\[ q(\mathbf{x}) \frac{ \partial \psi_1(\mathbf{x} ; \theta)}{\partial x_1} \right\] dx_1 \right\] dx_2dx_3...dx_n \\
-\int \frac{\partial q(\mathbf{x})}{\partial x_1} \psi_1(\mathbf{x} ; \theta) d\mathbf{x} &= \int q(\mathbf{x}) \frac{ \partial \psi_1(\mathbf{x} ; \theta)}{\partial x_1} d\mathbf{x}
\end{aligned}
$$

So, for any element of $\mathbf{x}$, we have: 

$$
\begin{aligned}
-\int \frac{\partial q(\mathbf{x})}{\partial x_i} \psi_i(\mathbf{x} ; \theta) d\mathbf{x} &= \int q(\mathbf{x}) \frac{ \partial \psi_i(\mathbf{x} ; \theta)}{\partial x_i} d\mathbf{x} \\
\Sigma_i \int \frac{\partial q(\mathbf{x})}{\partial x_i}\psi_i(\mathbf{x} ; \theta) d\mathbf{x} &= -\Sigma_i \int q(\mathbf{x}) \frac{ \partial \psi_i(\mathbf{x} ; \theta)}{\partial x_i} d\mathbf{x}
\end{aligned}
$$

Hence, we can write the **ISM** as: 

$$
J_{ISM_q}(\theta)= \int q(\mathbf{x})\left\[\frac{1}{2}\left\Vert \psi(\mathbf{x} ; \theta)\right\Vert ^2 - \left( \frac{\partial \log q(\mathbf{x})}{\partial \mathbf{x}}\right)^T \psi(\mathbf{x} ; \theta) \right\]d\mathbf{x} = \frac{1}{2}\int q(\mathbf{x})\left\Vert \psi(\mathbf{x} ; \theta)\right\Vert ^2 d\mathbf{x} + \Sigma_i \int q(\mathbf{x}) \frac{ \partial \psi_i(\mathbf{x} ; \theta)}{\partial x_i} d\mathbf{x} 
$$

## Denoising score matching
The **denoising score matching (DSM)** ([original paper](https://www.iro.umontreal.ca/~vincentp/Publications/DenoisingScoreMatching_NeuralComp2011.pdf)) process is derived from the [denoising autoencoders (DAE)](https://lilianweng.github.io/posts/2018-08-12-vae/) and score matching. 

### The DAE process
1. In DAE, a training input $\mathbf{x}$ is first corrupted by additive gaussian noise of covariance $\sigma^2 \mathbf{I}$, yielding corrupted input $\tilde{\mathbf{x}}=\mathbf{x}+\epsilon, \epsilon \sim \mathcal{N}\left(0, \sigma^2 \mathbf{I}\right)$. This corresponds to conditional density $q_\sigma(\tilde{\mathbf{x}} \mid \mathbf{x})=\frac{1}{(2 \pi)^{d / 2} \sigma^d} e^{-\frac{1}{2 \sigma^2}\|\tilde{\mathbf{x}}-\mathbf{x}\|^2}$. 
2. The corrupted version $\tilde{\mathbf{x}}$  is encoded into a hidden representation as $\mathbf{h} = \text{encode}(\tilde{\mathbf{x}}) = \text{Sigmoid}(\mathbf{W}\tilde{\mathbf{x}}+\mathbf{b})$.
3. The hidden representation $h$ is decoded into the reconstruction as $\hat{\mathbf{x}} = \text{decode}(\mathbf{h}) = \text{Sigmoid}(\mathbf{W}^T \mathbf{h}+\mathbf{c})$.
4. The parameters $\theta = \{\mathbf{W}, \mathbf{b}, \mathbf{c}\}$ are optimized to minimize the expected squared reconstruction error. The objective function of DAE is:

$$
\begin{aligned}
J_{D A E \sigma}(\theta) & =\mathbb{E}\_{q_\sigma(\tilde{\mathbf{x}}, \mathbf{x})}\left\[\|\text{decode}(\text{encode}(\tilde{\mathbf{x}}))-\mathbf{x}\|^2\right\] \\
& =\mathbb{E}\_{q_\sigma(\tilde{\mathbf{x}}, \mathbf{x})}\left\[\left\|\mathbf{W}^T \text{sigmoid}(\mathbf{W} \tilde{\mathbf{x}}+\mathbf{b})+\mathbf{c}-\mathbf{x}\right\|^2\right\]
\end{aligned}
$$

### The connection between DSM and DAE
In the context of DSM, the goal is to estimate the gradient $\psi$ of the log density at some corrupted point $\tilde{\mathbf{x}}$ in a way that guides us toward the clean sample $x$. Given $q_{\sigma}(\tilde{\mathbf{x}}, \mathbf{x}) = q_{\sigma}(\tilde{\mathbf{x}} \mid \mathbf{x}) q(\mathbf{x}) $, we can write the DSM objective function as:

$$
J_{DSM_{q_{\sigma}}}(\theta)=\mathbb{E}\_{q_{\sigma}(\mathbf{x}, \tilde{\mathbf{x}})}\left\[\frac{1}{2}\left\Vert\psi(\tilde{\mathbf{x}}; \theta)-\frac{\partial \log q_{\sigma}(\tilde{\mathbf{x}} \mid \mathbf{x})}{\partial \tilde{\mathbf{x}}}\right\Vert^2\right\]
$$

Note that this objective function, inspired by denoising autoencoders, **is equivalent to ESM**. Given the Gaussian kernel, we have the following: 

$$
\frac{\partial \log q_{\sigma}(\tilde{\mathbf{x}} \mid \mathbf{x})}{\partial \tilde{\mathbf{x}}} = \frac{1}{\sigma^2} (\mathbf{x}-\tilde{\mathbf{x}})
$$

If we see it from the perspective of vectors $\mathbf{x}$ and $\tilde{\mathbf{x}}$, the term $\mathbf{x}-\tilde{\mathbf{x}}$ represents a direction pointing from the corrupted point $\tilde{\mathbf{x}}$ to $\mathbf{x}$, and $\frac{1}{\sigma^2}$ is a scaling factor determined by the chosen Gaussian kernel. The intuition here is that, in denoising, we want the estimated gradient $\psi(\tilde{\mathbf{x}}; \theta)$ to capture the same moving direction and scale. 

Also, in the paper, based on the **chosen energy function** we have (you can read Section 4.3 of the paper):

$$
\psi(\tilde{\mathbf{x}} ; \theta)=\frac{1}{\sigma^2}\left(\mathbf{W}^T \text{sigmoid}(\mathbf{W} \tilde{\mathbf{x}}+\mathbf{b})+\mathbf{c}-\tilde{\mathbf{x}}\right)
$$

Finally, to calculate **the objective function by using DSM**, we have:

$$
\begin{aligned}
J_{DSM_{q_{\sigma}}}(\theta) &=\mathbb{E}\_{q_{\sigma}(\mathbf{x}, \tilde{\mathbf{x}})}\left\[\frac{1}{2}\left\Vert\psi(\tilde{\mathbf{x}}; \theta)-\frac{\partial \log q_{\sigma}(\tilde{\mathbf{x}} \mid \mathbf{x})}{\partial \tilde{\mathbf{x}}}\right\Vert^2\right\] \\
&=\mathbb{E}\_{q_{\sigma}(\mathbf{x}, \tilde{\mathbf{x}})}\left\[\frac{1}{2}\left\Vert\psi(\tilde{\mathbf{x}}; \theta)-\frac{1}{\sigma^2} (\mathbf{x}-\tilde{\mathbf{x}})\right\Vert^2\right\] \\
&=\mathbb{E}\_{q_{\sigma}(\mathbf{x}, \tilde{\mathbf{x}})}\left\[\frac{1}{2}\left\Vert\frac{1}{\sigma^2}\left(\mathbf{W}^T \text{sigmoid}(\mathbf{W} \tilde{\mathbf{x}}+\mathbf{b})+\mathbf{c}-\tilde{\mathbf{x}}\right)-\frac{1}{\sigma^2} (\mathbf{x}-\tilde{\mathbf{x}})\right\Vert^2\right\]\\
&=\mathbb{E}\_{q_{\sigma}(\mathbf{x}, \tilde{\mathbf{x}})}\frac{1}{2\sigma^4} \left\Vert \mathbf{W}^T \text{sigmoid}(\mathbf{W} \tilde{\mathbf{x}}+\mathbf{b})+\mathbf{c}-\mathbf{x}\right\Vert^2\\
&=\mathbb{E}\_{q_{\sigma}(\mathbf{x}, \tilde{\mathbf{x}})}\frac{1}{2\sigma^4}J_{D A E \sigma}(\theta)
\end{aligned}
$$

## An overview of denoising score matching
Both DSM and DAE aim to enhance the robustness of a model by training it to reconstruct clean data from corrupted or noisy input. At the same time, DSM and DAE share a close connection between each other. **When a Gaussian kernel is chosen**,  it suggests that training DSM can be achieved by training a DAE with an associated scalar factor $\frac{1}{2\sigma^4}$. This approach allows you to leverage the principles of denoising and reconstruction errors from DAE while accommodating the DSM objective. 

<p align="center" width="100%">
<img width="70%" src = "https://github.com/pseudo-Skye/StudyNotes/assets/117964124/059a6f35-7849-48e9-8a47-177af518b7d6">
</p>
