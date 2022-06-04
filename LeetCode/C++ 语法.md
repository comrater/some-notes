1. **边界情况**：永远考虑一下输入极端的情况。（如输入空表）
2. **经验判断算法复杂度限制**：一般数据规模代入时间复杂度不要超过10^7  10^8 10^9. 经典case: $n\leq8 \rightarrow O(n!)$​, $n\leq20 \rightarrow O(2^n)$​, $n\leq500 \rightarrow O(n^3)$​, $n\leq1000 \rightarrow O(n^2)$
3. 优化算法的三种常见思路
   1. 序列变形：计算前k项和/相邻两项差/前k项的最大/最小值；
   2. 反向映射：按a的顺序保存对应的b转写为按b的顺序保存对应的a；
   3. 记录数组元素VS记录数组下标（某种程度上是2的特例）
   4. Hash



# C++ 语法

- new 与 delete

```c++
L = new LinkList();// 分配空间，执行LinkList的构造函数，把指针返回给L
delete L;//这是一个命令，与new配对。执行LinkList的析构函数，释放空间
```

- 向量 vector

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
}

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

- 整数运算与数学函数

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
LLONG_MAX//9开头的19位数 = 2^63 - 1 (因为8byte = 64bit)
```
