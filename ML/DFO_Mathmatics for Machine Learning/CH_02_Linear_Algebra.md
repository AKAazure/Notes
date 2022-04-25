# Mathmatics for Machine Learning

## Chapter 02 Linear Algebra
- ***algebra***
  - When formalizing intuitive concepts, a common approach is to construct 
    - a set of objects (symbols) 
    - and a set of rules to manipulate these objects. 
  - This is known as an algebra.
- Linear algebra is the study of **vectors** and certain **algebra rules** to manipulate vectors.
  - In general, vectors are special objects that can be 
    - added together 
    - and multiplied by scalars 
  - to produce another object of the same kind. 

### 2.1 Systems of Linear Equations
- the general form of a system of linear equations
  - $$\begin{align}a_{11}x_1 + \cdots + a_{1n}x_n&= b_1 \\ \dots \\ a_{m1}x_1 + \cdots + a_{mn}x_n &= b_m \end{align}$$
  - $x_1, . . . , x_n$ are the ***unknowns*** of this system
  - Every **n-tuple** $(x_1, . . . , x_n) ∈ R^n$ that satisfies it is a ***solution*** of the linear equation system.
- In general, for a **real-valued system** of linear equations we obtain either
  - **no**, 
  - **exactly one**, 
  - or **infinitely many** solutions.

### 2.2 Matrices
- . They can be used to compactly represent 
  - **systems of linear equations**, 
  - and **linear functions** **(linear mappings)**
- ***Matrix*** 
  - **Definition 2.1 (Matrix).** With $m, n ∈ N$ a real-valued $(m, n)$ 
  - ***matrix*** $A$ is an $m·n$-tuple of elements $a_{ij} , i = 1, . . . , m, j = 1, . . . , n$, 
    - which is ordered according to a rectangular scheme consisting of m rows and n columns: $$A= \begin{bmatrix}a_{11}&a_{12}&\cdots &a_{1n} \\ a_{21}&a_{22}&\cdots &a_{2n} \\ \vdots & \vdots &\ddots &\vdots \\ a_{m1}&a_{m2}&\cdots &a_{mn} \end{bmatrix}, a_{ij}\in R$$
  - By convention 
    - $(1, n)$-matrices are called ***rows*** 
    - $(m, 1)$-matrices are called ***columns***. 
    - These special matrices are also called ***row/column vectors***.
- $R^{m\times n}$ is the set of all real-valued (m, n)-matrices.
- $A ∈ R^{m\times n}$ can be equivalently represented as $a ∈ R^{mn}$ by stacking all $n$ columns of the matrix into a long vector; 

#### 2.2.1 Matrix Addition and Multiplication
- ***Addition***
  - The sum of two matrices $A ∈ R^{m\times n} , B ∈ R^{m\times n}$ is defined as the element-wise sum $$A+b\coloneqq \begin{bmatrix}a_{11}+b_{11}&\cdots &a_{1n}+b_{1n}\\ \vdots &\ddots &\vdots \\ a_{m1}+b_{m1}&\cdots &a_{mn}+b_{mn}\end{bmatrix} \in R^{m\times n}$$
- ***Multiplication*** 
    - For matrices $A ∈ R^{m\times n}, B ∈ R^{n\times k}$ , the elements $c_{ij}$ of the product $C = AB ∈ R^{m\times k}$ are computed as $$c_{ij}=\sum^{n}_{l=1}a_{il}b_{lj}, \\ i=1,\dots,m \\ j=1,\dots,k$$
- ***Identity Matrix*** **Definition 2.2 (Identity Matrix).**
- properties
  - ***Associativity***:
    - $\forall A ∈ R^{m\times n}, B ∈ R^{n\times p}, C ∈ R^{p\times q}: (AB)C = A(BC)$
  - ***Distributivity***
    - $$\begin{aligned}∀A, B ∈ R^{m×n}, C, D ∈ R^{n×p}: (A + B)C &= AC + BC \\A(C + D) &= AC + AD \end{aligned}$$
  - ***Multiplication with the identity matrix***:
    - $∀A ∈ R^{m\times n}: I_mA = AI_n = A$
    - Note that $I_m \neq I_n$ for $m \neq n$

