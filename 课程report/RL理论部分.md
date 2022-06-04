# 理论部分

本次报告的理论部分介绍有限马尔科夫决策过程与强化学习的理论知识、单步时序差分算法的理论知识和论文《xxx》中具体使用的算法三部分内容。

## 有限马尔科夫决策过程(finite MDP)

### 基本概念

**MDP（马尔科夫决策过程）**是基于马尔科夫性假设的一种决策序列模型，是一种通过交互式学习来实现目标的理论框架。MDP的原理在最优控制、强化学习等领域得到了广泛的应用。它包含**智能体(agent)**、**环境(environment)**、**状态(state)**、**动作(action)**、**收益(reward)** 5大要素。每个时刻，智能体在当前状态 $s$ 下选择一个动作 $a$ 后，环境对动作做出相应，反馈给智能体新的状态 $s^\prime $ 和一个收益 $r$ ，这一过程发生的可能性定义为概率$^{[1]}$
$$
p(s^\prime,r\ |\ s,a) \triangleq \Pr\{ S_t=s^\prime,R_t=r\ |\ S_{t-1}=s, A_{t-1}=a \}
\tag1
$$
由此定义可以看出，下一时刻智能体的状态与收益只与当前时刻的状态和动作有关，与历史信息无关，体现了MDP状态转移的马尔科夫性。

**有限MDP**是指智能体状态、动作和收益的集合($\cal{S、A、R}$)都只含有有限个元素的MDP。$\cal{S、A、R}$ 刻画了MDP的静态特性，而概率 $p(s^\prime,r\ |\ s,a)$ 刻画了MDP的动态特性。它们共同确定了这个MDP决策过程中“环境”的数学内涵。

![image-20210506125731396](C:\Users\yq\AppData\Roaming\Typora\typora-user-images\image-20210506125731396.png)



在一个MDP过程里，智能体的目标是给出一个当前环境下的最优策略。**策略**是指智能体处于某个状态 $s$ 时，它选择各种可能的行动 $a$ 的概率，即策略 $\pi$ 定义为
$$
\pi(a|s) \triangleq \Pr\{ A_t=a|S_{t}=s \}
\tag2
$$
那么什么策略是最优的，这取决于我们实际问题中要实现的目标是什么。这个目标我们用**回报**来描述。一般情况下，我们定义时刻 $t$ 的回报 $G_t$ 为： 
$$
G_t \triangleq R_{t+1} + \gamma R_{t+2} + \gamma^2R_{t+3} +\cdots = \sum_{k=0}^\infin \gamma^k R_{t+k+1}
\tag3
$$
其中 $0\leq\gamma\leq1$ 是一个参数，被称为**折扣率**。 当 $\gamma=1$ 时，(3)式是容易理解的，即从此刻起未来所有收益的总和就是这一时刻的回报。之所以要加入折扣率，一方面是数学上可以使求和式易于收敛，另一方面则是实际问题中，当前的收益是确定的，而未来的收益基于模型的预测，是不确定的，价值相对更低，因此通过折扣率 $\gamma$ 来修正智能体综合考虑当下与未来的权重大小。

有了回报的定义，最优策略就可以用期望得到的回报最大来描述，即我们希望找到的**最优策略** $\pi_*$ 满足
$$
\mathbb{E}_{\pi_*} \left[ G_t\ |\ S_t=s \right] \geq \mathbb{E}_\pi \left[ G_t\ |\ S_t=s \right]
\qquad\forall \pi,\ s
\tag4
$$
（注：根据Bellman最优原理，这个最优策略是存在的。）



### 求解最优策略

为了求解最优策略，我们需要定义策略 $\pi$ 的两个价值函数：

我们定义**策略 $\pi$ 的状态价值函数** $v_\pi(s)$ 为：
$$
v_\pi(s) \triangleq \mathbb{E}_\pi \left[ G_t\ |\ S_t=s \right]
\tag5
$$
表示智能体在状态 $s$ 下，按照策略 $\pi$ 决策所得到的期望回报。

定义**策略 $\pi$ 的动作价值函数** $q_\pi(s,a)$ 为：
$$
q_\pi(s,a) \triangleq \mathbb{E}_\pi \left[ G_t\ |\ S_t=s,A_t=a \right]
\tag6
$$
表示智能体在状态 $s$ 下执行动作 $a$ 后，按照策略 $\pi$ 决策所得到的期望回报。



计算状态价值函数 $v_\pi(s)$ 可以使用**贝尔曼方程**：
$$
\begin{align}
v_{\pi}(s) & \doteq \mathbb{E}_{\pi}\left[G_{t} \mid S_{t}=s\right] \\
&=\mathbb{E}_{\pi}\left[R_{t+1}+\gamma G_{t+1} \mid S_{t}=s\right] \\
&=\sum_{a} \pi(a \mid s) \sum_{s^{\prime}} \sum_{r} p\left(s^{\prime}, r \mid s, a\right)\left[r+\gamma \mathbb{E}_{\pi}\left[G_{t+1} \mid S_{t+1}=s^{\prime}\right]\right] \\\
&=\sum_{a} \pi(a \mid s) \sum_{s^{\prime}, r} p\left(s^{\prime}, r \mid s, a\right)\left[r+\gamma v_{\pi}\left(s^{\prime}\right)\right], \quad \text { for all } s \in \mathcal{S}\tag7
\end{align}
$$
求解 $|\mathcal{S}|$ 个线性方程组成的方程组得到。



