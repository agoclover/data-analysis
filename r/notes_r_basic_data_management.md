# notes\_R\_basic\_data\_management

## 安装与基础

```r
# 安装包
install.packages("ggplot2")
# 加载包
library(ggplot2)
# 查看包
installed.packages()

# 基本语法
a <- 10
b <- "课程"
b = 5
x = c(1, 2, 3, 4, 5)
y = c(6:10)

# 运算符
a / b
a %% b # 余数
a %/% b # 整除
a < b
a != b
! x # 非x
x | y # 或
x & y # 与

# 访问数据
x[1]
x[c(1, 4)]

# 查看数据类型
mode(y)

# if 语句
grade <- 82
{
  if(grade >= 80)
    print("优秀")
  else if(grade >= 60)
    print("合格")
  else
    print("不合格")
}

# while 循环语句
i = 10
while(i > 0){
  print(i)
  i = i - 2
}

# for 循环语句
for(i in 1:10)
  print("hi")
```

## R 数据结构

### 数据类型

1. 向量 vector
2. 矩阵 matrix
3. 数组 array
4. 数据框 dataframe
5. 列表 list

### 向量 vector

#### 向量

用于存储数值型、字符型或逻辑型的一堆数组。

单个向量中必须是相同的数据类型。

#### 向量生成

```r
a <- c(1, 2, 3, 4, 5)
b <- c(6 : 10)
c <- c("red", "blue", "green")
d <- c(TRUE, FALSE, FALSE, TRUE)
# c是combine的意思
```

#### 访问向量

```r
# 访问某个值
# 默认首位是1，这点和python不同
a[n]
# 访问某些值
a[c(1, 4)]
```

### 矩阵 matrix

#### 矩阵

二维数据集，且其中每个元素都是相同的数据类型。

#### 矩阵生成

```r
> n1 <- c("a", "b", "c", "d", "e")
> n2 <- c("A", "B", "C", "D")
> m1 <- matrix(1:20, nrow=5, ncol=4, byrow=TRUE, dimnames = list(n1, n2))
> m1
   A  B  C  D
a  1  2  3  4
b  5  6  7  8
c  9 10 11 12
d 13 14 15 16
e 17 18 19 20
```

#### 访问矩阵中的元素

```r
m1[2, 4] # 访问第2行第4列的元素
m1[2, c(2, 3)] # 访问第2行第2列和第3列的元素
m1[3,] # 访问第3行的所有元素
M1[, 4] # 访问第4列的所有元素
```

### 数组 array

#### 数组

数组与矩阵类似，但是维度可以&gt;2

#### 数组生成

这里定义一个3维数组：

```r
> n1 <- c("a", "b")
> n2 <- c("c1", "c2", "c3")
> n3 <- c("z1", "z2", "z3", "z4")
> array1 <- array(1:24, c(2, 3, 4), dimnames = list(n1, n2, n3))
> array1
, , z1

  c1 c2 c3
a  1  3  5
b  2  4  6

, , z2

  c1 c2 c3
a  7  9 11
b  8 10 12

, , z3

  c1 c2 c3
a 13 15 17
b 14 16 18

, , z4

  c1 c2 c3
a 19 21 23
b 20 22 24
```

#### 数据访问

访问第2行第3列，在第1个矩阵中的元素：

```r
> array1[2, 3, 1]
[1] 6
```

### 数据框 dataframe

#### 数据框

由不同的列组成，不同列可以是不同的数据类型。

这些和python中的DataFrame类似。

#### 生成 dataframe

由向量作为各列生成：

```r
> age <- c(22, 23, 35, 44)
> gender <- c("female", "male", "male", "female")
> grade <- c(80, 90, 96, 77)
> df1 <- data.frame(age, gender, grade)
> df1
  age gender grade
1  22 female    80
2  23   male    90
3  35   male    96
4  44 female    77
```

比较自由的生成形式，比如固定值、数列、抽样等：

