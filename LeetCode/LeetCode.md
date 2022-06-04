# 通用trick

## 算法类

1. 优化算法的三种常见思路

   1. 序列变形：计算前k项和/相邻两项差/前k项的最大/最小值；
   2. 反向映射：按a的顺序保存对应的b转写为按b的顺序保存对应的a；
   3. 记录数组元素VS记录数组下标（某种程度上是2的特例）
   3. Hash

2. 常见思考问题的方式之**自变量因变量互换**：

   > e.g. LeetCode45.跳跃游戏II 问达到数组最后一个位置的最少跳跃次数，即**给定位置n, min次数t**——思考方法是求跳t次时数组达到的最远位置是多少，即**给定次数t, max位置n**，而后贪心
   >
   > e.g. LeetCode887.鸡蛋掉落 问**给定总楼层n，鸡蛋数k，min操作次数t**——方法3的思路是求**给定次数t，鸡蛋数k时，max最高可求解的楼层总数n**，而后递推。

3. DP的优化方法之决策单调性：指在状态转移方程中，最优决策argmin如果和状态之间具有单调性，那么就不需要每次都回溯计算了。

   >e.g. LeetCode887.鸡蛋掉落 ——方法2，优化动态规划时就可以知道，在k确定时，投掷的楼层p，相对于总楼层数n是单调的（很好理解）

4. 形似DP类的问题要时刻注意是不是可以压缩空间。

5. 前缀和的优势是可以将n个数的求和问题转化为2个数的求差问题，虽然直接使用不能优化时间复杂度，但 n -> 2 的变化为进一步使用其他算法提供了便利，如Hash表。

   > e.g. LeetCode560.和为K的子数组。不论是使用动态规划或只使用前缀和来计算所有连续子数组的和，都必须进行O(n^2)次计算。
   >
   > 最优O(n)解法的第一步是先转化数组为前缀和，这样目标变成了```nums[j] - nums[i - 1] == k```，可以使用Hash表进行O(n)时间内计算求和。
   >
   > 这一做法避免了计算不等于k的连续子数组的值，从而实现了复杂度的突破

6. Hash表可以理解为一个O(1)随机查找的神数据结构，在优化时间复杂度时有奇效。

7. 先排序是一种常见的预处理方式。

4. 头结点、结束符等等各种在开头和结尾甚至中间的辅助操作对简化边界条件逻辑有帮助。

## 场外类

1. **边界情况**：永远考虑一下输入极端的情况。（如输入空表）
2. **经验判断算法复杂度限制**：一般数据规模代入时间复杂度不要超过10^7  10^8 10^9. 经典case: $n\leq8 \rightarrow O(n!)$​, $n\leq20 \rightarrow O(2^n)$​, $n\leq500 \rightarrow O(n^3)$​, $n\leq1000 \rightarrow O(n^2)$​
3. **提高算法平均速度**：如果可以简单判定常见的部分情况，那么可以先单独判断（e.g.括号匹配可以先判断括号数量是不是偶数）
4. **勇敢使用STL**：比较两数大小的时候，STL的max(a, b)函数比三目运算符 (a>b)? a:b 要略好。因为如果a和b是表达式，那么后者进行了重复计算。
5. 递归！
6. **根据算法需求预处理数据结构，磨刀不误砍柴工**：当数据结构不好的时候可以先花费时间调整到自己喜欢的数据结构（比如310如果直接做拓扑排序复杂度很高，但先变成邻接矩阵就好了）



# 分类算法要点总结：

## AL. 二分法（已发知乎）

### 使用场景：

- 有序数组的查找（最常见的基础形式）
- **min max P 或 max min P 问题**（一般形式）
- （进阶形式）如果令mid VS max P，那么**二分搜索就可以把 max P 的最小化问题 转化为 若干个判定 min max P VS mid 大小关系的问题**（从计算变成比大小）如LC410

详见：[二分查找类问题的通用写法](E:\yqDATA\等待备份区\research material\我的文章\finished\二分查找类问题的通用写法.md)

