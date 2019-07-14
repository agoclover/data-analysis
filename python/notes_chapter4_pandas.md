# Pandas

## 4.5 Pandas 数据结构简介

### 4.5.1 Series对象

主数组+Index（数组）

#### 1 声明Series对象

```python
# pd.Series(value, index)
# 不指定标签
>>>s = pd.Series([1, 2, 3, 4, 5])
>>>s
0    1
1    2
2    3
3    4
4    5
dtype: int64
>>>s.values
array([1, 2, 3, 4, 5])
>>>s.index
RangeIndex(start=0, stop=5, step=1)
# 指定标签
>>>s2 = pd.Series([1.1, 1.2, 3], index=['a', 'b', 'c'])
>>>s2
a    1.1
b    1.2
c    3.0
dtype: float64
>>>s2.values
array([1.1, 1.2, 3. ])
>>>s2.index
Index(['a', 'b', 'c'], dtype='object')
```

#### 2 选择内部元素

```python
# 索引
>>>s2[0]
1.1
>>>s2['b']
1.2
# 切片
>>>s2[:]
a    1.1
b    1.2
c    3.0
dtype: float64
>>>s2[:2]
a    1.1
b    1.2
dtype: float64
>>>s2[::2]
a    1.1
c    3.0
dtype: float64
```

#### 3 为元素赋值

```python
# 利用标签为元素赋值
>>>s2[0] = 0
>>>s2
a    0.0
b    1.2
c    3.0
dtype: float64
>>>s2['c'] = 0
>>>s2
a    0.0
b    1.2
c    0.0
dtype: float64
```

#### 4 用Numpy对象或现有的Series对象定义新Series对象

```python
# 通过NumPy数组定义新的Series对象，视图
>>>arr = np.array([1, 2, 3, 4, 5])
>>>s3 = pd.Series(arr)
>>>s3
0    1
1    2
2    3
3    4
4    5
dtype: int64
# 通过其他Series对象构造Series对象，视图
>>>s4 = pd.Series(s2)
>>>s4
a    0.0
b    1.2
c    0.0
dtype: float64
# 以上两种方式均为视图，非副本
>>>arr[2] = 0
>>>s3
0    1
1    2
2    0
3    4
4    5
dtype: int64
>>>s2['c'] = 100
>>>s4
a      0.0
b      1.2
c    100.0
dtype: float64
```

#### 5 筛选元素

pandas库的开发是以NumPy库为基础的，因此就数据结构而言，NumPy数组的多种操作方法得以扩展到Series对象中。

```python
>>>s
0    1
1    2
2    3
3    4
4    5
dtype: int64
>>>s[s > 3]
3    4
4    5
dtype: int64
```

#### 6 Series对象运算和数学函数

```python
# numpy数组运算符(+,-,*,/)或其他数学函数，同样适用于Series对象。
>>>s
0    1
1    2
2    3
3    4
4   -1
dtype: int64
>>>s / 2
0    0.5
1    1.0
2    1.5
3    2.0
4   -0.5
dtype: float64
# numpy库的数学函数，必须指定出处np，并把Series实例作为参数传入。
>>>s[4] = 10
>>>s
0     1
1     2
2     3
3     4
4    10
dtype: int64
>>>np.log(s)
0    0.000000
1    0.693147
2    1.098612
3    1.386294
4    2.302585
dtype: float64
```

#### 7 Series对象的组成元素

```python
# 统计元素重复出现的次数或判断一个元素是否在Series中
>>>serd = pd.Series([1, 0, 2, 1, 2, 3], index=['white', 'white', 'blue', 'green', 'green', 'yellow'])
>>>serd
white     1
white     0
blue      2
green     1
green     2
yellow    3
dtype: int64
# unique()函数返回一个新的去重后的元素
>>>serd.unique()
array([1, 0, 2, 3])
# value_counts函数返回一个新的计算元素次数
>>>serd.value_counts()
2    2
1    2
3    1
0    1
dtype: int64
# isin()函数判断所属关系
>>>serd.isin([0,3])
white     False
white      True
blue      False
green     False
green     False
yellow     True
dtype: bool
# 布尔值的可作为筛选条件
>>>serd[serd.isin([0, 3])]
white     0
yellow    3
dtype: int64
```