```r
# 生成字母
> L3 <- LETTERS[1:3]
> L3
[1] "A" "B" "C"
# 抽样
> fac <- sample(L3, 10, replace = TRUE)
> fac
[1] "A" "C" "B" "C" "C" "A" "C" "A" "A" "B"
# 创建dataframe
> data.frame(a = 1, b = 1:10, c = sample(L3, 10, replace = TRUE))
   a  b c
1  1  1 A
2  1  2 A
3  1  3 A
4  1  4 B
5  1  5 C
6  1  6 C
7  1  7 C
8  1  8 C
9  1  9 C
10 1 10 B
# 判断是不是dataframe
> d <- data.frame(x = 1, y = 1:10, fac = fac)
> is.data.frame(d)
[1] TRUE
```

#### 访问 dataframe

通过下标访问：

```r
> df1[1,]
  age gender grade
1  22 female    80
> df1[, 2]
[1] female male   male   female
Levels: female male
> df1[1, c(1, 2)]
  age gender
1  22 female
# 访问1~2行
> df1[1:2,]
  age gender grade
1  22 female    80
2  23   male    90
# 访问除了第1行
> df1[-1,]
  age gender grade
2  23   male    90
3  35   male    96
4  44 female    77
# 访问除了第2行
> df1[-2,]
  age gender grade
1  22 female    80
3  35   male    96
4  44 female    77
```

通过索引访问：

```r
> df1["age"]
  age
1  22
2  23
3  35
4  44
> df1[c("age", "gender")]
  age gender
1  22 female
2  23   male
3  35   male
4  44 female
```

通过`$`符号:

```r
> df1$age
[1] 22 23 35 44
```

### 列表 list

#### 列表

列表就是一些对象的有序集合，这些集合可能是向量、矩阵、数组、数据框和其他列表的组合。

#### 列表生成

```r
> v1 <- c(1, 2, 3, 4)
> m1 <- matrix(1:20, 4)
> m1
     [,1] [,2] [,3] [,4] [,5]
[1,]    1    5    9   13   17
[2,]    2    6   10   14   18
[3,]    3    7   11   15   19
[4,]    4    8   12   16   20
> age <- c(19, 20, 23)
> country <- c("China", "US", "UK")
> df1 <- data.frame(age, country)
> list1 <- list(v=v1, m=m1, df=df1)
> list1
$v
[1] 1 2 3 4

$m
     [,1] [,2] [,3] [,4] [,5]
[1,]    1    5    9   13   17
[2,]    2    6   10   14   18
[3,]    3    7   11   15   19
[4,]    4    8   12   16   20

$df
  age country
1  19   China
2  20      US
3  23      UK
```

#### 访问元素

通过下标访问：

```r
> list1[[2]][2, 3]
[1] 10
```

通过`$`访问：

```r
> list1$v
[1] 1 2 3 4
```

### 区别

|  | vector | matrix | array | dataframe | List |
| :--- | :--- | :--- | :--- | :--- | :--- |
| 维数 | 一维 | 二维 | 可以&gt;二维 | 二维 | 对象集合 |
| 数据类型 |  | 相同 |  | 各列数据类型 |  |

## I/O

### function related

| Function | Info |
| :--- | :--- |
| getwd\(\) | 得到当前文件存放的工作目录 |
| setwd\(\) | 重新设置当前文件存放的工作目录 |
| read.csv\(path\) | 导入数据文件 |
| write.csv\(data, name\) | 导出数据文件 |

### 导入`.csv`文件或 Excel 文件

1. 通过`setwd()`重置工作目录；
2. 将Excel文件另存为`.csv`文件\(或有本身`.csv`文件\)；
3. `data_titanic = read.csv("titanic_data.csv")`。

### 导出数据流程

1. 通过`setwd()`重置工作目录；
2. 输入`write.csv(data, name)`;
3. 不导出索引`write.csv(data, name, row.names = FALSE)`。

## dataframe 常用操作函数

### 简化数据框的批量操作

```r
attach(dataframe) # 绑定数据框
操作1
操作2
detach(dataframe) # 接触绑定数据框

with(dataframe, {
  操作1
  操作2
  操作3
})
```

绑定示例，调用`mtcars`数据集：