```c++
//在[0, n - 1]上二分查找类问题的通用写法：

//公共部分初始化：
left = 0, right = n - 1;//注要点：此时[left, right]必须恰好是求解的范围。如果少了可能漏解，如果大了可能报错/出错。

//如果是 min mid 满足 P 的情况：
while (left <= right) {
    mid = left + (right - left)/2;
    //或者 mid = left + (right + 1 - left)/2;
    if (mid 满足 P)
        right = mid - 1;
    else
        left = mid + 1;
}
return left;

//如果是max mid 满足 P 的情况：
while (left <= right) {
    mid = left + (right - left)/2;
    //或者 mid = left + (right + 1 - left)/2;
    if (mid 满足 P)
        left = mid + 1;
    else
        right = mid - 1;
}
return right;

//公共部分结尾：
处理无解等边界情况 //无解的情况就是返回值不在区间[0, n - 1]中。对于返回 left 的情况就是 left = n; 对于返回 right 的情况就是 right = -1.
```

注意：把 `mid = (left + right)/2` 写作`mid = left + (right - left)/2` 是为了防止溢出

- 例题
  - LeetCode 704. 二分查找（基础）
  - LeetCode 275. H指数II （以上模板的使用范例）
  - LeetCode 4. 寻找两个正序数组的中位数（寻找二分法的场景）
  - LeetCode 410. 分割数组的最大值（**巧妙！对值域的二分法**）
  - LeetCode 668. 乘法表中第k小的数
  - LeetCode 300. 最长递增子序列（与其他方法配合）
  - LeetCode 154. 寻找旋转排序数组中的最小值II（这个系列里二分法的效果有什么区别？）
  - LeetCode 剑指offer 04. 二维数组中的查找（二分法可以降低复杂度吗？）
  - LeetCode 373. 查找和最小的K对数字（没想到吧……）


## AL. [滑动窗口](./滑动窗口.md)

### 使用场景：

- 某些连续子区间类问题，可将复杂度从 $O(n^2)$ 降至 $O(n)$ 。

  Q: 什么时候可以用滑动窗口降低复杂度？

  A: **如果能够保证l和r中某一个增加的时候另一个确定不需要回溯**（也只需要增加）就可以使用滑动窗口

```c++
//在[0, n)上滑动窗口
int l = 0;
int r = 0;
while(true){
	if (窗口扩张条件){
		if (r >= n) break;//左闭右开区间[left, right）比较方便
		操作
        ++r;//窗口扩张
    }
    else{
        if (l >= n) break;//左闭右开区间[left, right）比较方便
        操作
        ++l;//窗口收缩
    }
}
```

- 例题：
  - LeetCode 209. 长度最小的子数组
  - LeetCode 713. 乘积小于K的子数组
  - LeetCode 76. 最小覆盖子串

## DS. 链表

### **技巧：**

- **双指针法**

  - LeetCode 19. 删除倒数第N个结点
  - LeetCode 21.合并两个有序链表
  - LeetCode 160.判断两个链表是否相交——两种方法实现链表对齐

  - **快慢指针**
    - LeetCode 141&142. 判断列表是否有环/找到环的入口
    - LeetCode 206. 反转链表 -> LeetCode 234. 回文链表
    - LeetCode 876. 寻找列表中间结点

- **头结点trick**：对于第一个结点难以纳入循环中的问题，可以在一开始加入一个头结点指向第一个元素，算法结束后再删掉

- **二分法**：二分法理论上适用于任何我的知乎文章的情况，只不过条件P可以更复杂，专门写个函数或者查找判定复杂度O(n)那种——但即便如此总共的复杂度也才O(nlogn)，刚好可以前面配个排序if二分法需要

  

- 例题：
  - LeetCode 142. 环形链表II（经典问题）
  - LeetCode 剑指offer 35. 复杂链表的复制（很巧妙）



## AL. [动态规划](./动态规划.md)

### 使用场景：

- **最优子结构性质**：问题的最优解由子问题的最优解得到，且子问题可以独立求解。
- **重叠子问题**：子问题是重叠可复用的（与分治法的区别所在，分治法的子问题是相互独立的）

