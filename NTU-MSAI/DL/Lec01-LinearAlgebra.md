# AI6103

## AI6103 Linear Algebra

### 1. Vectors

#### 运算

1. 加法
   1. a+b
2. 减法
   1. a-b
3. 乘法
   1. 标量乘法
      1. $\beta$ scalar $\alpha$ n-vector
      2. $\beta \alpha = (\beta\alpha_1,\dots,\beta\alpha_n)$
         1. also denoted as $\alpha\beta$
   2. inner product
      1. n-vector $\alpha, \beta$
         1. $\alpha^T\beta = \alpha_1\beta_1 + \alpha_2\beta_2 + \cdots+\alpha_n\beta_n$
         2. notation $<a,b>, a\cdot b,(a,b),<a|b>$
         3. Properties
            1. commutative $a^Tb=b^Ta$
            2. scalar multiplication $(\gamma a)^Tb=\gamma(a^Tb)$
            3. distributive $(a+b)^Tc = a^Tc+b^Tc$
            4. Example $(a+b)^T(c+d)=a^Tc + a^Td+b^Tc +b^Td$
            5. Uses:
               1. $e_i^Ta=a_i$ 获取第i个元素
               2. $1^Ta = a_1 + \cdots + a_n$ 求和
               3. $a^Ta = a_1^2 + \cdots + a_n^2$ 求平方和
      2. Linear Regression
         1. regression model $\hat y = x^T\beta+v$
            1. $x$ is a feature vector
               1. $x_i$ also called **regressors**
            2. n-vector $\beta$ is the **weight vector**
            3. scalar $v$ is the **offset**
            4. scalar $\hat y$ is the **prediction**
      3. Vector Norm
         1. Euclidean norm of an n-vector $x$ is $||x||=\sqrt{x_1^2+x_2^2+\cdots+x_n^2}=\sqrt{x^Tx}$
         2. Properties for n-vector $x,y$ and scalar $\beta$
            1. homogeneity: $||\beta x||=|\beta|||x||$
            2. triangular inequality: $||x+y||\le||x||+||y||$
            3. non-negativity: $||x||\ge 0$
            4. definiteness $||x||=0 \iff x=0$
      4. Cosine
         1. $\cos(a,b) = \frac{a^Tb}{||a||||b||}$
         2. $\angle(a,b)=\arccos(\frac{a^Tb}{||a||||b||})$
         3. $\angle(a,b)$ should be $[0,\pi]$
         4. $a^Tb = ||a||||b||\cos(\angle(a,b))$

### 2. Matrix

- Matrix Inverse
  - The **rank** of a matrix = the number of **linearly independent rows**
  - A 𝑛 × 𝑛 matrix is **full rank** when it has 𝑛 **linearly independent rows**.
  - A matrix has an **inverse** (i.e., is vertible) if and only if it is **full rank**
  - $AA^{-1} = I$
    - $AA^{-1} \rightarrow A^{-1}A=I$
  - If $A$ has an inverse, $A^T$ also has an inverse
    - $(AA^{-1})^T=I^T=I$
    - $(A^{-1})^TA^T=I$
  -  That is, if $A$ has linearly independent rows, it must also have  linearly independent columns.
- Linear regression
  - for data matrix $X\in R^{n\times p}$ and lable vector $Y\in R^{n\times 1}$
  - minimize $||X\beta-Y||^2$
  - the best $\beta$ is $\beta^* = (X^TX)^{-1}X^TY$
- Orthonormal Matrix
  - A matrix $A$ is **orthonormal** if its **rows and columns** have 
    - **unit length** and 
    - are **orthogonal to each other**
  - $i=j \rightarrow a_i^Ta_j = 1$
  - $i\neq j \rightarrow a_i^Ta_j = 0$
  - $AA^T=A^TA=I$
  - $A^T=A^{-1}$
- Geometric Interpretation of Matrix
  - A matrix represents a **transformation** of a vector
  - $\begin{bmatrix}\cos\theta & -\sin\theta \\ \sin\theta &\cos\theta\end{bmatrix}$ performs a rotation of $\theta$

