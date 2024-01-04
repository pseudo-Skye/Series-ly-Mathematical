# Decoding Variational Autoencoders: Exploring the Mathematical Foundations
This article introduces the fundamental concepts of variational autoencoder (VAE) from a **probability model** perspective. VAEs use a method called **variational inference** to approximate the posterior distribution of latent variables, making them capable of various data generation and reconstruction tasks. The article will also discuss the **loss function** at the core of VAEs, known as the **Evidence Lower Bound (ELBO)**, which serves as a guide in optimizing the model.

## Limitation of autoencoders (AE)
Imagine you have a standard AE trained to learn about different types of fruits. It's really good at reconstructing the fruits you show it, but when you ask it to generate new fruit, it struggles. Because it tends to create distinct clusters in its latent space and only knows about the specific fruits it's seen during training, it can't create anything in between those familiar fruits.

This is where VAE comes in. VAEs ensure that the space where they encode things is smooth and continuous (a distribution). So, you can easily move between different fruits in that space. If you tell a VAE to generate something between an apple and an orange, it can do that because it understands the in-between space.

## Purpose 
VAEs aim to learn a meaningful latent representation of data that can be used for **(1) reconstruction** tasks (such as denoising or inpainting) and **(2) generating** new data samples.

## Probability model perspective of VAE
We can think of the latent variable $z$ as a hidden cause for the observed data $x$. For example, if we are generating images of faces, $z$ could represent the smile, skin tone, gender, and hair color of the face. 

<p align="center" width="100%">
<img width="50%" src = "https://github.com/pseudo-Skye/StudyNotes/assets/117964124/13bf5c2f-4864-4ff4-a219-f5bae9f0f5c4">
</p>

The probability model perspective assumes that there is a prior distribution $p(z)$ that defines how likely different values of $z$ are, and a likelihood distribution $p(x|z)$ that defines how likely different values of $x$ are given $z$. There are two perspectives that can help us understand why we need VAE in data generation/reconstruction tasks. 

### 1. Find the characteristics $`z`$ given $`x`$
In the real world, we often don't have direct knowledge of the hidden attributes in the latent space. So, when we have a dataset of images (denoted as $x$), we need to infer the characteristics $z$ associated with them. This inference is expressed as $p(z|x)$:

$$
p(z|x) = \frac{p(x|z)p(z)}{p(x)}
$$

However, calculating the denominator, $p(x) = \int p(x|z)p(z)dz$, by summing up all possible values of $z$ is nearly impossible due to the high dimensionality. 

### 2. Generate $`x`$ from the characteristics $`z`$

Suppose we know the distribution parameters $\theta$ of $z$, and we want to generate $x$ based on $z$. We can first sample $z^i$ from the distribution $p_\theta(z)$, and then we generate $x^i$ based on the conditional distribution $p_\theta(x|z = z^i)$. The optimal $\theta$ is the one that maximizes the probability of generating real data samples:

$$
\theta=\arg \max_\theta \prod_{i=1}^n p_\theta\left(\mathbf{x}^{i}\right)
$$

Still, to calculate the likelihood function, we also need to proceed with the calculation of $p(x^i) = \int p(x^i|z)p(z)dz$.

To tackle this challenge, **variational inference** comes into play. It's a method used to approximate $p(z|x)$ and forms the core of VAEs.