**从数学视角看实际上就是把问题建模成 $f_n$ 并找到递推公式。**

### 分析动态规划问题的方法：

1. **分析问题结构**：把问题建模成计算 $f_n$ 

2. **建立递推关系**：找到递推公式，关键是**寻找最优子结构性质**，即子问题的解如何推出父问题的解

3. 递推计算最优值：先确定**初值**和**计算次序**，再利用初值和状态转移方程递推计算

4. 追踪最优方案：如果不仅要求解，还需要知道解是如何得到时才需要这一步

   

- 注意事项：

  - 动态规划有实现两种方式：自顶向下（带备忘录的递归）和自底向上。一般为了减少递归调用的额外开销，多使用自底向上的实现方式。

  - 根据状态转移方程的形式，动态规划有时可以进行**状态压缩**以减小空间复杂度

- 例题：

  - LeetCode 53. 最大子数组和（经典例题：简单难度）

  - LeetCode 1143. 最长公共子序列（经典例题：中等难度）
  - LeetCode 72. 编辑距离（经典例题：进阶难度）
  - 01背包问题（经典例题）
  - 钢条切割（区间动态规划）
  - 矩阵链乘法（区间动态规划）
  - LeetCode 300. 最长递增子序列
  - LeetCode 264. 丑数II
  - LeetCode 410. 分割数组的最大值



## AL. 贪心法 :TODO



## AL. 回溯法/排列组合 :TODO



## DS. 栈和队列 :TODO

### 技巧：

- **单调栈**

- 栈的思维：不仅仅是FILO，而是一种递归的思维。Astart -- Bstart -- Bend -- Aend 代表的是完成A这个任务期间需要完成B这个子任务的模式。有这种特点的情景都可以考虑栈。

  并且如果对入栈(+1)出栈(-1)进行计数，n就代表了最多同时发生的个数（如开会问题需要多少个会议室），<0代表了不合法（如括号匹配）
  
  

## DS. 哈希 : TODO

Hash



## BFS :TODO

计算连通分量

拓扑排序

最短路径（最短路径其实可以不记录到每一个点的路径，而是只用一个step，在每次换层的时候读出此时queue中点的数量，执行等量的操作后再step++，就是一层一层来算step）

```c++
//BFS遍历无向图
```

双向BFS：LC127单词接龙（不改变复杂度下界的优化方法）

【虚拟节点】技巧减少构造邻接表的开销

# 经典数据结构总结：

## DS. 并查集

**效率：**

- 只使用标准的按秩合并后，最坏时间复杂度为：**查找O(logn)**、合并O(logn)——因为要先查找根。
- 只使用路径压缩，单次操作的平均时间复杂度为 O($\alpha(n)$) 近似为 $O(1)$.

**用处：** **计算**并**维护**连通分量

在只计算一次连通分量时（如LC200岛屿数量），虽然BFS、DFS、并查集具有相同的复杂度 $O(E + V)$，但由于并查集不涉及栈和队列的操作，在常数级别上更快。（不过DFS可以递归实现，写起来最快，作为算法题能用优先用）

如在Kruskal算法中，由于需要频繁确定两条边是否属于同一连通分量，使用并查集可以优化复杂度。

但并查集最重要的意义在于如果**面对需要维护连通分量，甚至修改图结构改变连通分量的情况**，并查集可以 $O(1)$ 高效实现多次查询。这时一次性遍历并输出连通分量数的DFS和BFS就没有办法解决了（每次查询只能再 $O(n)$ 遍历一次）

