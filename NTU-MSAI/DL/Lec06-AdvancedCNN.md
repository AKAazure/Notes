# A6103

## A6103 Advanced CNN

- Wide ResNet
  - deeper is not always better

- EfficientNet
  - Three major factors affecting performance and the amount of computation: 
    - Depth, 
      - Richer, more complex, higher-level features
    - width, 
      - Increasing width: **Wide ResNet**, easier to train.
    - and the resolution of feature maps.
      - capturing finer details.
- Joint Tuning of Three Factors
  - depth: $d = \alpha^\phi$
  - width $w = \beta^\phi$
  - resolution $r = \gamma^\phi$
- DenseNet
  - CNN with densely connected layers
    -  In the same stage, every convolution block outputs 𝑘 channels.
    -  The input 𝑥0 has 𝑘0 channels
    -  The input to the second block has 𝑘0 + 𝑘 channels, resulting from the concatenation of 𝑥0 and the first conv’s output
  - Transition Block
    - Convert the output to a new spatial resolution and channel number.
- Fractal Net
- MobileNet
  - The networks, while performing well, have too many parameters and require too much compute for deployment on mobile and embedded devices
  - How do we reduce the number of parameters and FLOPs without sacrificing performance too much?
  - MobileNet uses pointwise convolution (regular convolution with 1 × 1 kernels) to allow interaction among channels.
  - **Regular Convolution**
    - $K\times K \times C_{int} \times C_{out}$
    - FLOPS $K\times K \times C_{int} \times C_{out} \times M^2$
  - MobileNet (Depthwise+Poinwise)
    - $K\times k\times C_{in} + C_{in}\times C_{out}$
    - FLOPS $(K\times k\times C_{in} + C_{in}\times C_{out})\times M^2$