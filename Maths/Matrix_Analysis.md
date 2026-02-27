### Determinant

$$
    \det{A} = \sum_\sigma sgn\sigma \prod_i^n a_{i\sigma_i} \\
    \det{B} = \sum_\sigma sgn\sigma \prod_i^n b_{i\sigma_i} \\
    AB = 
        \begin{bmatrix}
            \sum_i^n{a_{1i}b_{i1}} & \cdots & \sum_i^n a_{1i}b_{in} \\
            \vdots & \ddots & \vdots \\
            \sum_i^n{a_{ni}b_{i1}} & \cdots & \sum_i^n a_{ni}b_{in} \\
        \end{bmatrix} \\
    \det{AB} = \sum_{\sigma_1} sgn{\sigma_1} \prod_i^n({\sum_j^na_{ij}b_{j\sigma_{1i}}}) \\
    \det{AB} = \sum_{\sigma_1,\sigma_2} sgn{\sigma_1} \prod_{i}^n(a_{i\sigma_2}b_{\sigma_2\sigma_1}) \\
    \det{A}\det{B} = (\sum_{\sigma_1}sgn\sigma_1\prod_i^n{a_{i\sigma_1}})(\sum_{\sigma_2}sgn\sigma_2 \prod_1^n{b_{i\sigma_2}}) \\
    \det{A}\det{B} = \sum_{\sigma_1,\sigma_2}{sgn\sigma_1 \ sgn\sigma_2\prod_{i}^n{a_{i\sigma_{i}}b_{i\sigma_{2}}}}
$$

Here $\sigma2$

Using $2\times2$ matrix to express calculation

$$
    A = \begin{bmatrix}
            a_{11} & a_{12} \\
            a_{21} & a_{22} \\
        \end{bmatrix}
                            \\
    B = \begin{bmatrix}
            b_{11} & b_{12} \\
            b_{21} & b_{22} \\
        \end{bmatrix}
                            \\
    \det A = (a_{11}a_{22} - a_{12}a_{21})\\
    \det B = (b_{11}b_{22} - b_{12}b_{21})\\
    AB = \begin{bmatrix}
            a_{11}b_{11} + a_{12}b_{21} &  a_{11}b_{12} + a_{12}b_{22} \\
            a_{21}b_{11} + a_{22}b_{21} & a_{21}b_{12} + a_{22}b_{22}\\
         \end{bmatrix} \\
$$


### Elementary row and Column operations


#### Type 1: 行交换式

$$

$$

#### Type 2: 

$$

$$

### Rank and Eigen

对于 One-off Block Matrix  $M$

当 $A$ 非奇异,即 $|A| \ne 0$

$$
    \begin{bmatrix}
        A & B \\
        0 & D \\
    \end{bmatrix}
    \begin{bmatrix}
        I_m & -A^{-1}B \\
        0 & I_n\\
    \end{bmatrix}
    =
    \begin{bmatrix}
        A & 0 \\
        0 & D \\
    \end{bmatrix}
    =
    \begin{bmatrix}
        I_m & 0 \\
        -CA^{-1} & I_n \\
    \end{bmatrix}
    \begin{bmatrix}
        A & 0 \\
        C & D \\
    \end{bmatrix}
$$

当 $D$ 非奇异，即 $|D| \ne 0 $

$$
    \begin{bmatrix}
        I_m & -BD^{-1} \\
        0 & I_n \\
    \end{bmatrix}
    \begin{bmatrix}
        A & B \\
        0 & D \\
    \end{bmatrix}
    =
    \begin{bmatrix}
        A & 0 \\
        0 & D \\
    \end{bmatrix}
    =
    \begin{bmatrix}
        A & 0 \\
        C & D \\
    \end{bmatrix}
    \begin{bmatrix}
        I_p & 0 \\
        -CD^{-1} & I_n\\
    \end{bmatrix}
$$

即 $rankM = rankA + rankB$

对于 NonSingular off-diagonal block

对于 
$$
    Z_1 = 
    \begin{bmatrix}
        A & 0 \\
        B & D \\
    \end{bmatrix}
$$

当 $B$ 为非奇异矩阵，这时，(前者看列，后者看行)

$$
    \begin{bmatrix}
        0 & I_n \\
        I_m & -AB^{-1} \\
    \end{bmatrix}
    \begin{bmatrix}
        A & 0 \\
        B & D \\
    \end{bmatrix}
    \begin{bmatrix}
        I_n & -B^{-1}D \\
        0 & I_q \\
    \end{bmatrix}
        =
    \begin{bmatrix}
        B & 0 \\
        0 & -DB^{-1}A \\
    \end{bmatrix}
$$

$$
    rk(Z_1) = rk(B) + rk(DB^{-1}A)
$$

对于

$$
    Z_2 = 
    \begin{bmatrix}
        A & C \\
        0 & D \\
    \end{bmatrix}
$$

当 $C$ 为非奇异矩阵,

$$
    \begin{bmatrix}
        I_m & 0 \\
        -DC^{-1} & I_n \\
    \end{bmatrix}
    \begin{bmatrix}
        A & C \\
        0 & D \\
    \end{bmatrix}
    \begin{bmatrix}
        0 & I_p \\
        I_m & -C^{-1}A \\
    \end{bmatrix}
    =
    \begin{bmatrix}
        C & 0 \\
        0 & -DC^{-1}A \\
    \end{bmatrix}
$$

$$
    rk(Z_2) = rk(C) + rk(AC^{-1}D)
$$

#### Rank Inequalities - 1



#### Rank Inequalities - 2



#### Sylvester Equation

#### 


#### 分块矩阵的逆

一对矩阵组，$A$,$B$ 

$A$, $B$ 可化为
$$
    A = 
    \begin{bmatrix}
        A_{11} & A_{12} \\
        A_{21} & A_{22} \\
    \end{bmatrix}
    \\
    B = 
    \begin{bmatrix}
        B_{11} & B_{12} \\
        B_{21} & B_{22} \\
    \end{bmatrix}
$$