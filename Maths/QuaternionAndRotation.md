### Quaternion Calculation

- Vector $\hat{v}$ which represent

### Quaternion Vect Representation

$$
    Q_x = \begin{bmatrix}
          \end{bmatrix}

$$


### Rotation Matrix Calculation

$$

    R_z = 
    \begin{bmatrix}
        cos\psi & -sin\psi & 0 \\
        sin\psi & cos\psi & 0 \\
        0 & 0 & 1\\
    \end{bmatrix} \\

    R_y = 
    \begin{bmatrix}
        cos\theta & 0 & sin\theta \\
        0 & 1 & 0 \\
        -sin\theta & 0 & cos\theta\\ 
    \end{bmatrix} \\
    R_z = 
    \begin{bmatrix}
        1 & 0 & 0\\
        0 & cos\phi & -sin\phi \\
        0 & sin\phi & cos\phi \\
    \end{bmatrix}
    \\
    R = R_zR_yR_x=
    \begin{bmatrix}
        cos{\psi}cos{\theta} & \\
        sin{\psi}cos{\theta} & \\
        -sin{\theta} & \\
    \end{bmatrix}
$$