## An example of variational inference
1. **Complex Probability Distribution**: You have a probability distribution that's hard to work with directly. This might be a posterior distribution in a Bayesian model (${p(z|x)}$), which can involve many variables and dependencies.
2. **Variational Distribution**: You choose a simpler distribution (${q(z|x)}$) from a family of distributions (e.g., a Gaussian distribution) and parameterize it.
3. **Optimization**: You adjust the parameters of the simpler distribution to make it as close as possible to the complex distribution. 
4. **Approximation**: Once the simpler distribution is optimized, it serves as an approximation to the complex distribution. You can use this simpler distribution for inference, sampling, and making predictions.
In this [post](https://www.jeremyjordan.me/variational-autoencoders/), the **Implementation** section gives an example of using Gaussian distribution to approximate the latent variables.

<p align="center" width="100%">
<img width="50%" src = "https://github.com/pseudo-Skye/StudyNotes/assets/117964124/b9d12a87-6435-404f-a4ea-f394756bd5f5">
</p>

## Loss function of VAE: evidence lower bound(ELBO)
The next step involves determining how to quantify the difference between two distributions, namely $p(z|x)$ and $q(z|x)$. One intuitive way is to measure the **Kullback-Leibler (KL) divergence** between these two distributions. However, the question is **whether to measure the similarity with $D_{\text{KL}}(p(z|x)||q(z|x))$ or $D_{\text{KL}}(q(z|x)||p(z|x))$**.

Before we dive into the choice of loss function used in VAEs, it's essential to become acquainted with the concept of **forward and reverse divergence**. You can learn more about it from this [post](https://agustinus.kristia.de/techblog/2016/12/21/forward-reverse-kl/). The calculation of KL (i.e., forward KL) can be interpreted as a weighted (i.e., ${P(x)}$) sum of difference (i.e., $\frac{P(x)}{Q(x)}$) between the distributions, and it can be viewed as the expectation of the difference. 

$$
DL(P(x)||Q(x)) = \int P(x) log \frac{P(x)}{Q(x)}dx = \mathbb{E}_{x \sim P(x)}log \frac{P(x)}{Q(x)}
$$

In brief, the forward KL divergence is weighted by the true distribution $p(z|x)$ and encourages the approximate distribution $q(z|x)$ to avoid collapsing to zero, ensuring that it spans across all possible $x$ according to $p(z|x)$. On the other hand, the reverse KL divergence is weighted by the approximate distribution $q(z|x)$ and aims to fit a portion of $p(z|x)$ as accurately as possible while ignoring the remaining part. The following figure shows the optimal $Q$ or $q(z|x)$ that minimizes the forward and reverse KL divergence respectively. **In the context of VAE, the reverse KL divergence is commonly employed.**

<p align="center" width="100%">
<img width="60%" src = "https://github.com/pseudo-Skye/StudyNotes/assets/117964124/f9dd5fbc-ef56-4356-b3e8-990b39673dd3">
</p>


Our purpose in VAE is to minimize the KL divergence between distribution $q(z|x)$ and $p(z|x)$, the detailed calculation process can be found in this [post](https://lilianweng.github.io/posts/2018-08-12-vae/). The final result is given by 

$$
D_{\mathrm{KL}}\left(q(z|x)||p(z|x)\right)=\log p(x)+D_{\mathrm{KL}}\left(q(z|x) \| p(z)\right)-\mathbb{E}_{z \sim q(z|x)} \log p(x|z)
$$

This is the same to maximize both sides of the following equation: 

$$
\log p(x)-D_\mathrm{KL}\left(q(z|x)||p(z|x)\right)=\mathbb{E}_{z \sim q(z|x)} - D\_\mathrm{KL}(q(z|x)||p(z))
$$

Given that we can denote the parameters of the prior distribution of $z$ as $p_\theta(z)$, the true posterior distribution of $z$ as $p_\theta(z|x)$, and the parameters in the encoder to approximate $p_\theta(z|x)$ as $q_\phi(z|x)$, we can rewrite the above equation as: 

$$
\log p_\theta(x)-D_{\mathrm{KL}}\left(q_\phi(z|x) || p_\theta(z|x)\right)=\mathbb{E}\_{z \sim q_\phi(z|x)} \log p_\theta(x|z)-D_{\mathrm{KL}}\left(q_\phi(z|x) \| p_\theta(z)\right)
$$

The purpose of the loss function is to **find the optimal $\theta$ and $\phi$ that give us the maximum $p_\theta(x)$ while minimizing the KL divergence between $p_\theta(z|x)$ and $q_\phi(z|x)$**. The loss function can be written as:

$$
\begin{aligned}
L_{\mathrm{VAE}}(\theta, \phi) & =-\log p_\theta(x)+D_{\mathrm{KL}}\left(q_\phi(z|x) || p_\theta(z|x)\right) \\
& =-\mathbb{E}\_{z \sim q_\phi(z|x)} \log p_\theta(x|z)+D_{\mathrm{KL}}\left(q_\phi(z|x) || p_\theta(z)\right)
\end{aligned}
$$

Given $D_{\mathrm{KL}}$ is non-negative, we have

$$
-L_{\mathrm{VAE}}(\theta, \phi) = \log p_\theta(x)-D_{\mathrm{KL}}\left(q_\phi(z|x) || p_\theta(z|x)\right) <= \log p_\theta(x)
$$

This $-L_{\mathrm{VAE}}(\theta, \phi)$ is called the **variational lower bound**, or **evidence lower bound (ELBO)**. We can say the purpose of VAE is to **minimize the loss** $-L_{\mathrm{VAE}}(\theta, \phi)$ or **maximize the ELBO** $-L_{\mathrm{VAE}}(\theta, \phi)$. 

Suppose our assumption to the prior distribution $p(z)$ is **standard Gaussian distribution**, then given the loss function from above, we can also write the loss function as: 

$$
\begin{aligned}
L_{\mathrm{VAE}}(\theta, \phi) &= -\mathbb{E}\_{z \sim q_\phi(z|x)} \log p_\theta(x|z)+D_{\mathrm{KL}}\left(q_\phi(z|x) || p_\theta(z)\right)\\
& = ||x-\hat{x}||^2 +  \mathrm{KL}\[N(\mu_x, \sigma_x), N (0, I)\]
\end{aligned}
$$
