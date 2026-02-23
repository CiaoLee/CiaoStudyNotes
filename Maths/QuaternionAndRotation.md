### Quaternion Calculation

- Vector $\dot{v}$ which represent

### Quaternion Vector Representation

for Quaternion representing rotation through an angle $\theta$ about axis $\vec{v}$ 

$$
    \dot{q} 
            = \begin{bmatrix}
            q \\
            \vec{q}
          \end{bmatrix}
          =\begin{bmatrix}
                \cos{\theta} \\
                \sin{\theta}v_xi \\
                \sin{\theta}v_yi \\
                \sin{\theta}v_zi \\
            \end{bmatrix}
$$

Rotation represented by quaternion multiplicaiton

$$
    \dot{q}\dot{r}\dot{q}^* 
        =  (q,\vec{q})(0,\vec{r})(q,-\vec{q})
        = (q,\vec{q})(\vec{r} \cdot \vec{q},q\vec{r} + \vec{q} \times \vec{r})\\
        =(q\vec{q}\vec{r} - q\vec{q}\vec{r},q^2\vec{r}+ 2q\vec{q}\times \vec{r} + \vec{q} \times \vec{q} \times \vec{r} + (\vec{q}\cdot\vec{r})\vec{q}) \\
        =(0,q^2\vec{r} + (\vec{q} \cdot \vec{q})\vec{r} + 2q\vec{q} \times \vec{r} + 2\vec{q} \times \vec{q} \times \vec{r} + (\vec{q} \cdot \vec{r})\vec{r} - (\vec{q} \cdot \vec{r})\vec{r}) \\
        = \vec{r} + 2q\vec{q} \times \vec{r} + 2 \vec{q} \times \vec{q} \times \vec{r}
$$

Quaternion matrix representation

$$
    M_{\dot{q}} = 
        \begin{bmatrix}
            1 - 2(q_2^2 - q_3^2) & 2(q_1q_2-q_0q_3) & 2(q_0q_2 + q_1q_3)\\
            2(q_0q_3 + q_1q_2) & 1 - 2(q_1^2 - q_3^2) & 2(q_2q_3 - q_0q_1) \\
            2(q_1q_3 -q_0q_2) & 2(q_0q_1 + q_2q_3) & 1 - 2(q_1^2 - q_2^2) \\
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
        cos{\psi}cos{\theta} & cos{\phi}(-sin{\psi}) + sin{\phi}sin{\theta}cos{\psi} & (-sin{\phi})(-sin{\psi}) + cos{\phi}sin{\theta}cos{\psi}\\
        sin{\psi}cos{\theta} & cos{\phi}cos{\psi} + sin{\phi}sin{\theta}sin{\psi} & (-sin{\phi})(cos{\psi}) + cos{\phi}sin{\theta}sin{\psi}\\
        -sin{\theta} & sin{\phi}cos{\theta} & cos{\phi}cos{\theta}\\
    \end{bmatrix}
$$

### Unreal Codes: Construction Quaternion

```C++
    //Axis need to be the normalize one
    TQuat(const TVector<T> Aixs,const float AngleRad)
    {
        const T HalfA = 0.5f * AngleRad;
        T S,C;
        FMath::SinCos(S,C,HalfA);
        //First represent the Image part of quaternion
        X = S * Axis.X
        Y = S * Axis.Y;
        Z = S * Axis.Z;
        W = C;
        //Check for nan XYZW
    }
```

### Unreal Codes: Calculation from Euler to Quaternion 

```C++
    // The key
    
```

### Unreal Codes: Calculation from Quaternion to Euler

```C++
    FRotator FQuat::Rotator() const
    {
        //q_1 * q_3 - q_0 * q_2 // Corresponding to the rotation matrix 0.5f * M31 , 0.5f * -sin{\theta}
        const float SingularityTestValue =  Z * X - W * Y;
        //And we use M11, M21 to consider the tan{\psi} of yaw(Axix_Z)
        //cos{\psi}cos{\theta} 1-2.0f *(q_2*q_2-q_3*q3)
        const float Yaw1 = 1 - 2 * (Y * Y + Z * Z)
        //sin{\psi}cos{\theta} 2.0f * (q_0q_3 + q_1q_2)
        const float Yaw2 = 2.0f * (X * Y + Z * W)
        //Singularity test threshold 

        const float SingularityTestThreshold = 0.4999995f;


    }


```

### Unreal Codes: Spherical Interpolation through Quaternion

