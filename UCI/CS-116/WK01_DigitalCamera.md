# Computer Vision: Algorithms and Applications

## 2. Image formation

### 2.3 The digital camera

#### 2.3.2 Color
- When the incoming light hits the imaging sensor, light from different parts of the spectrum is somehow integrated into the discrete red, green, and blue (RGB) color values that we see in
a digital image
  - P87
- RGB is *additive* primary colors
- the existence of **three primaries** is a result of the tri-stimulus nature of the human visual system, since we have three different kinds of cells called *cones*, each of which responds selectively to a different portion of the color spectrum 
- it is preferable to use many more wavelengths for machine applications
  - 红外线 near-infrared (NIR) range

##### CIE RGB and XYZ

- CIE's RGB
  - In the 1930s, the **Commission Internationale d’Eclairage (CIE)** standardized the **RGB** representation using the primary colors of
    -  red (700.0nm wavelength),
    -  green (546.1nm),
    -  and blue (435.8nm).
  - Problems:
    - (440,550) nm for certain pure spectra in the blue–green range, a *negative* amount of red light has to be added
    - (400,440) nm a certain amount of red has to be added to the color being matched to get a color match
- CIE's XYZ
  - Because of the problem associated with *mixing negative light*, the CIE also developed a new color space called XYZ, which contains all of the pure spectral colors within its positive octant
    - It also maps the Y axis to the ***luminance***
      -  perceived relative brightness,
    -  and maps *pure white* to a diagonal (equal-valued) vector.
- The transformation from RGB to XYZ is
given by $$\begin{bmatrix} X\\ Y \\ Z \end{bmatrix}=\frac{1}{0.176967}\begin{bmatrix} 0.49 & 0.31& 0.20 \\ 0.17697 & 0.81240 & 0.01063 \\ 0.00 & 0.01 & 0.99 \end{bmatrix} \begin{bmatrix} R\\ G \\ B \end{bmatrix}$$
  - has the matrix normalized so that the Y value corresponding to *pure red* is 1
  - a more commonly used form is to omit the leading fraction
    - so that the second row adds up to one, i.e., the RGB triplet (1, 1, 1) maps to a Y value of 1
  - If we divide the XYZ values by the sum of X+Y+Z, we obtain the chromaticity coordinate $$x=\frac{X}{X+Y+Z},y=\frac{Y}{X+Y+Z},z=\frac{Z}{X+Y+Z}$$  which sum to 1
    - discard the absolute intensity of a given color
  sample and just represent its pure color
  - A convenient representation for color values is therefore Yxy (luminance plus the two most distinctive chrominance components)
    - when we want to tease apart luminanceand chromaticity
  - it does not actually predict ***how well humans perceive*** differences in color or luminance

#### L\*a\*b\*
- Because the response of the human visual system is ***roughly logarithmic*** (we can perceive relative luminance differences of about 1%), the CIE defined a non-linear re-mapping of theXYZ space called L\*a\*b\*
  - f(t) $$f(t) = 
  \begin{cases}
   t^{1/3} &\text{if } t>\delta^3 \\
   t/(3\delta^2)+2\delta/3 &\text{else, } 
  \end{cases}$$
    - is a finite-slope approximation to the cube root with $\delta = 6/29$
  - The L* component of lightness is $$L^*=116f(\frac{Y}{Y_n})$$
    - Yn is the luminance value for *nominal white* (Fairchild 2013)
  - In a similar fashion, the a* and b* components are defined as $$a^* = 500[f(\frac{X}{X_n})-f(\frac{Y}{Y_n})] ,\text{and}, b^* = 500[f(\frac{Y}{Y_n})-f(\frac{Z}{Z_n})]$$
    - $(X_n,Y_n,Z_n)$ is the *measured white point*