### 1 优化问题

#### 4.1.1 基础表现形式

$$
    \begin{array}{rl}
      minimize& \  f_0(x) \\
      s.t.& \ f_i(x) \le 0, &i=1,\dots,m\\
      &\ h_i(x)=0, & i=1,\dots,p \\
     \end{array}
$$

其中 $x\in R^n$，$n$ 维实数向量是该优化问题的*优化变量*，而 $R^n \to R$ 的函数 $f_0$ 则是该优化问题中的*目标函数* 或*损失函数*.


### 优化问题的对偶问题

#### 基础定义

优化问题中的对偶问题一般也被称为拉格朗日对偶问题。拉格朗日对偶问题，就是将约束加权之后对目标函数进行一个增广。

可以将对应优化问题的拉格朗日 *Lagrangian* 函数 $L$ 定义为 $L:R^n \times R^m \times R^p \to R$ 

$$
    L(x,\lambda,v) = f_0(x) + \sum_{i=1}^m\lambda_if_i(x) + \sum_{i=1}^m{v_ih_i(x)}
$$

然后对应函数 $L$ 的定义域为 $dom \ L = D \times R^m \times R^p$，而对应的 $v_i$ 与 $h_i(x)=0$ 相关联。而 $\lambda, v$ 被称为对偶变量和拉格朗日乘子。

#### 拉格朗日对偶方程

我们将拉格朗日对偶方程 *Lagrange dual function* $g:R^m \times R^p \to R$ 这里主意是将 $\lambda$ 和 $v$ 作为自变量计算某个 $x , x \in D$

$$
g(\lambda,v) = \inf_{x\in D}(f_0(x)+\sum_{i=1}^m\lambda_if_i(x) + \sum_{i=1}^pv_ih_i(x))
$$

-  当该 Lagrangian 在特定 $x$ 下无下界，这个时候该对偶函数值趋近于 $-\infin$.由于该对偶函数是无穷多个逐点下确界，即使，即使原问题不是一个凸函数，该对偶 $g(\lambda,v)$ 函数也是一个凹函数。

#### 最优值的下界

待优化问题的最优值 $p*$ ，满足对于任意$\lambda \ge 0$ 和 $v$ 都有
$$
    g(\lambda,v) \le p*
$$
证明 :
    
假设 $\hat{x}$ 是目标函数的其中一个可行点，$f_i(\hat{x}) \le 0$ 和 $h_i(\hat{x})=0$, 在 $\lambda \ge 0$时我们将有
$$
    \sum_{i=1}^m\lambda_if_i(\hat{x}) + \sum_{i=1}^pv_ih_i(\hat{x}) \le 0
$$
也因此
$$
    L(\hat{x},\lambda,v)=f_0(\hat{x}) + \sum_{i=1}^m\lambda_if_i(\hat{x}) + \sum_{i=1}^pv_ih_i(\hat{x}) \le f_0(\hat{x})
$$
故而
$$
    g(\lambda,v) = \inf_{x\in D} L(x,\lambda,v) \le L(\hat{x},\lambda,v) \le f_0(\hat{x})
$$
当 $\lambda \ge 0$ 和 $(\lambda,v) \in dom \ g$,给出了 $f_0$ 在 $p*$ 的非奇异下界
$$
    g(\lambda,v) = -\infin
$$

#### 线性逼近法

拉格朗日函数及其下界性质可以通过基于集合 {0} 和 $-R_+$的指示函数的线性逼近来给出一种简单的解释。

先将原问题转换为一个有约束的问题。

$$
\begin{array}{l}
    minimize & f_0(x) + \sum_{i=1}^mI\_(f_i(x)) + 
    \sum_{i=1}^pI_0(h_i(x))
\end{array}
$$

再将指示函数替换为软约束
软约束在利用中间点 interior-point method 的时候特别有用。

#### 线性方程组的最小二乘

$$
    \begin{array}{rl}
        minimize & x^Tx \\
        s.t. &Ax=b
    \end{array}
$$

对应优化问题的拉格朗日函数为
$$
    L(x,v) = x^Tx + v^T(Ax-b)
$$
函数的定义域$R^n \times R^p$
固定$x$，对偶函数 $g(v)=\inf_xL(x,v)$
对函数求梯度，
$$
    \nabla_xL(x,v) = 2x + A^Tv =0
$$
该梯度方程给出 $x = \frac{-A^Tv}{2}$
将新的$x$关于$v$的方程带入
$$
    g(v)=L(\frac{-A^Tv}{2},v) = \frac{-v^TAA^Tv}{4} - b^Tv
$$
同时这个函数也是一个凸函数，在定义域 $R^p$时，将满足小于原函数的值