```r
> df1 <- head(mtcars)
> df1
                   mpg cyl disp  hp drat    wt  qsec vs am gear carb
Mazda RX4         21.0   6  160 110 3.90 2.620 16.46  0  1    4    4
Mazda RX4 Wag     21.0   6  160 110 3.90 2.875 17.02  0  1    4    4
Datsun 710        22.8   4  108  93 3.85 2.320 18.61  1  1    4    1
Hornet 4 Drive    21.4   6  258 110 3.08 3.215 19.44  1  0    3    1
Hornet Sportabout 18.7   8  360 175 3.15 3.440 17.02  0  0    3    2
Valiant           18.1   6  225 105 2.76 3.460 20.22  1  0    3    1
> df1$cyl
[1] 6 6 4 6 8 6
# attach()绑定
> attach(df1)
The following object is masked from package:ggplot2:

    mpg

> cyl
[1] 6 6 4 6 8 6
> hp
[1] 110 110  93 110 175 105
> detach(df1)

# with() 绑定
> df1$mpg + df1$cyl + df1$disp
[1] 187.0 187.0 134.8 285.4 386.7 249.1
> with(df1, {
+   mpg + cyl + disp
+ })
[1] 187.0 187.0 134.8 285.4 386.7 249.1
```

### 增加dataframe的变量\(列\)

```r
# within()
within(dataframe, {
  操作1
  操作2
  操作n
})

# transform()
tansform(dataframe, 操作1, ...操作n)

# dplyr包
mutate(dataframe, 操作1, ...操作n)
```

示例，调用`mtcars`数据集：

```r
# within() 法
> df1
                   mpg cyl disp  hp drat    wt  qsec vs am gear carb
Mazda RX4         21.0   6  160 110 3.90 2.620 16.46  0  1    4    4
Mazda RX4 Wag     21.0   6  160 110 3.90 2.875 17.02  0  1    4    4
Datsun 710        22.8   4  108  93 3.85 2.320 18.61  1  1    4    1
Hornet 4 Drive    21.4   6  258 110 3.08 3.215 19.44  1  0    3    1
Hornet Sportabout 18.7   8  360 175 3.15 3.440 17.02  0  0    3    2
Valiant           18.1   6  225 105 2.76 3.460 20.22  1  0    3    1

> df2 = within(df1,{
+   sum = mpg + cyl + disp
+   mean = (mpg + cyl + disp) / 3
+ })
> df2
                   mpg cyl disp  hp drat    wt  qsec vs am gear carb      mean   sum
Mazda RX4         21.0   6  160 110 3.90 2.620 16.46  0  1    4    4  62.33333 187.0
Mazda RX4 Wag     21.0   6  160 110 3.90 2.875 17.02  0  1    4    4  62.33333 187.0
Datsun 710        22.8   4  108  93 3.85 2.320 18.61  1  1    4    1  44.93333 134.8
Hornet 4 Drive    21.4   6  258 110 3.08 3.215 19.44  1  0    3    1  95.13333 285.4
Hornet Sportabout 18.7   8  360 175 3.15 3.440 17.02  0  0    3    2 128.90000 386.7
Valiant           18.1   6  225 105 2.76 3.460 20.22  1  0    3    1  83.03333 249.1

# transform()
> transform(df1, sum=mpg+cyl+disp, mean=(mpg + cyl + disp) / 3)
                   mpg cyl disp  hp drat    wt  qsec vs am gear carb   sum      mean
Mazda RX4         21.0   6  160 110 3.90 2.620 16.46  0  1    4    4 187.0  62.33333
Mazda RX4 Wag     21.0   6  160 110 3.90 2.875 17.02  0  1    4    4 187.0  62.33333
Datsun 710        22.8   4  108  93 3.85 2.320 18.61  1  1    4    1 134.8  44.93333
Hornet 4 Drive    21.4   6  258 110 3.08 3.215 19.44  1  0    3    1 285.4  95.13333
Hornet Sportabout 18.7   8  360 175 3.15 3.440 17.02  0  0    3    2 386.7 128.90000
Valiant           18.1   6  225 105 2.76 3.460 20.22  1  0    3    1 249.1  83.03333

# dplyr包mutate()
> mutate(df1, sum=mpg+cyl+disp, mean=(mpg + cyl + disp) / 3)
   mpg cyl disp  hp drat    wt  qsec vs am gear carb   sum      mean
1 21.0   6  160 110 3.90 2.620 16.46  0  1    4    4 187.0  62.33333
2 21.0   6  160 110 3.90 2.875 17.02  0  1    4    4 187.0  62.33333
3 22.8   4  108  93 3.85 2.320 18.61  1  1    4    1 134.8  44.93333
4 21.4   6  258 110 3.08 3.215 19.44  1  0    3    1 285.4  95.13333
5 18.7   8  360 175 3.15 3.440 17.02  0  0    3    2 386.7 128.90000
6 18.1   6  225 105 2.76 3.460 20.22  1  0    3    1 249.1  83.03333
```