### 3. Eigenvalues and Eigenvectors
- A non-zero vector 𝑣 is an eigenvector of square matrix 𝐴 if
  - $Av = \lambda v$
- $\lambda$ is called the eigenvalue associated with $v$
- Generically speaking, $Av$ does not change the direction of $v$ (except when $\lambda = 0$)
- Eigendecomposition
  - A square 𝑛 × 𝑛 matrix $M$ can be written as $M=QΛQ^{-1}$
  - $Q$ is a 𝑛 × 𝑛 matrix whose i-th column is the eigenvector $q_i$
    - These eigenvectors are usually normalized to length 1
  - $Λ$ is a diagonal matrix with the eigenvalues on the diagonal.
  - It requires $M$ to be diagonalizable
  - Derivation
    - For eigenvector $q$, we have $Mq = \lambda q$
    - So for $[q_1,\dots,q_n]$, we can write $M[q_1,\dots,q_n]=[q_1,\dots,q_n]\begin{bmatrix}\lambda_1 &0 &\dots &0 \\ 0 &\lambda_2 &\dots &0 \\ \vdots &\vdots &\ddots &\vdots \\ 0 &0 &\dots &\lambda_n \end{bmatrix}$
      - i.e. $MQ=QΛ$
      - For a diagonalizable matrix, 𝑄 is full rank -> invertible
  - We can freely switch the positions of eigenvalues, as long as we keep $(\lambda_i, q_i, v_i)$ together
    - Many math texts assume the eigenvalues are sorted 
  - For real **symmetric** matrices
    - The eigenvectors are real and usually chosen to be orthonormal. That is, $𝑄^{−1} = 𝑄^⊤$

### Symmetric Matrix
- A square matrix $A$ is **symmetric** iff $A_{ij} = A_{ji}, \forall i,j$
- A square matrix is **anti-symmetric** (or skew-symmetric) iff $A_{ij}=-A_{ji} \forall i\neq j$
- Properties
  - $A=A^T$
  - $A^k$ is symmetric for integer $k$
  - $A^{-1}$ is symmetric, if it exists
- Any matrix $X$ can be written as the sum of a symmetric and an anti-symmetric matrix. 
  - Symmetric $\frac{1}{2}(X+X^T)$
  - Anti-Symmetric $\frac{1}{2}(X-X^T)$

### Hessian Matrix
- Jacobian matrix $[\frac{dg(x)}{dx_1},\dots,\frac{dg(x)}{dx_p}]^T$
- Hessian matrix is second derivative
  - is symmetric


### Positive Definiteness
- A matrix $A$ is positive definite (PD) iff for all vector $x\neq 0$, $𝑥⊤𝐴𝑥 > 0$
- Non-symmetric PD matrices can be made symmetric by adding an anti-symmetric zero-diagonal matrix to it
  - $x^T\begin{bmatrix}2 &-2 &0 \\ 0 &3 &0 \\ 0 &-2 &1 \end{bmatrix} x = x^T\begin{bmatrix}2 &-1 &0 \\ -1 &3 &-1 \\ 0 &-1 &1 \end{bmatrix}x$
  - we can write C=A+B
    - $A$ is symmetric PD matrix
    - $B$ is an anti-symmetric with 0 diagonals
- Properties
  - If 𝐴 is PD and 𝐵 is PD, 𝐴 + 𝐵 is PD
    - $x^T(A+B)x = x^TAx + x^TBx >0$
  - If 𝐴 is PD and scalar 𝛼 is positive, 𝛼𝐴 is PD
    - $x^T(\alpha A)x = ax^TAx>0$
  - If 𝐴 is symmetric and PD, **all eigenvalues of 𝐴 are positive**
- 与特征向量的关系
  - Positive definite: All eigenvalues > 0
  - Positive semi-definite: All eigenvalues ≥ 0
  - Negative definite: All eigenvalues < 0
  - Negative semi-definite: All eigenvalues ≤ 0
  - **Some matrices do not belong to any of these categories**
    - 比如有的既有positive也有negative eigenvectors
- **Any matrix** in the form of $M^TM$ is **Positive-Semi Definite**