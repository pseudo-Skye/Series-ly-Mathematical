# Information Entropy
The **information** can be quantified by the unit of **bit** (short for binary digit), which is analogous to how we use kilograms (kg) for weight. In the context of information entropy, a bit represents a unit of information based on **binary (two-state) choices**, similar to how digital information is stored as 0s and 1s. 

When you flip a fair coin, you have two possible outcomes: heads or tails, each with a probability of 50%. This reflects the fact that each of the two equally probable outcomes (heads or tails) will contribute 1 bit of information to the overall scenario. The information of each event outcome can be written as:

- $\log_2 2=1$ bit: This corresponds to flipping a fair coin (2 outcomes—H, T), where 1 bit of information is needed to specify each outcome.
- $\log_2 4=2$ bits: This is like flipping a coin twice (4 possible sequences—HH, HT, TH, TT), requiring 2 bits to describe each sequence.

Thus, we can see that information is **the reduction in uncertainty** when a message or an event occurs. The information $I(x)$ provided by an event $x$ with probability $p(x)$ is given by:

$$
I(x) = \log \frac{1}{p(x)} = -\log p(x)
$$

This tells us that if an unlikely event happens, it provides more information than an expected event. Hence, the use of the **base-2 logarithm** $\log_2$ in the following entropy formula is precisely because we are dealing with binary information. 

## 1. Entropy and uncertainty
In information theory, **uncertainty is measured by entropy**. **Information entropy** $H(X)$ is a measure of the **average uncertainty** of the outcomes of a random variable $X$, or we say it **quantifies how much information is needed** to describe the outcome of an event. The formula of entropy calculation is given by:

$$
H(X) = \sum p_i \log \frac{1}{p_i} = -\sum p_i \log {p_i}
$$

where $p_i$ is the possibility of outcome $i$. A system with **high entropy (high uncertainty)** implies that its outcome is difficult to predict, whereas **low entropy (low uncertainty)** indicates more predictability.

## 2. Entropy of event with equally likely outcomes 
In the example of tossing a coin, we have the possibility of getting a tail or head is $p_1 = p_2 = 50%$. In this case, the average uncertainty of the outcomes of a coin flip is given by: 

$$
H(X) = p_1 \log \frac{1}{p_1} + p_2 \log \frac{1}{p_2} = 1
$$

So, the **average uncertainty of flipping a coin** is 1 bit. Similarly, for a event with $N$ **equally** possible outcomes ($p_i = \frac{1}{N}$), the entropy is given by: 

$$
H(X) = p_1 \log \frac{1}{p_1} + p_2 \log \frac{1}{p_2} + ... p_N \log \frac{1}{p_N} = (p_1+p_2+p_3+...p_N) \log N = N \times \frac{1}{N} \log N = \log N
$$

A key property of entropy is that when you have a discrete random variable where each outcome is **equally likely** (**uniform distribution**), the entropy is **maximized**. Intuitively, if you pick samples from a set with equal probabilities, it **increases** the randomness or uncertainty. For example, when you toss a **fair coin**, the randomness is highest because each outcome (head or tail) has an equal probability of 50%. In contrast, if one side of the coin is more likely than the other, the **uncertainty decreases** because you're more likely to get that particular side. 

## 3. Gibbs' inequality
Suppose we have two sets of probabilities $P=\\{p_1, \ldots, p_n\\}$ and $Q=\\{q_1, \ldots, q_n\\}$, representing **discrete** probability distributions. The Gibbs' inequality states that:

$$
-\sum_{i=1}^n p_i \log p_i \leq-\sum_{i=1}^n p_i \log q_i
$$

The left-hand side of the inequality represents the entropy (or uncertainty) of the distribution $P$, while the right-hand side is the **cross entropy** between $P$ and another distribution $Q$. The inequality tells us that the entropy of $P$ (its inherent randomness) is always less than or equal to its cross entropy with $Q$ (**how well $Q$ approximates $P$**).

**Proof**:

KL divergence is a measure used in information theory and statistics to quantify **how much information is lost** when one probability distribution $q$ is used to approximate another distribution $p$. This [post](https://www.countbayesie.com/blog/2017/5/9/kullback-leibler-divergence-explained) gives a very intuitive explanation of KL divergence. Proving the Gibbs' inequality is the same as proving that **the KL divergence is non-negative**:

$$
\begin{split}
&\text{Gibb's inequality:}~~~\sum p_i \log p_i - \sum p_i \log q_i \geq 0 \\
&\text{KL divergence:}~~~\sum p_i \log \frac{p_i}{q_i} \geq 0, \text{or} -\sum p_i \log \frac{p_i}{q_i} \leq 0 
\end{split}
$$

Given the [rules of logarithm](https://github.com/pseudo-Skye/Series-ly-Mathematical/blob/main/Math%20ideas%20look-up%20dictionary.md#5-logarithm-rules), we have $\log_b a = \frac{\ln a}{\ln b}$, and $\ln x \leq x-1$. Therefore,

$$
\begin{split}
-\sum p_i \log_b \frac{p_i}{q_i} = -\sum p_i \frac{\ln \frac{p_i}{q_i}}{\ln b} &= -\frac{1}{\ln b} \sum p_i \ln \frac{p_i}{q_i} = \frac{1}{\ln b} \sum p_i \ln \frac{q_i}{p_i} \\
\frac{1}{\ln b} \sum p_i \ln \frac{q_i}{p_i} \leq \frac{1}{\ln b} \sum p_i \left(\frac{q_i}{p_i}-1\right) &= \frac{1}{\ln b} \sum \left(q_i-p_i\right) = \frac{1}{\ln b} (\sum q_i - \sum p_i) = 0
\end{split}
$$

Thus, we have $-\sum p_i \log \frac{p_i}{q_i} \leq 0$, which is the same as $\sum p_i \log p_i - \sum p_i \log q_i \geq 0$, and **KL divergence can never be negative**. 