`within()`是双参数，后面各操作合为一个参数，`transform()`和`mutate()`是多参数输入。对于后两种实现方式，`mutate()`可以调用已前面输入的参数而`transform()`不可以，当然`within()`也是可以的，比如以下操作：

```r
> mutate(df1, sum=mpg+cyl+disp, mean=(mpg + cyl + disp) / 3, x=sum+mean)
   mpg cyl disp  hp drat    wt  qsec vs am gear carb   sum      mean        x
1 21.0   6  160 110 3.90 2.620 16.46  0  1    4    4 187.0  62.33333 249.3333
2 21.0   6  160 110 3.90 2.875 17.02  0  1    4    4 187.0  62.33333 249.3333
3 22.8   4  108  93 3.85 2.320 18.61  1  1    4    1 134.8  44.93333 179.7333
4 21.4   6  258 110 3.08 3.215 19.44  1  0    3    1 285.4  95.13333 380.5333
5 18.7   8  360 175 3.15 3.440 17.02  0  0    3    2 386.7 128.90000 515.6000
6 18.1   6  225 105 2.76 3.460 20.22  1  0    3    1 249.1  83.03333 332.1333
```

## 排序和选取子集\(条件筛选\)

### 排序

#### `order()`

R自带的排序函数。仍然以`mtcars`这个数据集为例：

```r
# 仅排序，注意这里是得到下标的排序
> order(mtcars$mpg, decreasing = TRUE)
 [1] 20 18 19 28 26 27  8  3  9 21  4 32  1  2 30 10 25  5  6 11 13 12 29 22 14 23 31 17  7
[30] 24 15 16
# 下标排序后，对原数据进行排序
# 这里并不是仅仅只有mpg这里列的数据进行了排序，而是所有数据都据此新的下标进行了排序
# [,] "," 后没有写即返回所有列，[, 1:3]是返回1~3列
> mtcars[order(mtcars$mpg, decreasing = TRUE),]
                     mpg cyl  disp  hp drat    wt  qsec vs am gear carb
Toyota Corolla      33.9   4  71.1  65 4.22 1.835 19.90  1  1    4    1
Fiat 128            32.4   4  78.7  66 4.08 2.200 19.47  1  1    4    1
Honda Civic         30.4   4  75.7  52 4.93 1.615 18.52  1  1    4    2
Lotus Europa        30.4   4  95.1 113 3.77 1.513 16.90  1  1    5    2
Fiat X1-9           27.3   4  79.0  66 4.08 1.935 18.90  1  1    4    1
Porsche 914-2       26.0   4 120.3  91 4.43 2.140 16.70  0  1    5    2
Merc 240D           24.4   4 146.7  62 3.69 3.190 20.00  1  0    4    2
Datsun 710          22.8   4 108.0  93 3.85 2.320 18.61  1  1    4    1
Merc 230            22.8   4 140.8  95 3.92 3.150 22.90  1  0    4    2
Toyota Corona       21.5   4 120.1  97 3.70 2.465 20.01  1  0    3    1
Hornet 4 Drive      21.4   6 258.0 110 3.08 3.215 19.44  1  0    3    1
Volvo 142E          21.4   4 121.0 109 4.11 2.780 18.60  1  1    4    2
Mazda RX4           21.0   6 160.0 110 3.90 2.620 16.46  0  1    4    4
Mazda RX4 Wag       21.0   6 160.0 110 3.90 2.875 17.02  0  1    4    4
Ferrari Dino        19.7   6 145.0 175 3.62 2.770 15.50  0  1    5    6
Merc 280            19.2   6 167.6 123 3.92 3.440 18.30  1  0    4    4
Pontiac Firebird    19.2   8 400.0 175 3.08 3.845 17.05  0  0    3    2
Hornet Sportabout   18.7   8 360.0 175 3.15 3.440 17.02  0  0    3    2
Valiant             18.1   6 225.0 105 2.76 3.460 20.22  1  0    3    1
Merc 280C           17.8   6 167.6 123 3.92 3.440 18.90  1  0    4    4
Merc 450SL          17.3   8 275.8 180 3.07 3.730 17.60  0  0    3    3
Merc 450SE          16.4   8 275.8 180 3.07 4.070 17.40  0  0    3    3
Ford Pantera L      15.8   8 351.0 264 4.22 3.170 14.50  0  1    5    4
Dodge Challenger    15.5   8 318.0 150 2.76 3.520 16.87  0  0    3    2
Merc 450SLC         15.2   8 275.8 180 3.07 3.780 18.00  0  0    3    3
AMC Javelin         15.2   8 304.0 150 3.15 3.435 17.30  0  0    3    2
Maserati Bora       15.0   8 301.0 335 3.54 3.570 14.60  0  1    5    8
Chrysler Imperial   14.7   8 440.0 230 3.23 5.345 17.42  0  0    3    4
Duster 360          14.3   8 360.0 245 3.21 3.570 15.84  0  0    3    4
Camaro Z28          13.3   8 350.0 245 3.73 3.840 15.41  0  0    3    4
Cadillac Fleetwood  10.4   8 472.0 205 2.93 5.250 17.98  0  0    3    4
Lincoln Continental 10.4   8 460.0 215 3.00 5.424 17.82  0  0    3    4
# 次要关键字排序
mtcars[order(mtcars$mpg, mtcars$cyl),]
```

