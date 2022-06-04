## 凸函数的性质（在 $\mathbb{R}^n$ 上）

### Convex + Lipschitz: ${\cal{S}}^1_{0,L}(\mathbb{R}^n)$ 函数的性质

**Thm 2.1.6** （用$\nabla^2f(x)$表示的${\cal{S}}^1_{0,L}(\mathbb{R}^n)$的等价条件）
$$
f \in {\cal{S}}^1_{0,L}(\mathbb{R}^n) \iff 0 \leq \nabla^2f(x) \leq L I_n
,\quad \forall x \in \mathbb{R}^n
\tag{7}
$$

> 证明同Thm 2.1.6$^\prime$

**Thm 2.1.5** 若 $f \in {\cal{S}}^1_{0,L}(\mathbb{R}^n)$，则 $\forall x,y \in \mathbb{R}^n$ 有
$$
\frac 1{2L} \left\| \nabla f(y)-\nabla f(x) \right\|^2_*
\leq  f(y)-f(x) - \left\langle \nabla f(x),\ y-x \right\rangle   
\leq \frac L2 \left\| y-x \right\|^2
\tag{8.1}
$$

$$
\frac 1{L} \left\| \nabla f(y)-\nabla f(x) \right\|^2_*
\leq  \left\langle \nabla f(y) - \nabla f(x),\ y-x \right\rangle
\leq L \left\| y-x \right\|^2
\tag{8.2}
$$

$$
\frac{\alpha(1-\alpha)}{2L} \left\| \nabla f(y)-\nabla f(x) \right\|^2_*
\leq \alpha f(x) + (1-\alpha)f(y) - f\left( \alpha x + (1-\alpha)y \right) 
\leq \frac{\alpha(1-\alpha)L}2 \| y-x \|^2
\tag{8.3}
$$

> 证明：见笔记<函数性质证明>



### Strongly Convex: ${\cal{S}}_{\mu}(\mathbb{R}^n)$ 函数的性质

**Cor 2.1.8** 若 $f \in {\cal{S}}_{\mu}(\mathbb{R}^n)$ 且 $\nabla f(x^*) = 0$，则 $\forall x \in \mathbb{R}^n$ 有
$$
f(x) \geq f(x^*) + \frac12 \mu \| x-x^* \|^2
\tag{9.0}
$$
> 证明：由(2.2)，$f(x) \geq f(x^*) + \left\langle \nabla f(x^*),\ x-x^* \right\rangle + \frac \mu2 \| x-x^* \|^2 = f(x^*) + \frac12 \mu \| x-x^* \|^2 \ \square.$

**Thm 2.1.10** 若 $f \in {\cal{S}}_\mu(\mathbb{R}^n)$，则 $\forall x,y \in \mathbb{R}^n$ 有
$$
\mu \left\| x-y \right\| \leq \left\| \nabla f(x) - \nabla f(y) \right\|
\tag{9.1}
$$

$$
\frac \mu2 \left\| y-x \right\|^2
\leq  f(y)-f(x) - \left\langle \nabla f(x),\ y-x \right\rangle   
\leq \frac 1{2\mu} \left\| \nabla f(y)-\nabla f(x) \right\|^2_*
\tag{9.2}
$$

$$
\mu \left \| y-x \right\|^2
\leq  \left\langle \nabla f(y) - \nabla f(x),\ y-x \right\rangle
\leq \frac 1{\mu} \left\| \nabla f(y)-\nabla f(x) \right\|^2_*
\tag{9.3}
$$

$$
\frac{\alpha(1-\alpha)\mu}2 \| y-x \|^2
\leq \alpha f(x) + (1-\alpha)f(y) - f\left( \alpha x + (1-\alpha)y \right)
\tag{9.4}
$$

> 证明：见笔记<函数性质证明>
>
> 推广：类似Con+ Lip



### Strongly Convex + Lipschitz:  ${\cal{S}}^1_{\mu,L}(\mathbb{R}^n)$ 函数的性质

**Thm 2.1.12** 若 $f \in {\cal{S}}^1_{\mu,L}(\mathbb{R}^n)$，则 $\forall x,y \in \mathbb{R}^n$ 有
$$
\left\langle \nabla f(y) - \nabla f(x),\ y-x \right\rangle 
\leq \frac{\mu L}{\mu+L} \left\| y-x \right\|^2 + \frac{1}{\mu+L} \left\| \nabla f(y) - \nabla f(x) \right\|^2
\tag{10}
$$

> 证明：注意到一定有 $\mu \leq L$。如果 $\mu = L$，则 $\nabla^2 f(x)\equiv \mu=L$，此时 $f(x) = \frac\mu2\|x-x^*\|^2$，验证可知(10)式 $左=\mu\|y-x\|^2=右$。
>
> 如果 $\mu < L$，令 $h(x) = f(x) - \frac \mu2 \| x \|^2 \in {\cal{S}}^1_{0,L-\mu}(\mathbb{R}^n)$，由(8.2)可得
> $$
> \left\langle \nabla h(y) - \nabla h(x),\ y-x \right\rangle
> \geq \frac 1{L-\mu} \left\| \nabla h(y)-\nabla h(x) \right\|^2
> $$
> 即
> $$
> \left\langle \nabla f(y) - \nabla f(x),\ y-x \right\rangle - \mu \left\| y-x \right\|^2
> \geq \frac 1{L-\mu} \left\| \nabla f(y)-\nabla f(x) - \mu(y-x) \right\|^2
> $$
> 化简即可得(10) $\square.$

**Thm A.3.** 
$$
f \in {\cal{S}}^1_{\mu,L}(\mathbb{R}^n) \iff \\
f(y) - f(x) -\langle\nabla f(x) ,\ y-x\rangle \geq 
\frac{1}{2 L}\|\nabla f(y)-\nabla f(x)\|^{2} 
+\frac{\mu}{2(1-\mu / L)}\left\|y-x-\frac{1}{L}(\nabla f(y)-\nabla f(x))\right\|^{2}
$$

> 证明略



容易看出，在无约束条件下多出的性质一般是与函数梯度差的范数相关的性质。其中凸Lipschitz函数的性质与强凸函数的性质在结果上具有很强的对称性。



## 参考文献

本文主要结论参考

1.  *Lectures on Convex Optimization* by Yurii Nesterov
2.  *Acceleration Methods* by Alexandre d’Aspremont
