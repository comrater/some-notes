- **多元函数Taylor展开**：

  https://zhuanlan.zhihu.com/p/32274749*多元函数转化为一元函数（常用技巧）*

  一阶与二阶展开：

$$
\begin{align}
f(y) - f(x) &= (y-x)^T \int_0^1 {\nabla f(x+\theta(y-x))} \,{\rm d}\theta\\
&= (y-x)^T  {\nabla f(x+\theta_0(y-x))} \\

\nabla f(y) - \nabla f(x) &= (y-x)^T \int_0^1 {\nabla^2 f(x+\theta(y-x))} \,{\rm d}\theta\\
&= (y-x)^T {\nabla^2 f(x+\theta_0(y-x))}\\

f(y) - f(x) - (y-x)^T {\nabla f(x)} &= (y-x)^T \left[ \int_0^1 {\nabla^2 f(x+\theta(y-x))} (1-\theta) \,{\rm d}\theta \right] (y-x)^T\\
&= (y-x)^T \left[  \frac{\nabla^2 f(x+\theta_0(y-x))}2  \right] (y-x)^T\\

\end{align}
$$

*注意：积分型余项比拉格朗日余项更精细，特别是当有多个余项存在时，可以通过对积分式的变形进行化简。而拉格朗日余项由于 $\theta_0$ 未知，只能分别地放缩为上界。*

*e.g. https://arxiv.org/abs/1905.02637 eq(32)*



- **Young不等式**：

  https://zhuanlan.zhihu.com/p/41654910 Young不等式各种形式的推广

  假设 $a,b, \varepsilon>0, \quad p,q>1, \quad \frac1p + \frac1q =1$

  - 基础版
    $$
    ab \leq \frac{a^{p}}{p}+\frac{b^{q}}{q}\tag1
    $$
    等号成立当且仅当 $a^p = b^q$

    证明：
    $$
    a b=e^{\ln (a)} e^{\ln (b)}=e^{\frac{1}{p} \ln \left(a^{p}\right)+\frac{1}{q} \ln \left(b^{q}\right)} \leq \frac{1}{p} e^{\ln \left(a^{p}\right)}+\frac{1}{q} e^{\ln \left(b^{q}\right)}=\frac{a^{p}}{p}+\frac{b^{q}}{q}
    $$

  - 进阶版
    $$
    a \cdot b  \leq \varepsilon \frac{a^{p}}{p}+\varepsilon^{-\frac{q}{p}} \frac{b^{q}}{q}\tag2
    $$

  - $ p = q = 2 $ 时
    $$
    2ab  \leq \varepsilon {a^{2}}+\varepsilon^{-1} {b^{2}}\tag3
    $$
    证明：
    $$
    a \cdot b=\left(\varepsilon^{\frac{1}{p}} a\right)\left(\varepsilon^{-\frac{1}{p}} b\right) \leq \varepsilon \frac{a^{p}}{p}+\varepsilon^{-\frac{q}{p}} \frac{b^{q}}{q}
    $$

*注意：Young不等式是均值不等式的推广。其中进阶版可以通过对 $\varepsilon $ 的调节控制两项的大小*

*e.g. https://arxiv.org/abs/1905.02637 eq(34)*



- **均值与两两差**：

$$
\sum_{j=1}^m {(x_j - x_i)^2} = \sum_{j=1}^m {(x_j - \bar{x})^2} + m(x_i - \bar{x})^2\\
\sum_{j=1}^m {(x_j - x_i)^2} \leq 2(\sum_{j=1}^m {(x_j - y)^2} + m(x_i - y)^2)\\
\sum_{i=1}^m \sum_{j=1}^m {(x_i - x_j)^2} = 2m \sum_{i=1}^m {(x_i - \bar{x})^2}\\
$$



- **Kronecker product**：

如果*A*是一个 *m* × *n* 的矩阵，而*B*是一个 *p* × *q* 的矩阵，**克罗内克积**$A\otimes B$则是一个 *mp* × *nq* 的分块矩阵
$$
A\otimes B = 
\begin{bmatrix} 
a_{11}B &\cdots& a_{1n}B \\ 
\vdots & \ddots & \vdots \\ 
a_{m1}B &\cdots& a_{mn}B \\ 
\end{bmatrix}\\

\begin{bmatrix} 
A_1 & \cdots & A_n \\ 
\end{bmatrix}
\otimes B = 
\begin{bmatrix} 
A_1\otimes B & \cdots & A_n\otimes B \\ 
\end{bmatrix}\\

\begin{bmatrix} 
A_1^T \\ \vdots \\ A_n^T \\ 
\end{bmatrix}
\otimes B = 
\begin{bmatrix} 
A_1^T\otimes B \\ \vdots \\ A_n^T\otimes B \\ 
\end{bmatrix}
$$
*注意：可以用于向量拼接后描述原本运算，特别是 $A\otimes I$*

*e.g. https://arxiv.org/abs/1905.02637 eq(26)*