#### `dplyr`包中的`arrange()`

`arrange()`函数比较方便，降序直接在变量前加"-"，但是排序后丢失了索引。

```r
arrange(mtcars, -mpg, vs)
```

### 选取子集\(条件筛选\)

#### `subset()`函数

筛选出`mpg >= 20 & vs == 0`的数据：

```r
> subset(mtcars, mpg >= 20 & vs == 0)
              mpg cyl  disp  hp drat    wt  qsec vs am gear carb
Mazda RX4      21   6 160.0 110 3.90 2.620 16.46  0  1    4    4
Mazda RX4 Wag  21   6 160.0 110 3.90 2.875 17.02  0  1    4    4
Porsche 914-2  26   4 120.3  91 4.43 2.140 16.70  0  1    5    2
# 仅返回mpg列
> subset(mtcars, mpg >= 20 & vs == 0, mpg)
              mpg
Mazda RX4      21
Mazda RX4 Wag  21
Porsche 914-2  26
```

#### dplyr包中的filter\(\)函数

这个函数每次返回全部列

```r
filter(mtcars, mpg >= 20 | vs ==0)
```

## 重命名列和组合数据

### 重命名

#### names\(\)

将`data`这个dataframe的第1列的列名重命名为"mmm"，这是在data上操作的：

`names(data)[1] <- "mmm"`

#### reshape包中的rename\(\)

`rename()`这个函数是返回一个新的dataframe，语法如下:

`rename(data, c(mpg="newmpg", hp="newhp"))`

但如果要修改原数据一种方法就是将得到的新数据赋值给原数据名：

`data <- rename(data, c(mpg="newmpg", hp="newhp"))`

### 组合数据

#### paste\(\) 拼接函数

Concatenate vectors after converting to character.

```r
# 示例1
> v1 <- c(10, 20, 30)
> v2 <- "g"
> v <- paste(v1, v2)
> v
[1] "10 g" "20 g" "30 g"
> v <- paste(v1, v2, sep = "") # 不加空格
> v
[1] "10g" "20g" "30g"

# 示例2
> nth <- paste(1:12, c("st", "nd", "rd", rep("th", 9)))
> nth
 [1] "1 st"  "2 nd"  "3 rd"  "4 th"  "5 th"  "6 th"  "7 th"  "8 th"  "9 th"  "10 th" "11 th"
[12] "12 th
```

#### rbind\(\) 行合并 、cbind\(\) 列合并

```r
# rbind()
> df1 <- mtcars[1:10,] # 选取1~10行
> df2 <- mtcars[11:20,] # 选取11~20行
> df <- rbind(df1, df2)

# cbind()
> df1 <- mtcars[,1:3] # 选取1~3列
> df2 <- mtcars[,4:6] # 选取4~6列
> df <- cbind(df1, df2)
```

#### merge\(\): inner/left/right join

