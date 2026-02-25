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

对于 One-off Block Matrix

当 $A$ 与 $D$ 均为非奇异方阵时
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
        I_m & -BD^{-1} \\
        0 & I_n \\
    \end{bmatrix}
    \begin{bmatrix}
        A & B \\
        0 & D \\
    \end{bmatrix}
$$

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