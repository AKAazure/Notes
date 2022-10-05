# AI6103

## AI6103 Probability Theory

- ***Sample space Ω***: The **set** of possible outcomes 𝜔 of a **random experiment**
- ***Event***: A **subset** of the **sample space**.
- ***Probabilistic model***: a **function** 𝑃 that assigns a **probability** to every **event**.
- Three Axioms of Probability
  - **Non-negativity** For any event 𝐴, its probability $P(A) ≥ 0$
  - The probability of the entire sample space $P(Ω) = 1$
  - If events $A_1$ and $A_2$ are disjoint, then $P(A_1 \cup A_2) = P(A_1) + P(A_2)$
  - Usages
    - The probability of the empty set $∅$, $P(∅)$ is 0
    - For events 𝐴1 and 𝐴2, if 𝐴1 ⊆ 𝐴2, P(𝐴1) ≤ P(𝐴2)
    - $P(A^c) = P(\Omega/A) = 1 - P(A)$
    - If $A_1$ and $A_2$ are not disjoint, $P(A_1 ∪ A_2) = P(A_1) + P(A_2) − P(A_1 ∩ A_2)$

### Conditional Probability and Independence
- For any events A and B,  $P(A, B) = P(B)P(A|B) = P(A)P(B|A)$
- If A and B are independent, $P(A|B) = P(A) \rightarrow P(A,B)=P(A)P(B)$
  - one event has no effects on the  probability of the other
- **Conditional Independence**
  - We write the probability associated with a die as a vector $\theta = (\theta_1, … , \theta_6)$ 
  - Notation
    - For events A, B, and C 
      - $P(A,B,C)=P(A|B,C)P(B|C)P(C)$
    - Assuming A and B are independent given C
      - $P(A,B,C)=P(A|C)P(B|C)P(C)$
- ***Directed Probability Graphs***
- ***Bayes’ Theorem***
  - $P(A|B) = \frac{P(B|A)P(A)}{P(B)}$
  - ***marginalization*** P(umberalla) = P(umberalla,rainy) + P(umberalla,sunny)
    - That is, we marginalized the weather variable
- Bayes’ theorem is important in machine learning because we often do the following
  - We design a model which gives us the probability $P(data | model)$
  - We observe some data 
  - We want to find out P(model|data).
- rain and spillage
  - independent events may not be conditionally independent
  - not independent when given the floor is wet
    - they compete with each other when the floor is wet

### Random Variables and Their Distributions
- A ***random variable*** is a **real-valued** function defined on a sample space
- Probabilistic Distribution
  - **Discrete**: random variables can be **integer-valued**
    - $P(X=i)\ge0$ Probability mass function PMF
    - $\sum{P(X=i)}=1$
    - $\mu=\sum{iP(X=i)}$
    - $\sigma^2 = \sum{(i-\mu)^2P(X=i)}$
    - *Bernoulli Distribution*
      - The experiment has two outcomes, like flipping a coin, and is performed  only once
      - $P(X=1)=p\\ P(X=0)=1-p$
      - $\mu=p\\\sigma^2 = p(1-p)$
    - *Binomial Distribution*
      - $P(X=i) = P^{K}_{i}p^{i}(1-p)^{K-i}$
      - $\mu = Kp \\ \sigma^2=Kp(1-p)$ Independent from each other
    - Categorical Distribution
      - The experiment has N>2 possible outcomes (dice, cards)
      - $N-1$ free parameters: 𝜃1 to 𝜃𝑛−1. 𝜃𝑛 is determined once the first N-1 are determined
    - Multinomial Distribution
      - The experiment has N possible outcomes. It is repeated K times.
      - total probability is $\frac{K!}{\prod^{N}_{i}(X_i!)}\prod^{N}{i}\theta_i^{(X_i)}$
  - **Continuous**: random variables can be **real-valued**
    - difficult to deal with because the number of  possibilities are infinite and uncountable.
    - CPF Cumulative probability function $F_X(a) = 𝑃(𝑋 ≤ a)$
    - $F_X$ is non-decreasing
    - $F_X(x) = \int^{x}_{-\infin}f_x(y)dy$
    - $\frac{d}{dx}F_X(x) = f_X(x)$
    - ***Gaussian Distribution***
      - Two parameters: mean $\mu$ and standard deviation $\sigma$
      - $f(x)=\frac{1}{\sigma\sqrt{2\pi}}e^{-\frac{1}{2}(\frac{x-\mu}{\sigma})^2}$
  - **Multivariate or multidimensional**: random variables can be **vectors** or **matrices**.

#### Estimating a probability distribution
- Maximum Likelihood Estimation
  - We have a probability distribution 𝑃(𝑋|𝜃) parameterized by $\theta$
  - We have K observations, $X^1,\dots,X^k$
  - We want to find $\theta$ 
  - the best $\theta$ is 
    - $\hat \theta_{MLE} = \argmax_{\theta}P(X^1,\dots,X^k|\theta)$
- For Gaussian Distribution: The best 𝜇, 𝜎 are the ones that maximize $P(X^1,\dots,X^K|\mu,\sigma)$
  - $\hat \theta_{MLE} = \argmax_{\theta}{P(X^1,\dots,X^k|\mu,\sigma)} \\=\argmax_\theta \prod^{K}_{i}P(X^i|\mu,\sigma) \\=\argmax_\theta\sum^{N}_{i}\log P(X^i|\mu,\sigma)$
  - **Thus** $\hat \mu_{MLE}= \argmax_\mu \sum^{N}_{i}\log P(X^i|\mu,\sigma) \\ = \argmax_\mu$
  - $\hat\mu_{MLE}=\frac{1}{N}\sum X^i$
  - $\hat \sigma_{MLE}=\sqrt{\frac{1}{N}\sum(X_i-\mu)^2}$

#### Estimators
- We randomly sample 100 Singaporeans and get their average height, $\hat\mu^1$
- Repeat this procedure, we get $\hat\mu^2,\hat\mu^3,\dots$
- We will examine the mean and variance of this distribution, called the  **sampling distribution.**

#### Bias and Variance
- ***Bias***: the difference between this **estimator's expected value** and the **true value of the parameter** being estimated.
  - $E(\hat \theta)-\theta$
- ***Variance***: The scatter of the estimator around its expected value
  - $E[(\hat \theta^{i} - E(\hat \theta))^2]$
- **Bias-variance trade-off**: Often we can decrease bias at the cost of variance and vice versa. 
- Is $\hat \mu_{MLE}$ Biased?
  - $\hat \mu_{MLE} is unbiased$
  - $\hat \sigma_{MLE} is biased$
    - unbiased estimator $\hat \sigma_{UB} = \sqrt{\frac{1}{N-1}\sum(X^i-\mu)^2}$