即匹配，相当于vlookup或SQL中的查询或连接。

```r
# 先创建两个dataframe
> df1 <- data.frame(id1 = c(101, 102, 104, 106, 107),
+            gender = c("男", "女", "男", "女", "男"))
> df1
  id1 gender
1 101     男
2 102     女
3 104     男
4 106     女
5 107     男
> df2 <- data.frame(id2 = c(101, 103, 104, 105, 107, 110, 101),
+                   date = c("2016/1/1", "2016/1/20", "2016/1/30", "2016/2/4", "2016/3/5", "2016/4/6", "2016/5/7"),
+                   income = c(2000, 3000, 5000, 4000, 2500, 6000, 3000))
> df2
  id2      date income
1 101  2016/1/1   2000
2 103 2016/1/20   3000
3 104 2016/1/30   5000
4 105  2016/2/4   4000
5 107  2016/3/5   2500
6 110  2016/4/6   6000
7 101  2016/5/7   3000

# inner join
# 公有的变量(组合键)名称不一样
> merge(df1, df2, by.x = "id1", by.y = "id2")
  id1 gender      date income
1 101     男  2016/1/1   2000
2 101     男  2016/5/7   3000
3 104     男 2016/1/30   5000
4 107     男  2016/3/5   2500
# 公有的变量(组合键)名称一样
> names(df1)[1] <- "id"
> names(df2)[1] <- "id"
> merge(df1, df2, by = "id")
  id1 gender      date income
1 101     男  2016/1/1   2000
2 101     男  2016/5/7   3000
3 104     男 2016/1/30   5000
4 107     男  2016/3/5   2500

# left join
> merge(df1, df2, by = "id", all.x = TRUE)
   id gender      date income
1 101     男  2016/1/1   2000
2 101     男  2016/5/7   3000
3 102     女      <NA>     NA
4 104     男 2016/1/30   5000
5 106     女      <NA>     NA
6 107     男  2016/3/5   2500

# right join
> merge(df1, df2, by = "id", all.y = TRUE)
   id gender      date income
1 101     男  2016/1/1   2000
2 101     男  2016/5/7   3000
3 103   <NA> 2016/1/20   3000
4 104     男 2016/1/30   5000
5 105   <NA>  2016/2/4   4000
6 107     男  2016/3/5   2500
7 110   <NA>  2016/4/6   6000

# 均保留
> merge(df1, df2, by = "id", all = TRUE)
   id gender      date income
1 101     男  2016/1/1   2000
2 101     男  2016/5/7   3000
3 102     女      <NA>     NA
4 103   <NA> 2016/1/20   3000
5 104     男 2016/1/30   5000
6 105   <NA>  2016/2/4   4000
7 106     女      <NA>     NA
8 107     男  2016/3/5   2500
9 110   <NA>  2016/4/6   6000
```

## apply家族函数

### apply\(\)函数

#### 作用

一般有类似python的map和reduce两种方式。

map即函数作用与每一个元素，reduce即按照行或列来代入聚合函数。

#### 语法

`apply(X, MARGIN, FUN, ...)`

arguements：

| arguement | meaning |
| :--- | :--- |
| `X` | an array, including a matrix. |
| `MARGIN` | a vector giving the subscripts which the function will be applied over. E.g., for a matrix `1` indicates rows, `2` indicates columns, `c(1, 2)`indicates rows and columns. Where `X` has named dimnames, it can be a character vector selecting dimension names. |
| `FUN` | the function to be applied: see ‘Details’. In the case of functions like `+`, `%*%`, etc., the function name must be backquoted or quoted. |
| `...` | optional arguments to `FUN`. |

#### 特点

矩阵or数组 -&gt; `apply()` -&gt; **矩阵或向量**

#### 实例

**对矩阵matrix使用apply\(\)**