```c++
//实现了路径压缩的并查集(parent, find, un)：
vector<int> parent(n);//parent[i]代表i的根节点
for(int i = 0; i < n; ++i){
    parent[i] = i;//初始化并查集，每个结点的根节点都是自己
}

int find(int i, vector<int>& parent){//查找
    if (i == parent[i])	return i;//当根节点是自己的时候返回
    parent[i] = find(parent[i], parent);//否则返回自己父亲的根节点（路径压缩修改父亲为根节点）
    return parent[i];
}

void un(int i, int j, vector<int>& parent){//合并
    int fi = find(i, parent);//找到i和j的根
    int fj = find(j, parent);
    parent[fj] = fi;//合并
    return;
}


//实际调用：
for(i 和 j in 某个范围){
    un(i, j, parent);//把(i, j)相连的都合并
}

//输出结果：
int count = 0;
for(int i = 0; i < n; ++i){
    if (parent[i] == i){//连通分量的数量 == 根节点的数量
        ++count;
    }
}
cout << count << endl;
```

- 注意事项：
  - 对于做算法题而言，只使用路径压缩就够了，而且好写。
  - 按秩合并的两种方式：标准方案按照树高（优化最坏时间复杂度），简易方案按照元素个数（优化平均时间复杂度）

- 例题：
  - LeetCode 200. 岛屿数量（连通分量基础问题）
  - LeetCode 684. 冗余连接
  - LeetCode 990.  等式方程的可满足性（关键是抽象出问题含义）
  - LeetCode 1202.  交换字符串中的元素
  - LeetCode 947. 移除最多的同行或同列石头（精彩！不同的**对象**和**合并方式**带来不同的复杂度）
  - LeetCode 765. 情侣牵手（关键是要想清楚交换座位的原则）



## DS. 队列：堆/优先队列...

### 类型：

- （双端）队列：FIFO不解释
- **优先队列/堆**：
- **单调队列**：类似单调栈
  - LeetCode 239. 滑动窗口最大值

### [堆/优先队列](./堆(优先队列).md)：

- 例题：

  - LeetCode 215. 数组中的第K个最大元素（堆的经典基础题）

  - LeetCode 692. 前k个高频单词（关键是自定义比较规则）

  - LeetCode 295. 数据流的中位数（两个堆的配合）
  - LeetCode 373. 查找和最小的K对数字



# 其他零散专项TOPIC：

## Topic：二进制枚举

**Def.** 用一个（二进制）整数表示一个集合：$(010011)_2$表示全集{0,1,2,3,4,5}的子集{0,1,4}

Notice：用**右起**第k位=1表示k在集合中，这样比较方便运算

**常见操作：**（^代表异或）

- 将 k 加入集合 S：S ^ (1 << k) 或 S | (1 << k)

- 在集合 S 中去掉 k：S ^ (1 << k)

- 判断 k 是否在 S 中：(S >> k) & 1

- 判断两个集合是否有交集：$S_1$​&$S_2$

- n & (n - 1) = 把n的二进制中最低位的1变成0后的结果

  

- 例题：
  - LeetCode 剑指offer 15. 二进制中1的个数（n & (n - 1)大显神威）

## Topic：（矩阵）快速幂

**快速幂**算法即用 $O(\log n)$ 的时间快速计算 $a^n$​, 其思路类似于二分法，即

- $n = 2k$ 时，$a^n = (a^k)^2$​​
- $n = 2k+1$ 时，$a^n = (a^k)^2 *a $

```c++
//递归写法：
long long binpow(long long a, long long b) {
  if (b == 0) return 1;//a^0 = 1
  long long res = binpow(a, b / 2);//计算 a^(b/2)
  if (b % 2)
    return res * res * a;//即b = 2k+1时，a^b = a^k * a^k *a 
  else
    return res * res;//即b = 2k时，a^b = a^k * a^k
}
```

```c++
//非递归写法（一般更快）：
long long binpow(long long a, long long b) {
  long long res = 1;////a^0 = 1
  while (b > 0) {
    if (b & 1) res = res * a;//即b = 2k+1时，a^b = a^2k *a 
    a = a * a;
    b >>= 1;//即a^2k = (a^2)^k
  }
  return res;
}
```

**矩阵快速幂**则是当 $a$ 为矩阵 $M$ 时，利用同样的思路快速计算 $M^n$​，使用题型为有明确常系数线性递推公式的数列/动态规划问题。

- 例题：
  - LeetCode 1137. 第 N 个泰波那契数（矩阵快速幂）

## Topic：[数论问题与基础算法](./数论问题与基础算法(未必最优).md)