$$
    \frac{-v^TAA^Tv}{4} - b^Tv \le \inf \{x^Tx | A^Tx =b \}
$$

#### 标准线性划分

线性方程组的标准形式：
$$
    \begin{array}{rl}
        minimize & c^Tx \\
        s.t. & Ax=b \\
        & x \ge 0
    \end{array}
$$
对应的拉格朗日函数：
$$
    L(x,\lambda,v) = c^Tx - \lambda^Tx + v^T(Ax -b) = -b^Tv + (c + A^Tv - \lambda)^Tx
$$
对应的对偶函数：
$$
    g(\lambda,v)= \inf_xL(x,\lambda,v)=-b^Tv+\inf_x(c+A^Tv-\lambda)^Tx
$$
对对偶函数进行分析：
$$
    g(\lambda,v)=
    \begin{array}{rl}
        -b^Tv & A^Tv-\lambda + c = 0\\
        -\infin & otherwise \\
    \end{array}
$$


#### 两路划分问题

对一个非凸函数进行优化
$$
    \begin{array}{rl}
        minimize & x^TWx \\
        s.t. & x^Tx = 1
    \end{array}
$$

该问题的拉格朗日形式为：

$$
    \begin{array}{rl}
        L(x,v)  = & x^TWx +\sum_{i=1}^nv_i(x_i^2 - 1)\\
        = & x^T(W + diag(v))x - v
    \end{array}
$$

其对应的拉格朗日对偶函数为:

$$
    \begin{array}{rl}
    g(v) = & \inf_xx^T(W+diag(v))x - v \\
        = & \begin{cases}
                -v & W+diag(v) \ge 0 \\
                -\infin & otherwise
            \end{cases}
    \end{array}
$$

在这里$W$是否是半正定决定了该二次型的下确界是0还是$-\infin$.

考虑拉格朗日对偶函数中的$W+diag(v) \ge 0$
$$
    v=-\lambda_{min}(W)
$$
在对偶可行区间，
$$
    W+diag(v)=W - \lambda_{min}(W)I \ge 0
$$
同时该对偶函数也指出了原优化问题的最优值下界

$$
    p* \ge -v = n\lambda_{min}(W)=n\lambda(W)
$$

优化问题变体

$$
    \begin{array}{rl}
        minimize & x^TWx \\
        s.t. & \sum_{i=1}^n x_i^2 = n
    \end{array}
$$


#### 拉格朗日对偶函数与共轭函数

对偶函数和共轭函数彼此紧密联系

$f(x)$ 的共轭函数形式
$$
    f^*(y) = \sup_{x\in dom f}(y^Tx - f(x)) \ y \in R^n
$$
这个与 LP 的优化问题的对偶函数有紧密联系
$$
    \begin{array}{rl}
        minimize & f(x) \\
        s.t. & x=0\\
    \end{array}
$$
这个优化问题拉格朗日函数形式为 $L(x,v)=f(x) + v^Tx$
对偶函数为:
$$
    g(v)=\inf_x(f(x)+v^Tx)=-\sup_{x}((-v)^Tx - f(x))=-f*(-v)
$$
变成了共轭函数的形式

对问题进行泛化到标准型优化问题

$$
    \begin{array}{rl}
        minimize & f_0(x) \\
        s.t. & Ax \le b \\
             & Cx = d
    \end{array}
$$
可以尝试用共轭函数替换对偶函数中的带 $x$ 项
$$
    \begin{array}{rl}
        g(\lambda,v) = &\inf_x(f_0(x) + \lambda^T(Ax -b)+v^T(Cx-d)) \\
        = & -b^T\lambda - d^Tv + \inf_x(f_0(x) + (A^T\lambda+C^Tv)^Tx) \\
        = & -b^T\lambda - d^Tv - \sup_x(-(A^T\lambda+C^Tv)^Tx - f_0(x)) \\
        = & -b^T\lambda - d^Tv - f_0^*(-A^T\lambda-C^Tv) \\
    \end{array}
$$

该对偶函数的定义域也可以用对偶函数来表达
$$
    dom g= \{(\lambda,v)|-A^T\lambda -C^Tv \in  f^*_0 \}
$$

#### 等式约束范数最小化
#### Equality constrained norm minimization

绝对值表达式
$$
    \begin{array}{rl}
        minimize & \lVert x \rVert \\
        s.t. & Ax = b \\
    \end{array}
$$
对于范式$f_0=\lVert . \rVert$

#### 熵最大化
#### Entropy Maximization


#### 最小体积覆盖椭球
#### Minimum Volume Covering Ellipsoid


### 拉格朗日对偶问题