```r
> m1 <- matrix(1:20, nrow=4)
> m1
     [,1] [,2] [,3] [,4] [,5]
[1,]    1    5    9   13   17
[2,]    2    6   10   14   18
[3,]    3    7   11   15   19
[4,]    4    8   12   16   20

# 对每一行求和，这里是聚合函数reduce，reduce生成vector
> apply(m1, MARGIN = 1, sum)
[1] 45 50 55 60

# 非聚合函数map，map生成matrix
# 先自定义函数
> f1 <- function(x){
+   x*100
+ }
> apply(m1, MARGIN = 1, f1)
     [,1] [,2] [,3] [,4]
[1,]  100  200  300  400
[2,]  500  600  700  800
[3,]  900 1000 1100 1200
[4,] 1300 1400 1500 1600
[5,] 1700 1800 1900 2000
> apply(m1, MARGIN = 2, f1)
     [,1] [,2] [,3] [,4] [,5]
[1,]  100  500  900 1300 1700
[2,]  200  600 1000 1400 1800
[3,]  300  700 1100 1500 1900
[4,]  400  800 1200 1600 2000
```

这里map虽然是作用于每一个元素，但是由于顺序不一样结果也不一样：

* MARGIN = 1时按照行，原dataframe每一行是5个，作用一行得到一个向量结果作为1列，最终有4列；
* MARGIN = 2时按照列，原dataframe每一列是4个，作用一列得到一个向量结果作为1列，最终有5列；

也就是说，无论是按照行还是按照列，每作用一行\(列\)得到的结果默认作为1列。

**对数组array使用apply\(\)**

```r
# 定义数组
> xname <- c("x1", "x2")
> yname <- c("y1", "y2", "y3")
> zname <- c("z1", "z2", "z3", "z4")
> a1 <- array(1:24, c(2, 3, 4), dimnames = list(xname, yname, zname))
> a1
, , z1

   y1 y2 y3
x1  1  3  5
x2  2  4  6

, , z2

   y1 y2 y3
x1  7  9 11
x2  8 10 12

, , z3

   y1 y2 y3
x1 13 15 17
x2 14 16 18

, , z4

   y1 y2 y3
x1 19 21 23
x2 20 22 24

# 聚合函数
# 求所有行的和
> apply(a1, MARGIN = 1, sum)
 x1  x2 
144 156 
# 求所有列的和
> apply(a1, MARGIN = 2, sum)
 y1  y2  y3 
 84 100 116 
# 求每个矩阵的和
> apply(a1, MARGIN = 3, sum)
 z1  z2  z3  z4 
 21  57  93 129
```

**对dataframe有时也可以用apply\(\)**

若dataframe各列均为数值型，则apply\(\)会自动将dataframe转化为matrix；存在非数值型数据，使用时将出错。

### lapply\(\)函数

#### 作用

对list中的各个元素使用函数。

#### 语法

`lapply(x, FUN, ...)`

x : 格式为列表的数据源

FUN : 函数，可以是系统自带的函数，也可以是自定义函数。

#### 特点

list -&gt; `lapply()` -&gt; **list**

#### 实例

```r
> l1 <- list(v <- 1:10, m <- matrix(1:20, 4))
> lapply(l1, sum)
[[1]]
[1] 55

[[2]]
[1] 210
```

### sapply\(\)函数

#### 作用

对list中的各个元素使用函数。

#### 语法

`sapply(x, FUN, ...)`

x : 格式为列表的数据源

FUN : 函数，可以是系统自带的函数，也可以是自定义函数。

#### 特点

list -&gt; `lapply()` -&gt; **vector**

`apply()`输出结果是vector或matrix，`lapply()`输出结果是list，`sapply()`输出结果是vector。

#### 实例

```r
> l1 <- list(v <- 1:10, m <- matrix(1:20, 4))
> sapply(l1, sum)
[1]  55 210
```

### tapply\(\)函数

tapply\(\)函数在分组数据处理中详述。

## 分组数据处理

相当于SQL中的`GROUP BY`。

先创建练习用的dataframe：

```r
> df <- data.frame(age = sample.int(10, 9, replace = TRUE) + 30,
+                  sex = sample(c("female", "male"), 9, replace = TRUE),
+                  education = sample(c("高中", "大专", "大学", "硕士"), 9, replace = TRUE),
+                  income = sample.int(5000, 9) + 3000)
> df
  age    sex education income
1  31 female      高中   7856
2  38 female      硕士   7087
3  32   male      大学   3766
4  34   male      大学   4581
5  39 female      高中   3409
6  35 female      硕士   4007
7  39   male      高中   7301
8  34   male      高中   4837
9  38   male      大专   5601
```

### tapply\(\)

`tapply(X, INDEX, FUN = NULL)`