- 判断一个数 $n$ 是否为素数 $\implies$ 找到 $n$ 的一个质因数$\implies$对 $n$ **分解质因数**$\implies$计算$\varphi(n)$：      $O(\sqrt{n})$

- **线性筛法**：计算 $\{ f(1), \ldots, f(n) \}$ 其中 $f(·)$ 是一个**积性函数**。

  - 如果 $f(p^k)$ 可以用 $O(1)$ 的时间计算，那么线性筛法计算 $\{ f(1), \ldots, f(n) \}$ 的复杂度是 $O(n)$ 。

    > 只需要在进行素数筛的过程中，筛到 $n$ 的时候同时计算以下两个数值：
    > $$
    > g(n) = p_1^{\alpha_1}, \quad 其中 p_1 是 n 最小的质因数, \alpha_1是p_1在n中的次数
    > $$
    >
    > $$
    > f(n) = f(p^a) * f(\frac{n}{p^a})
    > $$
    >
    > 其中 $g(n)$ 用公式 $g_{n}= \begin{cases}g_{x} \cdot p & x \bmod p=0 \\ p & \text { otherwise }\end{cases}$  更新，其中 $p$ 是筛 $n$ 时的那个素数，即可。

  - 经典示例：计算前 $n$ 个数中的质数、计算前 $n$ 个欧拉函数 $\{ \varphi(1), \ldots, \varphi(n) \}$。

    ```c++
    //线性筛法计算前n个数中的质数：
    vector<int> primes;//保存前n个质数
    vector<int> visited(n + 1, 0);//记录[2, ..., n]是否被筛过
    for(int i = 2; i <= n; ++i){
        if (! visited[i]){
            primes.push_back(i);//没有被visit过说明这是个质数
        }
        for(int j = 0; j < primes.size() && j * primes[i] <= n; ++j){
            visited[j * primes[i]] = 1;//筛掉 p*x (x>=p)
            //注意这样筛是足够的，因为任何一个数 n = p*x 一定有不超过\sqrt{n}的质因子
            
            if (j % primes[i] == 0)	break;//如果j是某个质数pi的倍数，那么不需要再用j来筛了
            //正确性是因为n = j * px = (j/pi)*px * pi可以交给更小的质因子pi而不是px来筛
            //因此线性筛的性质是：数 n 必定是被 n的最小的质因子p 筛掉的
        }
        return primes;
    }
    ```

- **扩展欧几里得算法**：裴蜀定理、乘法逆元、解线性同余方程

  - 若$(a, b) = 1$，计算 $ax+by=1$ 的一组解的方法为，利用等式：
    $$
    a * x + b * y = 
    b * (y + [\frac ab]x) + (a - [\frac ab]) * x
    $$

- **离散对数问题**：BSGS算法

  - **BSGS算法**可以在 $O(\sqrt{n})$ 的时间求解 $a^x \equiv b \quad(mod \ n)$，其中$(a, n) = 1$。
  
- 例题：

  - LeetCode 204. 计数质数




# C++ 语法

- new 与 delete

```c++
//使用new是为了让变量在离开局部作用域（函数或{}）后不被销毁
L = new LinkList();// 分配空间，执行LinkList的构造函数，把指针返回给L
delete L;//这是一个命令，与new配对。执行LinkList的析构函数，释放空间
```

- for循环

```c++
for (auto items: my_array)//不解释。auto是自动获取变量类型的意思
```

- 向量 vector
  - 相当于动态顺序表

```c++
vector<int> vec;//创建一个向量vec，其元素是int类型的（似乎不需要再new了）
vec = vector(……);//初始化向量vec
vec.empty();//判断向量是否为空
vec.size();//求向量中的元素个数
vec.clear();//清空向量

vec.at(i);//返回 pos=i 的元素, =会提示错误的v[i]
vec.front();//返回首元素的 "引用"
vec.back();//返回尾元素的 "引用"
vec.begin();//返回指向首元素的 "迭代器"
vec.end();//返回指向尾元素的下一个位置的 "迭代器"

vec.insert(vec.begin() + i, val);//在iter指向的元素前加元素val. 如左即是在vec[i]处插入val
vec.push_back();//在向量尾部增加元素，返回void
vec.pop_back();//删除尾元素，返回void
vec.erase(vec.begin() + i);//删除 iterator 指向的元素, 删除vec[i]
vec.erase(arr.begin() + k, arr.end());//具体例子，删除vec[k ... end)
vec.assign(……);//修改元素值
```

