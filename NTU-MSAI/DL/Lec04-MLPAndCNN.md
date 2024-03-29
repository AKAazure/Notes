# A6103

## A6103 Multi-layer Perceptron & Convolutional Neural Network

### Multi-layer Perceptron
- Every intermediate nodes have a set of $\beta$ weights and offsets params
  - EXAMPLE
    - $\sigma$ Activation function, $\beta$ neuron parameters
    - Layer 1, neuron 1 $z_{1,1} = \sigma(\beta^T_{1,1}x)$
    - Layer 1, neuron 2 $z_{1,2} = \sigma(\beta^T_{1,2}x)$
    - Put in matrix form
      - $z_1 = (z_{1,1}, z_{1,2})^T = \sigma(W_1x)$
    - Create L layers
      - $f(x) = \sigma(W_L \dots \sigma(W_2\sigma(W_1x)))$
      -  These matrices can be of arbitrary size, as long as the **multiplication**  works out
         -  𝑊1 is 10 by 𝑝, 𝑊2 is 100 by 10, 𝑊3 is 341 by 100, etc
            -  p is feature num
        - $W_L$ is a vector, which is 1 by K
      -  The output $f(x)$ is a **scalar**, denoting $P(y=1|x)$
 - Activation Function: ***Hyperbolic Tangent*** Tanh
   - $\tanh{(x)}=\frac{e^x-e^{-x}}{e^x+e^{-x}} = \frac{e^{2x}-1}{e^{2x}+1}$
   -  Like **sigmoid**, squashes (−∞, +∞) to a finite range
   -  Unlike sigmoid, it has a larger **range** (−1, 1)
- Multi-class Classification
  - $z_L = \sigma(W_L \dots \sigma(W_2\sigma(W_1x)))$
  - We now have 𝑀 classes
  - $W_L$ is a vector, which is 𝑀 by K
  - We put $z_L$ through a softmax function
    -  $\theta=softmax(z_L)=[\frac{e^{z_{L,1}}}{\sum{e^{z_{L,j}}}},\frac{e^{z_{L,2}}}{\sum{e^{z_{L,j}}}},\dots,\frac{e^{z_{L,M}}}{\sum{e^{z_{L,j}}}}]$
    -  The logits $z_{L,i}$ are unbounded. They could be negative or greater than 1
    -  Softmax ensures that the output is a valid probability distribution
       -  $\theta_i>0 ,\sum{\theta_{i}}=1$
 - Multi-class Cross-entropy
   - Multi-class Cross-entropy
     - $L = -\frac{1}{N}\sum^{N}_{i=1}y_i^T\log{f_w(x_i)}$
     - $f_w(x_i)$ is a vector of **probabilities**, such as the output of the **softmax operation**
     - $y_i$ is a **one-hot vector** [0, … 1, …, 0] with the GT class set to 1.

### Convolutional Networks
- Convolutional Neural Network
  - MLPs have many parameters
  - Convolutional networks reduce the total number of parameters by
    - **Local connection**
      - Every pixel in the next level depends on a small area in the input
    - **Weight sharing**
      - The filter weights are shared by all locations

#### 2D Convolution
- Use zero padding around the input to maintain the size of the feature maps
- Input size : $B × H × W × C$ (order 4 tensor)
  -  $B$: batch size 
     -  The number of data points in one forward operation
  - $H$ height of the image / feature mao
  - $W$ width of the image / feature map
  - $C$ number of channels
- Number of Parameters
  - Kernel / filter size: $K \times K$
    - $K$ is usually an odd number so that there is a central pixel.
  - Input Channels: $C_{in}$
  - Output Channels $C_{out}$
  - Total number of parameters = $K \times K \times C_{in} \times C_{out}$
  - Sizes of Feature Maps w/o Strides or Dilation
    - input size  $n\times n$
    - filter sizer $k \times k$
    - output size $(n-k+1)\times(n-k+1)$
  - **Padding**
  - **Stride**
    - Stride=1 -> Filter Moves 1 pixel at one time
    - Stride=2 -> Filter Moves 2 pixel at one time
  - **Dilation**
  - Altogether for torch
    - Input $(N,C_{in},H_{in},W_{in})$
    - Output $N,C_{out},H_{out},W_{out}$
      - $H_{out} = \lfloor \frac{H_{in}+2\times padding[0] - dilation[0]\times(kernel[0]-1)-1}{stride[0]} +1  \rfloor$
      - $W_{out} = \lfloor \frac{W_{in}+2\times padding[1] - dilation[1]\times(kernel[1]-1)-1}{stride[1]} +1  \rfloor$
  - $k\times k$ pixels in the next layer see $(2k-1)\times(2k-1)$
    - $n-k+1 = k \rightarrow n = 2k-1$
