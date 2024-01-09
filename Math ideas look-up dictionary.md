# Math Ideas Look-Up Dictionary
I've come across some key math concepts in my machine learning journey, so I made this math dictionary to explain them. **Keepin' it fresh** — this dictionary's always getting the updates, and you may catch these math vibes sprinkled throughout my blog articles. 

## Quick Glance at Essential Concepts
1. [Numerical underflow and overflow](#1-numerical-underflow-and-overflow) 


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
