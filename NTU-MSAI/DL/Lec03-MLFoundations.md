# AI6103

## AI6103 Deep Learning for Data Science

### Information Theory

- The scientific study of the quantification, storage, and communication of digital information
- ***Entropy***
  - The degree of **uncertainty** or “chaos / surprise / information” in a **random variable**
  - Expectation of negative log probability $H(P(X))=E[\log{X}]=-\int{P(X)\log{P(X)}dx}$
    - for discrete variables $H(P(X))=-\sum{P(X=x_i)\log{P(X=x_i)}}$
    - 最终应该是正数
    - More uncertain larger entropy
- ***Cross-Entropy***
  - For two probability distributions $P$ and $Q$, the cross-entropy is
    - $H(Q,P) = E_Q[-\log{P(X)}] = -\sum{Q(X=x_i)\log{P(X=x_i)}}$
    - Interpretation: We designed an encoding scheme under the assumption that **the probability distribution is $P$**. 
      - However, the **actual distribution** is $Q$. 
      - What is the number of bits we need to encode the information?
- ***Kullback–Leibler divergence (relative entropy)***
  - A measure for the differences between distributions
    - $KL(P||Q) = E_P[\log{\frac{P(X)}{Q(X)}}]=\sum{P(X=x_i)\log{\frac{P(X=x_i)}{Q(X=x_i)}}}$
- ***KL divergence and cross entropy***
  - $H(Q,P) = H(Q) + KL(Q||P)$
- KL divergence is asymmetric
  - P is large when Q is small -> large divergence
  - Q is large when P is small -> small divergence.
- Jensen–Shannon divergence
  - A symmetric measure for the differences between distributions
    - $JS(Q,P) = \frac{1}{2}KL(Q,P) + \frac{1}{2}KL(P,Q)$


### Machine Learning Basics
- There is a function $y=f(x)$ that we want to approximate
  - $x$ is the input to the model
  - $y$ is what the model tries to predict
- ***Classification vs. Regression***
  - Classification: the output $y$ is **discrete** and represents **distinct categories**
  - Regression: the output $y$ represents a **continuously varying quantity**. Typically, a real number
- General Guidelines for Feature Selection
  - Use features that are strongly correlated with the target variable, but they don’t have to be causal
  - Avoid features that you don’t want to model to consider, such as the year and the winery in wine quality regression
  - Beware of information leaks, privacy, ethical concerns
- Linear Regression
  - a p-dimensional feature vector $x = (x_1,x_2,\dots,x_p)$ and a scalar output $y$
  - Our model is linear $\hat y = \beta_1x_1+\beta_2x_2+\dots+\beta_px_p+\beta_0$
  - In vector form $\hat y = \beta^Tx+\beta_0$
  - How do we find the parameters $\beta$ and $\beta_0$?
  - The Loss Function
    - MSE: the average squared distance between $y^i$ and $\hat y^i$
    - We only observe limited amount of data, so it is usually taken as **minimizing error on training data** (empirical error)
  - ***A Closed-form Solution***
    - In matrix form, the loss function is $MSE = \frac{1}{n}(X\beta-y)^T(X\beta-y)$
    - To find the minimum, we find the derivative against $\beta$
      - $MSE' = \frac{2}{n}(X^TX\beta - X^Ty)$
    - Setting the above to zero and simplify, we get $\beta^* = (X^TX)^{-1}X^Ty$
    - **Second-order Conditions**
      - $MSE'' = \frac{2}{n} X^TX$
      - It is positive semi-definite because, for an arbitrary vector $z\neq 0$
        - $z^TX^TXz = (Xz)^TXz \ge 0$
        - So this is truly a **local minimum**
        - Since the loss function is **convex** (which we will not show), the local minimum is also the **global minimum**.
    - $\beta^* = (X^TX)^{-1}X^Ty$
      - Is $X^TX$ invertible?
        - when data point num $n$ > feature num $p$, and at least $p$ data points are linearly independent, then $X^TX$ is invertible
        - but when $p>n$, we need **regularization**,
          - can be solved by **ridge regression**.
- ***Ridge Regression***
  - The ordinary least squares (OLS) estimator: $\hat \beta_{OLS} = (X^TX)^{-1}X^Ty$
  - If $n<p$, $X^TX$ is not invertible
  - We can use ridge regression $\hat \beta_{RR}=(X^T+\lambda I)^{-1}X^Ty$
    - $\lambda$ is very small positive nmber
    - the ridge regression estimator for 𝛽 is **biased** but has **lower variance** than the OLS estimator [bias-variance tradeoff]
- Ridge Regression Is **L2-regularized** Linear Regression
  - Ridge Regression can be understood as optimizing a different loss function.
    - $L = \frac{1}{n}(X\beta-y)^T(X\beta-y) + \frac{1}{n}\lambda||\beta||^2$ 
  - We again take the derivative against 𝛽 and set it to zero
    - $L' = \frac{2}{n}(X^TX\beta - X^Ty + \lambda \beta) = 0$
    - $\hat \beta_{RR} = (X^TX+\lambda I)^{-1}X^Ty$

### Non-Linear Models
- Logistic Regression: a Single “Neuron”
  - Assumption: Every data point follows the same model
    - $\hat y^i = \sigma(\beta^Tx^i)$
  - Activation function
    - $\sigma(z) = \frac{1}{1+e^{-z}} = \frac{e^z}{1+e^z}$
  - ***The Cross-entropy Loss***
    - The label $y^i$ either 0 or 1
    - The cross-entropy loss
      - $L = -\frac{1}{n}\sum{y^i\log{\hat y^i}+(1-y^i)\log{(1-\hat y^i)}}$
        - $\hat y^i$ is the estimated prob of $y^i = 1$
          - 1.0 -> definitely 1
    - $H(P,Q) = -E_{P(X)}[\log{Q(X)}] = H(P)+KL(P||Q)$
    - Minimizing the loss minimizes the distance between the GT distribution $P(y^i|x^i)$ and estimated distribution $Q(\hat y^i | x^i,\beta)$

- MLE Perspective
  - MLE for bionomial distribution
    - The binomial RV takes 1 with probability $\theta$ and 0 with probability $1 - \theta$
    - Assuming we observe RV=1 for $n$ times and RV=0 for $m$ times
      - prob is $P^{n+m}_{m}\theta^{n}(1-\theta)^m$
      - log it, we have $\theta^* = \argmax_\theta{n\log{\theta}+m\log{1-\theta}}$
      - diff it, we have $d\theta = \frac{n}{\theta} - \frac{m}{1-\theta} = 0 \rightarrow \theta = \frac{n}{m+n}$
    - 换句话说，基于观察到n成功，m失败这一事实，猜测$\theta = \frac{n}{m+n}$是MLE的，这一点也符合直觉
  - Logistic Regression
    - Find the parameters $\beta$ that minimize the cross-entropy loss function

### Optimization 

- The Gradient Descent Algorithm
  - input
    - loss function $L(\beta)$
    - inital position $\beta_0$
  - learning rate $\eta$ is a small constant
  - Repeat for a predefined amount of time (or until convergence)
    - Move in the direction of negative gradient
    - $\beta_t = \beta_{t-1} - \eta\frac{dL(\beta)}{d\beta}|_{\beta_{t-1}}$
  - Too small a learning rate 𝜂: **slow convergence**
  - Too large a learning rate 𝜂: **oscillation**, **overshooting**
  - The learning rate 𝜂 determines how  much we move at each step
- A single neuron is still **VERY limited**
  -  infamously fails to represent the **XOR** function