- Receptive Field and Resolution
  - High resolution image & Small perceptive field -> Cannot recognize image
  - Low resolution image & Small perceptive field -> Suitable for image recognition
  - High resolution image & Large perceptive field -> High cost
  - In neural networks for image recognition, a common practice is to **gradually lower the resolution** of the feature maps
- Downsampling
  - **Max-pooling**
  - **Mean-pooling**
  - Convolution with Stride > 1

### more on activation functions
- The ReLU Activation Function
  - $f(z) = \max{(0,z)}$
  - Benefits:
    - Non-linear
    - Creates many zero elements (i.e., sparse activations)
    - Good gradients when z > 0
- Leaky / Parametric ReLU
  - ReLU has zero gradient when 𝑧 < 0
  - Once that happens, z will never receive any update
  - $f(z) = \max(0,x) + a\min{(0,x)}$
    - $0<a<1$
- GeLU 
  - Intuitively, GeLU provides a “second chance” before one heads into zero-gradient territory. 

### Normalization
- Input Normalization / Whitening
  - We usually normalize the input images by channel
  - So that each channel is expected to have 0 mean and 1 unit standard deviation
  - Without balancing
    - Consider the simple model $y = \beta_1x_1 + \beta_2x_2$
    - if $\beta_1$ and $\beta_2$ are of similar magnitudes and $x_1<<x_2$, $\beta_2x_2$ will dominate
    - so we need normalization
  - Procedure:
    - compute $\mu_r,\mu_g,\mu_b$ and stdev $\sigma_r,\sigma_g,\sigma_b$
    - for input $X=[x_r,x_g,x_b]$ use $X' = [\frac{x_r-\mu_r}{\sigma_r},\frac{x_g-\mu_g}{\sigma_g},\frac{x_b-\mu_b}{\sigma_b}]$
- Batch Normalization
  - Given: batch-wide channel mean $\mu_c$ and standard deviation $\sigma_c$.
  - For feautre map $X=[x_1,x_2,\dots,x_c]$ compute per channel statistics $\mu_i,\sigma_i$
    - $X_i' = \gamma_i \frac{x_i-\mu_i}{\sigma_i+\epsilon}+\beta_i$
      - $\epsilon$ is small to avoid zero division
      - $\gamma_i,\beta_i$  are learnable scalar parameters
        - one pair/channel
  - Replacing whole-dataset statistics with the mini-batch statistics requires the **batch size** to be **reasonable large**
    - because the variance of mean estimator $\hat \mu_{MLE} = \frac{Var(X)}{N}$
      - 𝑁 is the number of elements
      - When $N=H \times W \times B$ is too small, the mean estimator is unreliable
  -  **Dataset-wise** statistics are good but too **expensive**
  -  Training utilizes the batch-wise mean and standard deviation
  -  During inference (i.e., testing), we **keep a running mean and a running variance.**
     -  model.eval() to do inference 
        -  `model.eval()` does NOT turn off autograd
           -  use `torch.no_grad()` `torch.inference_mode()`
- Other Normalization Forms
  - Instance Normalization
    - normalize one sample by itself
  - Group Normalization
  - Layer Normalization

### Skip Connection
- Benefits of skip connection
  - Creates another path for gradient backpropagation
  - Allows the network to easily learn identity mappings
- In the BN+ReLU setup, about 50% of outputs are zero
- Building Blocks So Far
  - Convolution layers
  - MLP layers
  - Max Mean pool.ing
  - ReLu acitvation
  - Residual connection 
  - Batch Norm
- ResNet
  - 5 groups of convolution layers, each group operating at one resolution
  - The first convolution is the “stem”. There is only one in  this group