- 栈 stack 和 (优先)队列 queue 

```c++
stack<char> stk;//创建一个栈stk，其元素是char类型的（似乎不需要再new了）
queue<char> Q;//创建一个栈stk，其元素是char类型的（似乎不需要再new了）
priority_queue<int> pq;
priority_queue<int, vector<int>, less<int>> pq; // 默认情况, 大根堆
priority_queue<int, vector<int>, greater<int>> pq; // 小根堆。表示元素为int, 存储容器为vector<int>, 比较函数为greater<int>(即更大的元素沉底，即小根堆)
priority_queue<Node, vector<Node>, comp> pq; // 自定义优先级
struct comp{
    bool operator()(Node& a, Node& b){
        return a > b
    }
};

Q/pq/stk.empty();
Q/pq/stk.size();
//注意栈和队列都没有.clear()函数

pq/stk.top();//返回优先队列顶/栈顶元素的 "引用"
Q.front();//返回队首元素的 "引用"
Q.back();//返回队尾元素的 "引用"

Q/pq/stk.pop();//弹出栈顶或队首元素，返回void
Q/pq/stk.push(e);//入栈或入队
```

- 字符串 string

```c++
string s;
s.size();
s.assign(……);//字符串赋值

s1 + s2;//加法就是拼接
s.append(s2);//字符串拼接，s2不能是单个字符
s.push_back('c');//只用于末尾添加字符

s1 < s2;//比较按照字典序
s.compare(……);

s.find(s1, pos);//查找s[pos, end)中第一次出现子串s1的位置，并返回
int s.find(s1);//查找s中第一次出现子串s1的位置，并返回
//注意string.find()和其他find不同，不返回迭代器
    
s.substr(pos, n);//截取s中从pos开始（包括pos）的n个字符的子串，并返回
s.substr(int pos);//截取s中从从pos开始（包括pos）到末尾的所有字符的子串，并返回
s.erase(pos, n);//删除s中从pos开始（包括pos）的n个字符的子串
s.erase(int pos);//删除s中从从pos开始（包括pos）到末尾的所有字符的子串

to_string(val);//把数转为字符 e.g. 12 -> "12"
#include <cstdlib>
stoi(str);//把字符转为int
stoll(str);//把字符转为long long
strtd(str);//把字符转为double
```

|                              | append() | +=     | push_back |
| ---------------------------- | -------- | ------ | --------- |
| 全字符串（string）           | √        | √      | ×         |
| 部分字符串（substring）      | √        | ×      | ×         |
| 字符数组（char array）       | √        | √      | ×         |
| 单个字符（char）             | ×        | √      | √         |
| 迭代器范围（iterator range） | √        | ×      | ×         |
| 返回值（return value）       | \*this   | \*this | none      |
| cstring（char*）             | √        | √      | ×         |

- 哈希表、map

```c++
#include <unordered_map>
#include <map>//以下函数均对map和unordered_map通用
unordered_map<int, int> m = {{1,1}, {2,2}};
umap.size();
umap.empty();
umap.clear();

umap[key] //返回value，没有则会插入这个键值对
umap.find(key); //查找key, 返回iterator, 不存在则返回.end()
umap.at(key); //查找key，返回value，没有会提示

umap.insert({key, value}); //插入键值对
umap.erase(key);//删除key, 如果key不存在则无事发生

if (umap.find(key) == umap.end()) //判断key不在umap的方法1
if (umap.count(key) > 0) //判断key在umap的方法2

for (auto items : mp){//遍历
    cout << items.first;//访问key
    cout << items.second;//访问value
}
```

