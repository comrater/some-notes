# pandas的常见操作

## 创建与访问: DataFrame与Series

- 用字典（按列）创建DataFrame

```python
## 创建字典
data = {'value1': [1, 2, 3, 4, 5, 6],
        'value2': [10, 20, 30, 40, 50, 60],
        'type':[1.0, 2.0, 3.0, 1, 2, 3]}
df = pd.DataFrame(data)  # 把dict转为DataFrame

print(df)
"""Output:    
value1  value2  type
0       1      10   1.0
1       2      20   2.0
2       3      30   3.0
3       4      40   1.0
4       5      50   2.0
5       6      60   3.0
"""
print(type(df))
"""Output: 
<class 'pandas.core.frame.DataFrame'>
"""
print(df.shape)
"""Output: 
(6, 3)
"""
```

- 访问

```python
types = df['type']
values = df[['value2','value1']]  # 注意要访问多列的时候需要用list
# 按条件访问
df[df[label].isin(ls)]  # 访问label在ls里的行
```

- DataFrame与Series的关系

```python
print(type(types))  # Dataframe的一行，是一个Series
"""Output: 
<class 'pandas.core.series.Series'>
"""
print(types)  # Series只有index和value
"""Output: 
0    1.0
1    2.0
2    3.0
3    1.0
4    2.0
5    3.0
"""
print(types.shape)
"""Output: 
(6,)
"""

print(type(values))  # Dataframe的多行，是一个Dataframe
print(values)  # Series和Dataframe的区别在于，Dataframe有column
print(values.shape)
```



## 格式互转

- list、array、tensor 转 Series/DataFrame

```python
list0 = [1, 2, 3]  # 也可以是多维的list
array = np.array(list0)
tensor = torch.tensor(list0)

# list 和 array 和 tensor (dim=1) 到 series
series = pd.Series(list0)
series = pd.Series(array)
series = pd.Series(tensor)

# TO dataframe
    # list 和 array 到 dataframe
df = pd.DataFrame(list0)  # col自动生成为编号
df = pd.DataFrame(array)
    # tensor 到 dataframe: tensor -> array -> dataframe
df = pd.DataFrame(np.array(tensor))
    # series 到 dataframe
df = series.to_frame()
```

- Series/DataFrame 转 list、array、tensor

```python
series = df['colnames_0']

# series 到 list
list0 = series.to_list()

# series/dataframe 到 array
array = df.values
array = series.values

# series/dataframe 到 tensor: series/dataframe -> array -> tensor
tensor = torch.from_numpy(df.values)
tensor = torch.from_numpy(series.values)
```



## 常见属性和操作

### 读文件

- .npy文件：`pd.DataFrame(np.load('./filename.npy'))`

  ```python
  df = pd.DataFrame(np.load('./NYC.npy'), columns='user_id', 'poi_id', 'time'])
  #读取当前文件夹里的NYC.npy文件到DataFrame df,并将df.columns指定为['user_id', 'poi_id', 'time']
  ```

- .csv文件

### 修改列名

对于DataFrame `df`，`df.columns`是一个 list。直接修改`df.columns`就可以修改`df`的列名。如

```python
df.columns = ["colnames_" + str(i) for i in range(3)]
#把df的三列名称改为: [colnames_0  colnames_1  colnames_2]
```

### 连接表 df.join()  -> DataFrame

```python
df = df.join(another_df.set_index('key'), on='key')
```

### 查找特定元素

- 查找给定的DataFrame `df`里是否有NaN：

```python
#含有Nan等价于:
df.isna().any().any() == True # 对于DataFrame
se.isna().any() == True # 对于Series
#解释:
#df.isna() 的效果是返回一个和df同样形状的dtype = bool的DataFrame.
#			规则是把None和Nan映射为True,其他映射为False.
#df.any() 的效果是判断df沿给定轴是否至少有一个元素为True(或等效).
#			用两次是因为.any()之后, DataFrame -> Series, Series -> bool.
```



### 删除

- **删除缺失值** **df.dropna()** -> DataFrame | None

```python
df.dropna(inplace=True)
#相当于 
df = df.dropna()
#相当于 
df = df.dropna(axis=0, how='any', inplace=False)
#axis=0表示删除对应行, =1删除对应列
#how='any'表示有NA value就删除, ='all'表示全为NA value才删除
#inplace=False表示返回新DataFrame, =True表示原地修改, 无返回值
```

- **去重 se.unique()** -> array

  得到对value去重后的se，返回的类型是array

- **去除DataFrame中重复项 df.drop_duplicates()** -> DataFrame | None

```python
df.drop_duplicates(inplace=True)
#相当于 
df = df.drop_duplicates()
#相当于 
df = df.drop_duplicates(subset=None, keep='first', inplace=False, ignore_index=False)
#subset表示寻找重复项的范围, 默认为整个DataFrame
#keep表示是保留首次出现的重复项、最后一次出现的重复项、全删
#inplace=False表示返回新DataFrame, =True表示原地修改, 无返回值
#ignore_index = True 表示排序之后丢弃过去的index重新编index
```

- **删除特定行或列 df.drop()** -> DataFrame | None

  - 删除满足条件的x所在的行

    ` df = df.drop(df[<some boolean condition>].index)`

```python
#删除dataframe中满足 x<0.01 的行
df_clear = df.drop(df[df['x']<0.01].index)
#删除dataframe中满足 x<0.01或x>10的行
df_clear = df.drop(df[(df['x']<0.01) | (df['x']>10)].index)
```

### 排序 df.sort_values() -> DataFrame

```python
df = df.sort_values(by = ['user_id', 'time'], ignore_index = True)  # 排序并重新编号
# by 后面跟排序的规则, 默认为升序, 想改也有参数
#ignore_index = True 表示排序之后丢弃过去的index重新编index
```

### 提取编号 df.reset_index() -> DataFrame

把 df 此时的 index 提取出成一个新列（第一列），然后重排 index

### 计数 se.value_counts() -> Series

对 se 进行计数，返回这样的一个series：index是se中的所有value，对应的value是se中这个value的出现次数。

默认按value的降序排列（也就是次数最多的排首位）（有参数可以调），默认丢弃NaN（有参数可以调）