#### 2.2.2 Inverse and Transpose
- ***Inverse***
  - **Definition 2.3** (Inverse). Consider a square matrix $A ∈ R^{n×n} B ∈ R^{n×n}$ have the property that $AB = I_n = BA$. $B$ is called the ***inverse*** of $A$ and denoted by $A^{−1}$
  -  If this **inverse exist**, $A$ is called *regular/invertible/nonsingular*, **otherwise** *singular/noninvertible*. 
  - When the matrix inverse exists, it is **unique**
- ***Transpose***
  - **Definition 2.4 (Transpose).** For $A ∈ R^{m×n}$ the matrix $B ∈ R^{n×m}$ with $b_{ij} = a_{ji}$ is called the ***transpose*** of $A$. We write $B = A^{T}$.
- **The following are important properties of inverses and transposes:**
  - $$\begin{aligned}
    AA^{-1}&=I=A^{-1}A \\ (AB)^{-1}&=B^{-1}A^{-1}\\ (A+B)^{-1}&\neq 1A^{-1}+B^{-1}\\ (A^T)^T &= A \\ (A+B)^T &= A^T+B^T \\ (AB)^T &= B^TA^T\end{aligned}$$
- ***Symmetric Matrix***
  - **Definition 2.5 (Symmetric Matrix).** A matrix $A ∈ R^{n×n}$ is symmetric if symmetric matrix $A = A^T$

#### 2.2.3 Multiplication by a Scalar
- Let $A ∈ R^{m×n}$ and $λ ∈ R$. Then $λA = K$, $K_{ij} = λ a_{ij}$ .
- Practically, $λ$ scales each element of $A$. For $λ, ψ ∈ R$, the following holds:
  - Associativity:
    - $(λψ)C = λ(ψC)$, $C ∈ R^{m×n}$
  - Distributivity:
    - $(λ + ψ)C = λC + ψC$, $C ∈ R^{m×n}$
    - $λ(B + C) = λB + λC$, $B, C ∈ R^{m×n}$
  - $λ(BC) = (λB)C = B(λC) = (BC)λ$
  - $(\lambda C)^T=C^T\lambda^T=C^T\lambda=\lambda C^T$, since $\lambda = \lambda ^T$

#### 2.2.4 Compact Representations of Systems of Linear Equations
- Generally, a **system of linear equations** can be compactly represented in their matrix form as $Ax = b$
  - the product $Ax$ is **a (linear) combination** of the **columns of A**.

### 2.3 Solving Systems of Linear Equations

#### 2.3.1 Particular and General Solution 
- The general approach we followed consisted of the following 3 steps:
  1. Find a **particular solution** to $Ax = b$.
  2. Find **all solutions** to $Ax = 0$.
  3. Combine the solutions from steps 1. and 2. to the general solution.
- ***Gaussian elimination***

#### 2.3.2 Elementary Transformations
- ***elementary transformations***
  - **Exchange** of two equations (**rows**)
  - **Multiplication** of an equation (**row**) with a **constant** $λ ∈ R \backslash \{0\}$
  - **Addition** of two equations (**rows**)
- ***Pivots and Staircase Structure***
  - The leading coefficient of a row (first nonzero number from the left) is called the pivot and is always strictly to the right of the pivot of the row above it
- ***Row-Echelon Form REF*** 
  - **Definition 2.6 (Row-Echelon Form)**. A matrix is in row-echelon form if:
    - All rows that contain **only zero**s are at the bottom of the matrix;
    - Looking at nonzero rows only, the first nonzero number from the left (also called the pivot or the leading coefficient) is always strictly to the right of the pivot of the row above it.
- ***Basic and Free Variables***
  - The variables corresponding to the **pivots** in the row-echelon form are called ***basic variables***
  - the other variables are ***free variables***
- ***Reduced Row Echelon Form***
  - An equation system is in ***reduced row-echelon form*** (also: row-reduced echelon form or row canonical form) if
    - It is in row-echelon form.
    - Every pivot is 1.
    - The pivot is the **only** nonzero entry in its **column**

#### 2.3.3 The Minus-1 Trick
- We extend the matrix $A$ to an $n × n$-matrix $A˜$ by adding $n − k$ rows of the form $\begin{bmatrix}0 &\cdots &0 &-1 &0 &\cdots &0\end{bmatrix}$
![](./images/02/EX_2.8.png)