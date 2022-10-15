# A6103

## A6103 Optimization and Regularization 

### Backpropagation & Autograd
- Backpropagation
  - In modern neural networks, the actual computational graph can be very complex
  - But we can use backpropagation as long as
    - the chain rule applies
    - the graph is a **directed acyclic graph**
- Auto-differentiation
  - Autograd performs exact matrix multiplication
    - Autograd is not finite-difference (imprecise), nor Symbolic differentiation (no use of symbolic representations). 
- The Vanishing Gradient Problem
  - Tanh and sigmoid both have derivatives less than 1, leading to difficulties in training **deep networks**
- Exploding Gradient
  - For non-singular $A$, $\sigma_{min}||x||\le||Ax||\le\sigma_{max}||x||$
  - If many matrices have $\sigma_{min}>1$, their product will grow very **large**.

### Practical Solutions
- Vanishing Gradient
  - In modern deep learning
    - Different **activation functions**, which have gradients = 1.
    - Optimization methods like **Adam**, which adaptively scales gradients
    - **Skip connections**, which create different paths for gradients to flow.
    - In RNN (covered later), **long short-term memory**
- Exploding Gradient
  - **Gradient Clipping**: set a maximum value for the gradient norm.
  - Vanishing gradient is usually a more difficult problem.
- Stochastic Gradient Descent
  - $\hat g_{mini_batch} = \frac{1}{|B|}\sum{\frac{dl(x^i,y^i,w)}{dw}} = g + \epsilon$
  - The mini-batch estimate of the gradient contains some random noise.
    - Bias is bad. We need to **shuffle** datasets to get unbiased estimates
    - Variance may not be too bad
      - As long as the angle between the estimated gradient $\hat g$ and true gradient 𝑔 is less than 90 degrees, we are still roughly in the right direction
- Batch Size and Learning Rate
  - We increase the batch size together with the learning rate 
    - factor is $\frac{\eta}{|\Beta|}$
  - The two things usually are proportional (i.e., linear relationship)
- Learning Rate Schedule
  - Exponential
    - $\eta_t = \alpha^t \eta_0, 0<\alpha<1$
    - Decrease by the factor 𝛼 at predefined iteration / epoch
    - Total number of epochs = $T$
    - Multiply 𝜂 by 1/10 at $\frac{T}{2}$, and another 1/10 at $\frac{3T}{4}$
  - Cosine Annealing
    - $\eta_t = \eta_{min} + \frac{1}{2}(\eta_{max} -\eta_{min}(1+\cos{\frac{t}{T}\pi}))$
    - A half cycle of the cosine function
    - Slow decrease at the beginning  and at the end, but fast drop in the middle.
  - Plateau
    - Decrease the learning rate by 𝛼 when the training loss does not decrease by at least 𝛽 in 𝑆 epochs
    - May create difficulties for reproduction.

### Regularization
- Underfitting vs. Overfitting
  - Underfitting: The model is too limited and cannot represent the function that generates the data.
    - **not learning enough**
  - Overfitting: The model is too expressive. It can represent the data-generating function as well as the noise in the data
    - **learning too much**
- Optimization vs. Regularization
  - Optimization: fitting the data
    - Minimize the empirical loss on the training set
    - Maximize performance on the training set
- Regularization: avoid overfitting the data
  - Improve performance on future data (test set), often at the cost of training loss.
- Traditionally, these two are considered to be opposite to each other.
  - In deep learning, not always
- DL deals with more complex semantics than traditional ML
- Deep Learning is **Representation Learning**
  - Shallow models work with existing features
    - logistic regression
  - Deep models can be seen as **learning features**
  - Learning the **right features** is critical
- Many regularization techniques in DL aim to improve the **quality** of features extracted by the deep network.
  - MixUp
  - Data Augmentation
  - Pretraining is effective against overfitting
- Optimization vs. Regularization in DL
  -  Optimization is hard in DL
     -  First-order optimization is hard.
     -  Second-order methods are great but we can’t use them.
  - Regularization is hard in DL
    - Overparameterized deep nets are capable of **memorizing** every training instancE
- Weight Decay  
  - For a loss function ℒ(𝒘), the GD update is $w_t = w_{t-1} - \eta\Delta_{w_{t-1}}L(w)$
    - We can add L2 regularization, just like we did in Ridge Regression
  - Linear vs. Ridge Regression
  - Intuitively, L2 regularization and weight decay favors model parameters 𝒘 that have small norm 𝒘 . 
  - Limited model complexity may lead to better generalization 
- Data Augmentation