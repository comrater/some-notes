# 线性判别分析(LDA)

**线性判别分析**(Linear Discriminant Analysis，简称**LDA**)是一种经典的**线性分类模型**。从模型原理角度，LDA即是采用Fisher准则设计线性判别函数的线性分类模型。



线性分类模型的设计可以归结为准则函数的设计。



## Fisher准则

### 思想

线性分类的几何意义是用超平面分割样本集。分隔出的两类就是沿着超平面法线方向（即$w$方向）投影到一维子空间后某点（由$b$确定）的两侧。那么**寻找$w$，其实就是在寻找最优的投影方向。**

什么是最优的投影方向呢，Fisher准则希望，将样本集投影到一条直线上后，能使得**同类样本的投影点尽可能近，异类样本的投影点尽可能远**。翻译为数学语言即，同类样本的散度尽量小，异类样本的均值差尽量大。

### 计算过程

定义类别$i$的**样本均值** $$m_i = \frac1{N_i} \sum_{x \in \mathcal{X}_i} x$$

类别$i$的**类内散度矩阵** $$S_i =  \sum_{x \in \mathcal{X}_i} {(x-m_i)(x-m_i)^T}$$

**总类内散度矩阵** $$S_w = S_1 + S_2 =\sum_{i \in \mathcal{C}} \sum_{x \in \mathcal{X}_i} {(x-m_i)(x-m_i)^T}$$

**类间散度矩阵** $$S_b =  {(m_1-m_2)(m_1-m_2)^T}$$



由于$x$投影后为$w^Tx$，因此投影后的样本均值
$$
\begin{align}

 \tilde{m}_i &= \frac1{N_i} \sum_{x \in \mathcal{X}_i} w^Tx \\
​            &= w^T (\frac1{N_i} \sum_{x \in \mathcal{X}_i} x) \\
     	     &= w^T m_i

\end{align}
$$
投影后的类内散度（矩阵）
$$
\begin{align} 
\tilde{S}_i &= \sum_{x \in \mathcal{X}_i} {(w^T x-\tilde{m}_i)(w^T x-\tilde{m}_i)^T}\\
            &= \sum_{x \in \mathcal{X}_i} {(w^T x- w^T m_i)(w^T x-w^T m_i)^T}\\
            &= \sum_{x \in \mathcal{X}_i} {w^T( x - m_i)(x - m_i)^T w}\\
            &= w^T \sum_{x \in \mathcal{X}_i} {( x - m_i)(x - m_i)^T}  w\\
            &=  w^T S_i w
\end{align}
$$

$$
\begin{align} 
\tilde{S}_w &= \tilde{S}_1 + \tilde{S}_2\\
			&= w^T S_1 w + w^T S_2 w\\
			&= w^T (S_1 + S_2) w\\
			&= w^T S_w w
\end{align}
$$

投影后的类间散度（矩阵）
$$
\begin{align} 
\tilde{S}_b &= {(\tilde{m}_1-\tilde{m}_2)(\tilde{m}_1-\tilde{m}_2)^T}\\
            &= {(w^T m_1 - w^T m_2)(w^T m_1 - w^T m_2)^T}\\
            &= {w^T( m_1 - m_2)(m_1 - m_2)^T w}\\
            &= w^T S_b w
\end{align}
$$

将Fisher准则的思想对应为以上统计量，即希望$\tilde{S}_w$尽量小，$\tilde{S}_b$尽量大，因此准则函数可写为：
$$
max \quad   J_{Fisher}(w) = \frac{\tilde{S}_b}{\tilde{S}_w} = \frac{w^T S_b w}{w^T S_w w}
$$
以上就是Fisher准则，即LDA的









question：推广到多分类时，$S_i$可以直接推广，但$S_b$是这么做的：

step1.定义全局散度矩阵$$S_t =\sum_{i \in \mathcal{C}} \sum_{x \in \mathcal{X}_i} {(x-\mu)(x-\mu)^T} = S_b + S_w$$

step2.于是$S_b = S_t - S_w$计算得$$S_b =\sum_{i \in \mathcal{C}}  {N_i (\mu_i-\mu)(\mu_i-\mu)^T} $$

注意到当$\mathcal{C} = \{1,2\}$时，这里$S_b$的定义和之前的只相差常数倍（不影响Fisher准则函数）



question：对于$S_i$和$S_b$的定义可能有争议，比如$S_i$和$S_b$是否要除以总数，

——$S_b$除以总数反正对结果没有影响

——如果$S_i$除以总数，那它就是协方差矩阵了。会出现第二个问题：

​	是否要加权算$S_w$如果是，那这就是定义问题不改变Fisher准则；如果仍然直接加和，那么与多分类的这个推广不符合（得到另一种不知科不科学的准则了）