- set、unordered_set

```c++
#include <set>
#include <unordered_set>//这两类的函数也一样
set<int> s;
s.size();
s.empty();
s.clear();
s.count(x);//查询x的个数 = 查询是否有x

s.begin();
s.end();
s.find(x) != s.end(); // 表示存在x

s.insert(x);
s.erase(x);//删除x, 未找到不会报错

for (auto iter = a.begin(); iter != a.end(); ++iter){//注意，set不支持iter < a.end()的写法
      cout << *iter << endl;
}//遍历set
```

- <algorithm>：排序、查找、反转、交换……

```c++
#include <algorithm>
sort(v.begin(), v.end());//升序排序
sort(v.begin(), v.end(), comp);//比如，这是给一个向量v从头到尾排序，按照comp的原则。
//如果希望改成从大到小，可以定义comp如下，对于结构体同理可以定义自己想要的排序标准
static bool comp(const int &a,const int &b)
{
    return a>b;
}

find(v.begin(), v.end(), x);//在[begin, end)寻找第一个x，返回指向x的 "迭代器"
bool ret = binary_search(v.begin(), v.end(), val);//二分查找val，有则返回 true
int pos = lower_bound(v.begin(), v.end(), val) - v.begin();//二分查找 >= val 的第一个 pos
int pos = upper_bound(v.begin(), v.end(), val) - v.begin();//二分查找 > val 的第一个 pos

swap(a, b);
reverse(v.begin(), v.end());//用于反转在[first,last)范围内的顺序，比如这是完全翻转v
replace(v.begin(), v.end(), old_val, new_val);//把范围内的old_val替换为new_val
unique(v.begin(), v.end());//用于去重相邻元素，适合配合sort使用
```

- 整数运算与cmath

```c++
a / b;//向0取整。性质：(-a)/b == a/(-b) == -(a/b)
a % b;//符号与a相同。性质：a = b*(a/b) + a%b / 被除数 = 除数*商 + 余数
(int)x;//向0取整，并把数据类型变为int

#include <cmath>
floor(x);//向负无穷大取整
ceil(x);//向正无穷大取整
trunc(x);//向0取整

fabs(x);//取绝对值
pow(base,expo);//base^expo
log(x);//取自然对数
log10(X);//常用对数
log2(X);

#include <cstdlib>
int num = rand() % 100;//num为0-99范围内的随机数

#include<climits>
INT_MAX = 2147483647 //9位数 = 2^31 - 1 (因为4byte = 32bit)
INT_MIN = -2147483648
LLONG_MAX //9开头的19位数 = 2^63 - 1 (因为8byte = 64bit)
```

# 一些题解

## LeetCode 1434 每个人戴不同帽子的方案数

思考部分：

- 首先通过场外 $n\leq 10$ 推测出时间复杂度可能是指数级别。
- 利用二进制枚举的知识，可以用二进制数表示帽子和人的集合
- 分析问题：如果使用动态规划，每一步做决策时，必须知道的两个信息是此刻被分走的帽子和此刻有帽子的人，二者缺一不可（但帽子和人的对应关系并不重要）。因此使用动态规划，状态形如 $dp[2^{40}][2^{10}]$
- 这个状态的空间太大，考虑压缩：我们始终可以按照一个顺序（帽子或人），如果按照帽子一个个分配，那么 $2^{40}\to 40$, 同理按照人 $2^{10}\to 10$​. 显然应该按照多的来考虑。因此得到状态为 $dp[40][2^{10}]$​, 且分配过程应该按照帽子编号依次完成。

代码部分：

- 准备工作：由于按照帽子编号顺序分配，需要把题目的人$\to$帽子转写为帽子$\to$​人
- 收尾工作：状态压缩
  - 由于 $dp[i][xxx]$​​​只与 $dp[i-1][yyy]$​​ 有关，所以可以压缩空间为$2*2^{10}$
  - 由于 $dp[i][j]$只与 $dp[i-1][k]$ 有关，其中 $k\leq j$，所以可以直接压缩空间为1维 $dp[2^{10}]$​——所以