#### 8 NaN，nan，NAN

nan来自于numpy中numpy.nan，字面意思应该是Not a Number。在不同代码中有nan，有NaN，有NAN，但其实他们都一样的。

**nan与None的区别**

```python
>>>np.nan is np.NaN is np.NAN
True
# nan判断
# None等于None; 但是对于nan，nan并不等于nanI
>>>None == None
True
>>>np.nan == np.nan
False
# 判断时可以用
>>>np.isnan(np.nan)
>>>True
# nan为float类型
>>>type(np.nan)
float
>>>type(None)
NoneType
# 遇到nan时的处理方法
# 加法，直接忽略掉
>>>np.nansum([11,np.nan,123])
134.0
# 统计array中nan个数（利用nan !=nan 为True）
>>>a = np.array([1,2,3,4,np.nan,5,np.nan,np.nan])
>>>a
array([  1.,   2.,   3.,   4.,  nan,   5.,  nan,  nan])
>>>np.count_nonzero(a != a)
3
```

> 以上关于nan和None的区别部分内容了来自于[CSDN](https://blog.csdn.net/qq_33819591/article/details/77993590%20)。

**nan的判断**

```python
>>>s5 = pd.Series([5, -3, np.NaN, 14])
>>>s5
0     5.0
1    -3.0
2     NaN
3    14.0
dtype: float64
# Series.isnull()函数返回布尔值Series用于判断是nan
>>>s5.isnull()
0    False
1    False
2     True
3    False
dtype: bool
# Series.notnull()函数返回布尔值Series用于判断不是nan
>>>s5.notnull()
0     True
1     True
2    False
3     True
dtype: bool
# 其返回布尔值列表可以用于筛选或索引
>>>s5[s5.isnull()]
2   NaN
dtype: float64
>>>s5[s5.notnull()]
0     5.0
1    -3.0
3    14.0
dtype: float64
```

#### 9 Series用作字典

```python
>>>mydict = {'red':2000, 'blue': 1000, 'yellow':500, 'orange':1000}
# Series可由字典来定义, 不是视图。
>>>myseries = pd.Series(mydict)
>>>myseries
red       2000
blue      1000
yellow     500
orange    1000
dtype: int64
# 索引可另设，不足则补nan
>>>color = ['red', 'yellow', 'orange', 'blue', 'green']
>>>myseries = pd.Series(mydict, index=color)
>>>myseries
red       2000.0
yellow     500.0
orange    1000.0
blue      1000.0
green        NaN
dtype: float64
```

#### 10 Series对象之间的运算

```python
# 运算按照索引对齐不一致的数据
# 只有索引都存在才显示和，否则显示nan
>>>myseries
red       2000.0
yellow     500.0
orange    1000.0
blue      1000.0
green        NaN
dtype: float64
>>>myseries2
red        400
yellow    1000
black      700
dtype: int64
>>>myseries + myseries2
black        NaN
blue         NaN
green        NaN
orange       NaN
red       2400.0
yellow    1500.0
dtype: float64
```

### 4.5.2 DataFrame对象

#### 1 定义DataFrame对象

```python
# 利用字典来定义
# 略
# 只用np.arange(16).reshape((4, 4))和index、columns来定义
>>>frame3 = pd.DataFrame(np.arange(16).reshape((4, 4)), index=['red', 'yellow', 'blue', 'green'], colums= ['ball', 'pen', 'pencil', 'paper'])
>>>frame3
        ball  pen  pencil  paper
red        0    1       2      3
yellow     4    5       6      7
blue       8    9      10     11
green     12   13      14     15
```

#### 2 选取元素

```python
>>>frame3
        ball  pen  pencil  paper
red        0    1       2      3
yellow     4    5       6      7
blue       8    9      10     11
green     12   13      14     15
# 返回index
>>>frame3.index
Index(['red', 'yellow', 'blue', 'green'], dtype='object')
# 返回columns
frame3.columns
Index(['ball', 'pen', 'pencil', 'paper'], dtype='object')
# 返回值的ndarray
>>>frame3.values
array([[ 0,  1,  2,  3],
       [ 4,  5,  6,  7],
       [ 8,  9, 10, 11],
       [12, 13, 14, 15]])
# 索引与查找
# 返回某行
>>>frame3[0:2]
        ball  pen  pencil  paper
red        0    1       2      3
yellow     4    5       6      7
# 返回某标签对应的行或列
>>>frame3['paper']
red        3
yellow     7
blue      11
green     15
Name: paper, dtype: int64
# 返回标签行或列内的元素
>>>frame3['paper'][2]
11
```

通过`.loc`和`.iloc`来进行索引行。

```python
>>>frame3
item    ball  pen  pencil  paper  new
id                                   
red        0    1       2      3  NaN
yellow     4    5       6      7  1.0
blue       8    9      10     11  0.0
green     12   13      14     15  2.0
# DataFrame.iloc[]，完全基于行号的索引器，所谓行号就是第0、1、2行
>>>frame3.iloc[3]
item
ball      12.0
pen       13.0
pencil    14.0
paper     15.0
new        2.0
Name: green, dtype: float64
# DataFrame.loc[]，完全基于标签位置的索引器，所谓标签位置就是行标签定义的'red','yellow'。
>>>frame3.loc['red']
item
ball      0.0
pen       1.0
pencil    2.0
paper     3.0
new       NaN
Name: red, dtype: float64
# DataFrame.ix[]函数已过期，不建议使用
```

> 以上关于`.loc`和`.iloc`来自于[CSDN](https://blog.csdn.net/u014525494/article/details/80156497)。

#### 3 赋值

```python
# 为index和columns赋值
>>>frame3.index.name = 'id'
>>>frame3.columns.name = 'item'
>>>frame3
item    ball  pen  pencil  paper
id                              
red        0    1       2      3
yellow     4    5       6      7
blue       8    9      10     11
green     12   13      14     15
# 新增1列
>>>frame3['new'] = 12
>>>frame3
item    ball  pen  pencil  paper  new
id                                   
red        0    1       2      3   12
yellow     4    5       6      7   12
blue       8    9      10     11   12
green     12   13      14     15   12
# 更改new列值，若使用Series标签需要与行匹配，否则输入nan
>>>ser = pd.Series(np.arange(3), index=['blue', 'yellow', 'green'])
>>>ser
blue      0
yellow    1
green     2
dtype: int64
>>>frame3['new'] = ser
>>>frame3
item    ball  pen  pencil  paper  new
id                                   
red        0    1       2      3  NaN
yellow     4    5       6      7  1.0
blue       8    9      10     11  0.0
green     12   13      14     15  2.0
```

#### 4 元素所属关系

```python
# 用isin()函数判断所属关系
>>>frame
   ball  pen  pencil  paper
0     0    1       2      3
1     4    5       6      7
2     8    9      10     11
3    12   13      14     15
>>>frame.isin([1, 15])
    ball    pen  pencil  paper
0  False   True   False  False
1  False  False   False  False
2  False  False   False  False
3  False  False   False   True
# 其返回只包含布尔值的DataFrame对象，可用于筛选元素
>>>frame[frame.isin([1, 15])]
   ball  pen  pencil  paper
0   NaN  1.0     NaN    NaN
1   NaN  NaN     NaN    NaN
2   NaN  NaN     NaN    NaN
3   NaN  NaN     NaN   15.0
```

#### 5 删除一列

```python
# del 命令，直接在DataFrame内删除
del frame['paper']
frame
   ball  pen  pencil
0     0    1       2
1     4    5       6
2     8    9      10
3    12   13      14
# 利用drop()函数返回一个新的删除后的DataFrame，原矩阵不变
# 删除行
>>>frame.drop(0)
   ball  pen  pencil
1     4    5       6
2     8    9      10
3    12   13      14
# 通过index标签删除行
>>>frame.drop(index=3)
   ball  pen  pencil
0     0    1       2
1     4    5       6
2     8    9      10
# 删除列
>>>frame.drop('pen', axis=1)
   ball  pencil
0     0       2
1     4       6
2     8      10
3    12      14
# 通过columns标签删除列
frame.drop(columns='ball')
   pen  pencil
0    1       2
1    5       6
2    9      10
3   13      14
```

#### 6 筛选

```python
# 通过指定条件筛选元素
>>>frame
   ball  pen  pencil paper
0     0    1       2     a
1     4    5       6     b
2     8    9      10     c
3    12   13      14     d
>>>frame[frame < 5]
报错，因为str无法与数字比较
>>>frame[frame == 5]
   ball  pen  pencil paper
0   NaN  NaN     NaN   NaN
1   NaN  5.0     NaN   NaN
2   NaN  NaN     NaN   NaN
3   NaN  NaN     NaN   NaN
```

#### 7 用嵌套字典生成DataFrame

```python
>>>mydict = {'red':{2012: 12, 2013:33}, 'white':{2011:13, 2012: 22, 2013:16}, 'blue':{2011:17, 2012:27, 2013:18}}
>>>frame2 = pd.DataFrame(mydict)
>>>frame2
       red  white  blue
2011   NaN     13    17
2012  12.0     22    27
2013  33.0     16    18
```

#### 8 DataFrame转置

```python
# 返回一个新的DataFrame的转置
>>>frame2.T
       2011  2012  2013
red     NaN  12.0  33.0
white  13.0  22.0  16.0
blue   17.0  27.0  18.0
```

### 4.5.3 Index对象

Index对象声明后，Index对象不能改变。不同数据结构共用Index对象时，该特性能够保证它的安全。

每个Index对象都有很多方法和属性，当你需要知道他们所包含的值时，这些方法和属性非常有用。

#### 1 Index对象的方法

```python
# idxmin()和idxmax()函数返回最值所对应的index
>>>ser = pd.Series(np.arange(4), index=['red', 'yellow', 'blue', 'green'])
>>>ser
red       0
yellow    1
blue      2
green     3
dtype: int64
>>>ser.idxmin()
'red'
>>>ser.idxmax()
'green'
```

#### 2 含有重复标签的Index

```python
>>>serd = pd.Series(np.arange(5), index=[1, 1, 2, 3, 4])
>>>serd
1    0
1    1
2    2
3    3
4    4
dtype: int64
# 标签对应多个元素，我们得到的将是一个Series对象而不是单个元素
>>>serd[1]
1    0
1    1
dtype: int64
# 数据量大时识别索引很难，is_unique属性可知数据结构是否存在重复的索引项。适用于DataFrame
>>>serd.index.is_unique
False
```

## 4.6 索引对象的其他功能

索引机制能实现很多基础操作。

### 4.6.1 更换索引

已有索引虽然不能更改，但是可以排序、添加新索引。

```python
>>>ser
red       0
yellow    1
blue      2
green     3
dtype: int64
# reindex()会返回一个新的对索引重新排序(值与索引对应)的Series，未设计的会不考虑，新的会添加nan值。DataFrame也如此。
>>>ser.reindex(['blue', 'green', 'yellow', 'pink'])
blue      2.0
green     3.0
yellow    1.0
pink      NaN
dtype: float64
# 自动填充或插值
>>>ser2
0    0
1    1
2    2
3    3
4    4
dtype: int64
# 返回一个新的Series，'ffill'代表front fill，新增的索引值拷贝前索引值。
>>>ser2.reindex(range(7), method='ffill')
0    0
1    1
2    2
3    3
4    4
5    4
6    4
dtype: int64
# 返回一个新的Series，'bfill'代表back fill，新增的索引值拷贝前索引值。
>>>ser2.reindex(range(8), method='bfill')
0    0.0
1    1.0
2    2.0
3    3.0
4    4.0
5    NaN
6    NaN
7    NaN
dtype: float64
# 对于Dataframe来说也有index()，fill_value不填的话默认nan
>>>frame.reindex(range(5), columns=['paper', 'ball', 'new'], fill_value=0)
  paper  ball  new
0     a     0    0
1     b     4    0
2     c     8    0
3     d    12    0
4     0     0    0
```

### 4.6.2 删除

见4.5.2 5 删除

### 4.6.3 算数和数据对齐

pandas能够将两个数据结构的索引对齐，参与运算的两个数据结构，其索引顺序可能不一致，而且有的索引项可能只存在于一个数据结构中。

不是索引都存在，则返回nan

Series和DataFrame都满足这一特性。

## 4.7 数据结构之间的运算

### 4.7.1 灵活的算术运算方法

DataFrame之间可以直接算术运算符运算，也可以借助灵活的算术运算方法完成：`add(), sub(), div(), mul()`。通过`frame1.add(frame2)`返回一个结果的DataFrame。

如果索引和列名称差别很大，新得到对象将有很多元素为nan。

### 4.7.2 DataFrame和Series对象之间的运算

```python
# Series对象的索引和DataFrame对象的列名称保持一致，可直接运算。
>>>frame
        ball  pen  pencil  paper
red        0    1       2      3
blue       4    5       6      7
yellow     8    9      10     11
white     12   13      14     15
>>>ser = pd.Series(np.arange(4), index=['ball','pen','pencil','paper'])
ser
ball      0
pen       1
pencil    2
paper     3
dtype: int64
>>>frame - ser
        ball  pen  pencil  paper
red        0    0       0      0
blue       4    4       4      4
yellow     8    8       8      8
white     12   12      12     12
# 索引和列名称不一致会返回nan
>>>ser['mug'] = 9
ser
ball      0
pen       1
pencil    2
paper     3
mug       9
dtype: int64
>>>frame - ser
        ball  mug  paper  pen  pencil
red        0  NaN      0    0       0
blue       4  NaN      4    4       4
yellow     8  NaN      8    8       8
white     12  NaN     12   12      12
```

### 4.8 函数应用和映射

### 4.8.1 操作元素的函数

```python
# 利用NumPy的通用函数来操作每个元素
>>>frame
        ball  pen  pencil  paper
red        0    1       2      3
blue       4    5       6      7
yellow     8    9      10     11
white     12   13      14     15
>>>np.sqrt(frame)
            ball       pen    pencil     paper
red     0.000000  1.000000  1.414214  1.732051
blue    2.000000  2.236068  2.449490  2.645751
yellow  2.828427  3.000000  3.162278  3.316625
white   3.464102  3.605551  3.741657  3.872983
```

### 4.8.2 按行或列执行操作的函数

DataFrame.apply\(\)按列，对一维数组进行运算；f返回标量，整体返回结果为一个Series。

```python
# 定义lambda函数
>>>f = lambda x : x.max() - x.min() 
# 普通方法定义函数
>>>def f2(x):
    return x.max() - x.min()
# apply(function)对列应用
>>>frame.apply(f)
ball      12
pen       12
pencil    12
paper     12
dtype: int64
# apply(function, axis=1)对行应用
>>>frame.apply(f2, axis=1)
red       3
blue      3
yellow    3
white     3
dtype: int64
```

通过合理定义函数f，f返回Series，整体返回DataFrame，即增加一个维度。

```python
>>>f3 = lambda x : pd.Series([x.max(), x.min()], index=['max', 'min'])
>>>frame.apply(f3)
     ball  pen  pencil  paper
max    12   13      14     15
min     0    1       2      3
```

### 4.8.3 统计函数

```python
>>>frame.sum()
ball      24
pen       28
pencil    32
paper     36
dtype: int64
>>>frame.mean()
ball      6.0
pen       7.0
pencil    8.0
paper     9.0
dtype: float64
>>>frame.describe()
            ball        pen     pencil      paper
count   4.000000   4.000000   4.000000   4.000000
mean    6.000000   7.000000   8.000000   9.000000
std     5.163978   5.163978   5.163978   5.163978
min     0.000000   1.000000   2.000000   3.000000
25%     3.000000   4.000000   5.000000   6.000000
50%     6.000000   7.000000   8.000000   9.000000
75%     9.000000  10.000000  11.000000  12.000000
max    12.000000  13.000000  14.000000  15.000000
```

## 4.9 排序和排位次

**Series的标签排序Series.sort\_index\(\)**

```python
>>> s = pd.Series(['a', 'b', 'c', 'd'], index=[3, 2, 1, 4])
>>> s
3    a
2    b
1    c
4    d
dtype: object
# 默认情况下index是数字，则由小到大、或字母acii码ascending排序。
>>> s.sort_index()
1    c
2    b
3    a
4    d
dtype: object
# 但是 ascending=False 则反过来
>>> s.sort_index(ascending=False)
4    d
3    a
2    b
1    c
dtype: object
# 以上是返回一个新的Series，如果是改变Series本身，需要令inplace=True
>>> s.sort_index(inplace=True)
>>> s
s
1    c
2    b
3    a
4    d
dtype: object
# By default NaNs are put at the end, but use `na_position` to place them at the beginning  
>>> s = pd.Series(['a', 'b', 'c', 'd'], index=[3, 2, 1, np.nan])
>>> s
3.0    a
2.0    b
1.0    c
NaN    d
dtype: object
>>> s.sort_index(na_position='first')
NaN    d
1.0    c
2.0    b
3.0    a
dtype: object
```

**Series的元素排序Series.sort\_value\(\)**，用法和参数与Series.sort\_index\(\)相同。

**DataFrame的标签排序DataFrame.sort\_index\(\)**，参数大抵相同，只是如果要对列标签排序需要参数`axis=1`。

```python
>>> frame.sort_index(axis=1, ascending=False, inplace=False, na_position='last')
        pencil  pen  paper  ball
red          2    1      3     0
blue         6    5      7     4
yellow      10    9     11     8
white       14   13     15    12
```

**DataFrame的元素排列DataFrame.sort\_value\(\)**

`by`：可一个index或多个index组成的列表，行或列，行时axis=1。

`axis`：默认axis=0代表列；axis=1代表行。

`ascending`：升序，bool

`in_place`：改变自身对象

`na_position`：'first'或'last'

```python
>>>frame.sort_values(by='pen', ascending=False)
        ball  pen  pencil  paper
white     12   13      14     15
yellow     8    9      10     11
blue       4    5       6      7
red        0    1       2      3
>>>frame.sort_values(by='red', axis=1, ascending=False)
        paper  pencil  pen  ball
red         3       2    1     0
blue        7       6    5     4
yellow     11      10    9     8
white      15      14   13    12
```

**Pandas中的rank\(\)方法——优先级问题**，排位次。

> 以下关于rank\(\)函数解释来源于[CSDN](https://blog.csdn.net/ZOUZHEN_ID/article/details/82631745)。

首先，生成Series，并使用默认rank方法（默认使用平均排名方式,也就是说当出现相同元素的时候,优先级相加除以元素的个数）：

```python
>>> obj = pd.Series([7, -5, 7, 4, 2, 0, 4])
>>> obj.rank()
0    6.5
1    1.0
2    6.5
3    4.5
4    3.0
5    2.0
6    4.5
dtype: float64
```

例如-5对应的优先级为1,可按如下表示:

```text
-5 -> 1.0 ;
 0 -> 2.0 ;  
{ 4 -> 4.0 ; 4 -> 5.0  ||   4 -> (4.0+5.0)/2=4.5 ; 4  (4.0+5.0)/2=4.5 }
7 -> 6.5 ; 7 -> 6.5 ;
```

当采用method=first时，此时按值的大小进行排序,元素相同时也不对其优先级进行平均.

```python
>>> obj.rank(method='first')
0    6.0
1    1.0
2    7.0
3    4.0
4    3.0
5    2.0
6    5.0
dtype: float64
```

## 4.10 相关性和方差

通过`corr()`和`cov()`函数可以实现求**相关系数**和**协方差**。

```python
>>> s1 = pd.Series([3, 4, 3, 4, 5, 4, 3, 2])
>>> s2 = pd.Series([1, 2, 3, 4, 4, 3, 2, 1])
>>> frame2 = pd.DataFrame([[1,4,3,6],[4,5,6,1],[3,3,1,5],[4,1,6,4]], columns=['a','b','c','d'])
>>> s1
0    3
1    4
2    3
3    4
4    5
5    4
6    3
7    2
dtype: int64
>>> s2
0    1
1    2
2    3
3    4
4    4
5    3
6    2
7    1
dtype: int64
>>> frame2
   a  b  c  d
0  1  4  3  6
1  4  5  6  1
2  3  3  1  5
3  4  1  6  4
# Series和Series之间
>>> s1.corr(s2)
0.7745966692414834
>>> s1.cov(s2)
0.8571428571428571
# DataFrame自身
>>> frame2.corr()
          a         b         c         d
a  1.000000 -0.276026  0.577350 -0.763763
b -0.276026  1.000000 -0.079682 -0.361403
c  0.577350 -0.079682  1.000000 -0.692935
d -0.763763 -0.361403 -0.692935  1.000000
>>> frame2.cov()
          a         b         c         d
a  2.000000 -0.666667  2.000000 -2.333333
b -0.666667  2.916667 -0.333333 -1.333333
c  2.000000 -0.333333  6.000000 -3.666667
d -2.333333 -1.333333 -3.666667  4.666667
# DataFrame与Series之间
>>> s3 = pd.Series([0, 1, 2, 3, 9])
>>> frame2.corrwith(s3)
a    0.730297
b   -0.831522
c    0.210819
d   -0.119523
dtype: float64
# DataFrame之间通过corrowith()计算相关系数，列要相同或有匹配项，不匹配返回nan
>>> frame3 = pd.DataFrame(np.arange(16).reshape((4,4)), columns=['a','b','c','d'])
>>> frame3
    a   b   c   d
0   0   1   2   3
1   4   5   6   7
2   8   9  10  11
3  12  13  14  15
>>> frame2.corrwith(frame3)
a    0.730297
b   -0.831522
c    0.210819
d   -0.119523
dtype: float64
```

## 4.11 NaN数据

pandas库在计算各种描述统计量时，其实没有考虑nan值。

### 4.11.1 为元素赋NaN值

利用NumPy库中的nan赋值。

```python
>>>s4 = pd.Series([0, 1, 2, np.nan, 9])
>>> s4
0    0.0
1    1.0
2    2.0
3    NaN
4    9.0
dtype: float64
>>> s4[0] = np.nan
>>> s4
0    NaN
1    1.0
2    2.0
3    NaN
4    9.0
dtype: float64
>>> s4[1] = None
s4
0    NaN
1    NaN
2    2.0
3    NaN
4    9.0
dtype: float64
```

### 4.11.2 过滤NaN

**Series过滤nan：**

```python
>>> s4
0    NaN
1    NaN
2    2.0
3    NaN
4    9.0
dtype: float64
# 通过dropna()返回一个去处nan的Series
>>> s4.dropna()
2    2.0
4    9.0
dtype: float64
# 通过布尔值过滤
>>> s4[s4.notna()]
2    2.0
4    9.0
dtype: float64
```

**DataFrame过滤nan：** 同样适用`dropna()`函数，返回一个新的DataFrame，但是会去除含有`nan`的行和列，参数`how='all'`时，会去除一个所有行或列都是`nan`的行或列。

```python
>>> frame4 = pd.DataFrame([[6, np.nan, 6], [np.nan, np.nan, np.nan], [2, np.nan, 5]], columns=['a', 'b', 'c'])
>>> frame4
     a   b    c
0  6.0 NaN  6.0
1  NaN NaN  NaN
2  2.0 NaN  5.0
>>> frame4.dropna()
Empty DataFrame
Columns: [a, b, c]
Index: []
>>> frame4.dropna(how='all')
     a   b    c
0  6.0 NaN  6.0
2  2.0 NaN  5.0
```

### 4.11.3 为NaN元素填充其他值

通过`fillna()`函数**返回一个新**的`nan`被填充后的DataFrame。

参数可以是数值或字典。

```python
>>> frame4
     a   b    c
0  6.0 NaN  6.0
1  NaN NaN  NaN
2  2.0 NaN  5.0
>>> frame4.fillna(0)
     a    b    c
0  6.0  0.0  6.0
1  0.0  0.0  0.0
2  2.0  0.0  5.0
>>> frame4.fillna(frame4.mean())
     a   b    c
0  6.0 NaN  6.0
1  4.0 NaN  5.5
2  2.0 NaN  5.0
>>> frame4.fillna({'a':0, 'b':1, 'c':2})
     a    b    c
0  6.0  1.0  6.0
1  0.0  1.0  2.0
2  2.0  1.0  5.0
```

## 4.12 等级索引和分级

```python
# Series索引的层级排序
>>> mser = pd.Series(np.random.rand(8), index=[['white', 'white', 'white', 'blue', 'blue', 'red', 'red', 'red'], ['up', 'down', 'right', 'up', 'down', 'up', 'down', 'left']])
>>> mser
white  up       0.073285
       down     0.063399
       right    0.972716
blue   up       0.761852
       down     0.497059
red    up       0.031965
       down     0.938567
       left     0.100455
dtype: float64
>>> mser.index
MultiIndex(levels=[['blue', 'red', 'white'], ['down', 'left', 'right', 'up']],
           codes=[[2, 2, 2, 0, 0, 1, 1, 1], [3, 0, 2, 3, 0, 3, 0, 1]])
# 返回第一级下的数据
>>> mser['white']
up       0.073285
down     0.063399
right    0.972716
dtype: float64
# 返回第二级下的数据
>>> mser[:, 'up']
white    0.073285
blue     0.761852
red      0.031965
dtype: float64
# 以标签值返回具体元素数据
>>> mser['white', 'up']
0.073284659895271
# 将一维Series利用unstack()函数转换为DataFrame
>>> mser.unstack()
           down      left     right        up
blue   0.497059       NaN       NaN  0.761852
red    0.938567  0.100455       NaN  0.031965
white  0.063399       NaN  0.972716  0.073285
# 将二维的DataFrame利用stack()函数转换为Series
>>> frame
        ball  pen  pencil  paper
red        0    1       2      3
blue       4    5       6      7
yellow     8    9      10     11
white     12   13      14     15
>>> frame.stack()
red     ball       0
        pen        1
        pencil     2
        paper      3
blue    ball       4
        pen        5
        pencil     6
        paper      7
yellow  ball       8
        pen        9
        pencil    10
        paper     11
white   ball      12
        pen       13
        pencil    14
        paper     15
dtype: int64
# DataFrame的行列索引也可以定义层级
>>> frame2 = pd.DataFrame(np.arange(16).reshape((4, 4)), index=[['white', 'white', 'red', 'red'], ['up', 'down', 'up', 'down']], columns=[['pen', 'pen', 'paper', 'paper'],[1, 2, 1, 2]])
>>> frame2
           pen     paper    
             1   2     1   2
white up     0   1     2   3
      down   4   5     6   7
red   up     8   9    10  11
      down  12  13    14  15
```

### 4.12.1 重新调整顺序和为层级排序

```python
>>> frame2 = pd.DataFrame(np.random.randn(4, 4), index=[['white', 'white', 'red', 'red'], ['up', 'down', 'up', 'down']], columns=[['pen', 'pen', 'paper', 'paper'],[1, 2, 1, 2]])
>>> frame2
                 pen               paper          
                   1         2         1         2
white up   -0.700044 -0.563039 -1.378678  0.768536
      down -0.571696 -2.304693  1.675079  0.178544
red   up    1.312485 -0.444405 -0.366702  0.712684
      down  0.803027 -0.977137  2.062417  0.877883
# 行标签信息
>>>frame2.index
MultiIndex(levels=[['red', 'white'], ['down', 'up']],
           codes=[[1, 1, 0, 0], [1, 0, 1, 0]])
# 列标签信息
>>>frame2.columns
MultiIndex(levels=[['paper', 'pen'], [1, 2]],
           codes=[[1, 1, 0, 0], [0, 1, 0, 1]])
# 给这一级标签命名
>>> frame2.index.names = ['colors', 'status']
>>> frame2.columns.names = ['objects', 'id']
>>> frame2
objects             pen               paper          
id                    1         2         1         2
colors status                                        
white  up     -0.700044 -0.563039 -1.378678  0.768536
       down   -0.571696 -2.304693  1.675079  0.178544
red    up      1.312485 -0.444405 -0.366702  0.712684
       down    0.803027 -0.977137  2.062417  0.877883
# 交换层级，返回新的DataFrame，不改变值得结构
>>> frame2.swaplevel('colors', 'status')
objects             pen               paper          
id                    1         2         1         2
status colors                                        
up     white  -0.700044 -0.563039 -1.378678  0.768536
down   white  -0.571696 -2.304693  1.675079  0.178544
up     red     1.312485 -0.444405 -0.366702  0.712684
down   red     0.803027 -0.977137  2.062417  0.877883
```

### 4.12.2 按层级统计数据

```python
# 按照行层级
>>> frame2.sum(level='colors')
objects       pen               paper          
id              1         2         1         2
colors                                         
white   -1.271740 -2.867732  0.296401  0.947080
red      2.115512 -1.421542  1.695715  1.590567
# 按照列层级，需要指定参数axis=1
>>> frame2.sum(axis=1, level='objects')
objects             pen     paper
colors status                    
white  up     -1.263082 -0.610143
       down   -2.876390  1.853623
red    up      0.868080  0.345982
       down   -0.174111  2.940300
```