求解最优策略，就相当于最大化价值函数，通过类似的推导，我们可以得到求解最优价值函数的**贝尔曼最优方程**：
$$
\begin{aligned}
v_*(s)&=\max _{a} \mathbb{E}\left[R_{t+1}+\gamma v_{*}\left(S_{t+1}\right) \mid S_{t}=s, A_{t}=a\right]\\
&=\max _{a} \sum_{s^{\prime}, r} p\left(s^{\prime}, r \mid s, a\right)\left[r+\gamma v_{*}\left(s^{\prime}\right)\right]
\end{aligned}
\tag{8.1}
$$

$$
\begin{aligned}
q_{*}(s, a) &=\mathbb{E}\left[R_{t+1}+\gamma \max _{a^{\prime}} q_{*}\left(S_{t+1}, a^{\prime}\right) \mid S_{t}=s, A_{t}=a\right] \\
&=\sum_{s^{\prime}, r} p\left(s^{\prime}, r \mid s, a\right)\left[r+\gamma \max _{a^{\prime}} q_{*}\left(s^{\prime}, a^{\prime}\right)\right] .
\end{aligned}
\tag{8.2}
$$

一旦求出了最优的价值函数，就可以轻易得到最优策略。具体而言，在得到 $v_*$ 后，可以通过单步搜索的贪心策略得到全局最优动作，从而得到最优策略 $$\pi_*$$ ；在得到 $q_*$ 后，只需要选择取值最大的 $q_*(s,a)$ 就可以得到状态 $s$ 时的全局最优动作 $a$ ，从而得到最优策略 $$\pi_*$$ 。



### 基本问题

对于实际中的MDP模型，我们关注两类问题：**预测问题**和**控制问题**。预测问题是指，当智能体的当前策略 $\pi$ 确定时，如何求出该策略的价值函数。控制问题是指，对于给定的MDP模型，寻找出智能体的最优策略。解决预测问题是解决控制问题的前提，或者说，预测问题可以看作控制问题的一个子问题。一个经典的求解MDP模型控制问题的框架叫做广义策略迭代(GPI)。GPI的思想便是通过交替的完成对当前策略价值函数的计算和根据当前策略的价值函数改进策略两个步骤实现最优策略的求解。在本次实验中，我们将问题的一部分建模为了MDP模型的预测问题。



## 单步时序差分算法( TD(0) )

虽然在理论上，求解MDP问题有着明确的公式，但观察(7)、(8.1)、(8.2)可以发现，精确使用贝尔曼方程求解需要的计算量与状态空间的大小有关。由于实际问题中状态空间 $|\mathcal{S}|$ 巨大，因此在实际中使用贝尔曼最优方程精确求解几乎是不可能的。我们必须开发其他算法降低求解的复杂度。单步时序差分算法(TD(0)) 就是一种常见的求解方法。

TD(0)方法是一种迭代法，它遵循迭代方法的一般框架：
$$
新估计值 \leftarrow 旧估计值 + 步长 \times [目标 - 旧估计值]
$$
对于状态价值函数 $v_\pi(s)$，理想的“目标”自然是 $v_\pi(s)$ 的真实值 $\mathbb{E}_\pi \left[ G_t\ |\ S_t=s \right]$。然而这个值的计算涉及求期望和计算回报 $G_t$ 两个困难。TD(0)的基本思想是：用采样近似期望，用当前价值代替最优价值，即
$$
\begin{aligned}
\mathbb{E}_{\pi}\left[G_{t} \mid S_{t}=S\right]
&=\mathbb{E}_{\pi}\left[R_{t+1}+\gamma G_{t+1} \mid S_{t}=S\right] \\
&\approx\mathbb{E}_{\pi}\left[R_{t+1}+\gamma v_{\pi}\left(S_{t+1}\right) \mid S_{t}=S\right] \\
&\approx R+\gamma V\left(S^{\prime}\right)  \\
\end{aligned}
$$
其中 $R, S^{\prime}$ 分别是在状态 $S$ 时根据策略 $\pi$ 进行一次模拟采样后得到的收益和转移到的新状态。

因此最终的计算公式为：
$$
V(S) \leftarrow V(S)+\alpha\left[R+\gamma V\left(S^{\prime}\right)-V(S)\right]
$$

完整的TD(0)预测算法如下图：

![image-20210521021158861](C:\Users\yq\AppData\Roaming\Typora\typora-user-images\image-20210521021158861.png)



我们将TD(0)算法中价值更新的部分 $R_{t+1}+\gamma V\left(S_{t+1}\right)-V(S_t)$ 称作**TD误差**，记为 $\delta_t$。我们可以推导出：
$$
\begin{align}
G_t - V(S_t) &= R_{t+1}+\gamma G\left(S_{t+1}\right)-V(S_t) +\gamma V\left(S_{t+1}\right)- \gamma V\left(S_{t+1}\right) \\
&= \delta_t + \gamma \left( G_{t+1} - V(S_{t+1}) \right)
\end{align}
$$
即用TD误差可以建立起当前时刻与下一时刻算法对价值函数的估计的偏差之间的关系。

TD(0)算法可以看作是动态规划方法（用当前价值代替最优价值）和蒙特卡洛方法（用采样近似期望）思想的结合，很好地继承了两种方法各自的优点。相比与依靠纯模拟的蒙特卡洛方法，TD(0)算法不需要等待一个完整模拟的结束，只需要模拟一个时刻就可以立即得到估计值；相比与动态规划方法，TD(0)算法不需要一个环境模型，即描述收益和下一状态联合概率分布的模型。TD(0)的收敛性也具有理论保证。对于任意的策略 $\pi$， TD(0)都已经被证明能够收敛到 $v_\pi$。





[1] Montague P R. Reinforcement learning: an introduction, by Sutton, RS and Barto, AG[J]. Trends in cognitive sciences, 1999, 3(9): 360.