| arguement | meaning |
| :--- | :--- |
| `X` | an **R** object for which a `split` method exists. Typically vector-like, allowing subsetting with `[`.  即要计算的变量 |
| `INDEX` | a `list` of one or more `factor`s, each of same length as `X`. The elements are coerced to factors by `as.factor`.  分组变量 |
| `FUN` | a function \(or name of a function\) to be applied, or `NULL`. In the case of functions like `+`, `%*%`, etc., the function name must be backquoted or quoted. If `FUN` is `NULL`, tapply returns a vector which can be used to subscript the multi-way array `tapply`normally produces. |

```r
# 一维汇总
> tapply(df$income, df$education, mean)
   大学    大专    高中    硕士 
4173.50 5601.00 5850.75 5547.00 

# 二维汇总，相当于EXCEL的数据透视表功能
> tapply(df$income, list(df$education,df$sex), mean)
     female   male
大学     NA 4173.5
大专     NA 5601.0
高中 5632.5 6069.0
硕士 5547.0     NA
```

### by\(\)

`by(data, INDICES, FUN)`

| arguement | meaning |
| :--- | :--- |
| `data` | an **R** object, normally a data frame, possibly a matrix. |
| `INDICES` | a factor or a list of factors, each of length `nrow(data)`. |
| `FUN` | a function to be applied to \(usually data-frame\) subsets of `data`. |

```r
> by(df$income, df$education, mean)
> by(df$income, list(df$education,df$sex), mean)
```

这里仅输出的格式不一样。

### aggregate\(\)

`aggregate(x~c1+c2, data, FUN)`：x为待求变量，c1、c2…为分组变量，间接好用。

```r
> aggregate(income ~ sex, df, sum)
     sex income
1 female  22359
2   male  26086
> aggregate(income ~ sex + education, df, sum)
     sex education income
1   male      大学   8347
2   male      大专   5601
3 female      高中  11265
4   male      高中  12138
5 female      硕士  11094
```

## 缺失值处理

### 缺失值 NA

* 数据源中缺少的数据；
* 由于记录错误产生了一些不符合常理的值，也需要将其定义为缺失值；
* 符号为`NA`。

### 处理缺失值方法

* 判别：`is.na()`
* 将`df`的`column`列的值为`value`的数据为`NA`：`df$column[df$column ==  value] <- NA`
* 将`df`中的所有缺失值替换为`value`：`df[is.na(df)] <- value`
* 排除缺失值：
  1. 有些函数自带的 `na.rm = TRUE` 选项，比如`mean`，`sum`函数；
  2. `na.omit()`可以将含有缺失值的那一整行都移除掉，此函数请慎用。

```r
# 判别缺失值
> ID <- paste("A", "00", 1:5, sep = "")
> age <- c(24, NA, 35, 19, -20)
> df1 <-data.frame(ID, age)
> df1
    ID age
1 A001  24
2 A002  NA
3 A003  35
4 A004  19
5 A005 -20
# 判断有误缺失值
> is.na(df1)
        ID   age
[1,] FALSE FALSE
[2,] FALSE  TRUE
[3,] FALSE FALSE
[4,] FALSE FALSE
[5,] FALSE FALSE
# 将负数值定义为NA
> df1$age[df1$age < 0] <- NA
> df1
    ID age
1 A001  24
2 A002  NA
3 A003  35
4 A004  19
5 A005  NA
# 将NA值替换为均值
> df1[is.na(df1)] <- mean(df1$age, na.rm = TRUE) # 注意这里要用na.rm = TRUE参数定义
> df1
    ID age
1 A001  24
2 A002  26
3 A003  35
4 A004  19
5 A005  26
# 删除NA行
> na.omit(df1)
    ID age
1 A001  24
3 A003  35
4 A004  19
5 A005 -20
```

## 数据类型转换函数

| 判断 |  | 转换 |
| :--- | :--- | :--- |
| is.numeric\(\) | 数值 | as.numeric\(\) |
| is.character\(\) | 字符 | as.character\(\) |
| Is.factor\(\) | 因子 | as.factor\(\) |
| is.vector\(\) | 向量 | as.vector\(\) |
| is.matrix\(\) | 矩阵 | as.matrix\(\) |
| is.data.frame\(\) | 数据框 | as.data.frame\(\) |

