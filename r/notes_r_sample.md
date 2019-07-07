# notes\_R\_sample

## set.seed\(\)

> 本小段引用自[CSDN](https://blog.csdn.net/vencent_cy/article/details/50350020%20)。

`set.seed()` is the recommended way to specify seeds.

`set.seed()`用于设定随机数种子，一个特定的种子可以产生一个特定的伪随机序列，这个函数的主要目的，是让你的模拟能够可重复出现，因为很多时候我们需要取随机数，但这段代码再跑一次的时候，结果就不一样了，如果需要重复出现同样的模拟结果的话，就可以用`set.seed()`。在调试程序或者做展示的时候，结果的可重复性是很重要的，所以随机数种子也就很有必要。

也可以简单地理解为括号里的数只是一个编号而已，例如`set.seed(100)`不应将括号里的数字理解成“100”，而是应该理解成“编号为一零零的随机数发生”，下一次再模拟可以采用二零零（200）或者（111）等不同的编号即可，编号设定基本可以随意。

```r
# 随机生成10个随机数 
> x <- rnorm(10)
> x 
[1] 0.3897943 -1.2080762 -0.3636760 -1.6266727 -0.2564784 1.1017795 0.7557815 
[8] -0.2382336 0.9874447 0.7413901 
# 再次随机生成10个随机数 
> x <- rnorm(10)
> x 
[1] 0.08934727 -0.95494386 -0.19515038 0.92552126 0.48297852 -0.59631064 -2.18528684 
[8] -0.67486594 -2.11906119 -1.26519802 

# 设定种子
> set.seed(5) 
# 在设定种子的前提下生成10个随机数 
> x <- rnorm(10) 
> x 
[1] -0.84085548 1.38435934 -1.25549186 0.07014277 1.71144087 -0.60290798 -0.47216639 
[8] -0.63537131 -0.28577363 0.13810822 
# 设定种子 
> set.seed(5) 
> y <- rnorm(10) 
> y 
[1] -0.84085548 1.38435934 -1.25549186 0.07014277 1.71144087 -0.60290798 -0.47216639 
[8] -0.63537131 -0.28577363 0.13810822 
> x == y 
[1] TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE
```

## rep\(\)

`rep(x, each, time, length)`

用以重复元素（数字、字符串等）举例：

```r
# 每个元素依次重复2次
> rep(1:4, each=2)
[1] 1 1 2 2 3 3 4 4
# 每个元素依次重复1 2 3 4次
> rep(1:4, times=1:4)
 [1] 1 2 2 3 3 3 4 4 4 4
# 由于顺序是先each再times，所以同时写相当于第一个的结果再加一个参数times
> rep(1:4, each=2, times=1:8)
 [1] 1 1 1 2 2 2 2 2 2 2 3 3 3 3 3 3 3 3 3 3 3 4 4 4 4 4 4 4 4 4 4 4 4 4 4 4
# 使用len选取最终长度，不够则循环补齐
> rep(1:4, time=1:4, len=15)
 [1] 1 2 3 4 1 2 3 4 1 2 3 4 1 2 3
```

## sample\(\)

`sample(x, size, replace = FALSE, prob = NULL)`

```r
# sample()来1~100随机抽样
sample(100, 10, replace = TRUE)
# 某几个值抽样
sample(c(1, 5, 10), 5, replace = TRUE)
# 总体是非数值
sample(LETTERS[1:20], 15, replace = TRUE)
# 使用sample.int()来对数字抽样
sample.int(100, 10, replace = TRUE)
```

若`size`缺省，则`size`等于总体个数：

```r
> set.seed(1)
> sample <- sample(1:10^6, replace = TRUE)
> p <- 1 - sum(unique(sample) %in% 1:10^6) / 10^6
> 1 / p
[1] 2.71666
```

## 随机抽样

### 1 导入数据

```r
getwd()
setwd("/Users/amos/Library/CloudStorage/iCloud Drive/Desktop/data_analysis/R/datafiles")
df <- read.csv("titanic_data.csv")
df <- subset(df, select = -c(Name, Ticket)) # 去除无用的Name和Ticket列
df <- df[order(df$PassengerId),] # 以PassengerID对数据进行排序
```

### 2 对数据集的序号进行抽样

```r
index <- sample(1:nrow(df), 60, replace = FALSE)
```

### 3 根据抽样得到的序号来提取数据

```r
sample_df <- df[index, ]
```

## 分层抽样

`sampling`包中的函数，用于分层抽样。

### 函数

`strata(data, stratanames=NULL, size, method=c("srswor","srswr","poisson","systematic"), pik,description=FALSE)`

### 参数

| Arguements | Meaning |
| :--- | :--- |
| `data` | data frame or data matrix; its number of rows is N, the population size.  dataframe或矩阵；行数\(总体容量\)=N。 |
| `stratanames` | vector of stratification variables.  分层变量的向量。 |
| `size` | vector of stratum sample sizes \(in the order in which the strata are given in the input data set\).  分层样本容量的向量。 |
| `method` | method to select units; the following methods are implemented: simple random sampling without replacement \(srswor\), simple random sampling with replacement \(srswr\), Poisson sampling \(poisson\), systematic sampling \(systematic\); if "method" is missing, the default method is "srswor".  选择单位的方法; 实现了以下方法：无替换的简单随机抽样（srswor），带替换的简单随机抽样（srswr），泊松抽样（poisson），系统抽样（systematic）; 如果缺少`method`，则默认方法为`srswor`。 |
| `pik` | vector of inclusion probabilities or auxiliary information used to compute them; this argument is only used for unequal probability sampling \(Poisson and systematic\). If an auxiliary information is provided, the function uses the [inclusionprobabilities](http://127.0.0.1:15926/help/library/sampling/help/inclusionprobabilities) function for computing these probabilities. 包含概率的向量或用于计算它们的辅助信息; 该参数仅用于不等概率抽样（泊松和系统）。 如果提供了辅助信息，则该函数使用[inclusionprobabilities](http://127.0.0.1:15926/help/library/sampling/help/inclusionprobabilities)函数来计算这些概率。 |
| `description` | a message is printed if its value is TRUE; the message gives the number of selected units and the number of the units in the population. By default, the value is FALSE. 如果消息的值为TRUE，则打印该消息; 该消息给出了所选单位的数量和总体中的单位数。 默认情况下，该值为FALSE。 |

### 细节

在应用函数之前，应按stratanames参数中给出的列按升序对数据进行排序。 例如，使用数据`[order(data$state，data$region), ]`。

### 值

该函数生成一个对象，其中包含以下信息：

| 内容 | 解释 |
| :--- | :--- |
| ID\_unit | 所选单位的标识符 |
| 分层 | 单位层级 |
| 概率 | 单位包含的概率 |

### 举例

#### 例1

```r
# Example from An and Watts (New SAS procedures for Analysis of Sample Survey Data)
# generates artificial data (a 235X3 matrix with 3 columns: state, region, income).
# the variable "state" has 2 categories ('nc' and 'sc'). 
# the variable "region" has 3 categories (1, 2 and 3).
# the sampling frame is stratified by region within state.
# the income variable is randomly generated
data=rbind(matrix(rep("nc",165),165,1,byrow=TRUE),matrix(rep("sc",70),70,1,byrow=TRUE))
data=cbind.data.frame(data,c(rep(1,100), rep(2,50), rep(3,15), rep(1,30),rep(2,40)),
1000*runif(235)) # runif()是产生0~1之间的随机数
names(data)=c("state","region","income")
# computes the population stratum sizes
table(data$region,data$state)
# not run
#     nc  sc
#  1 100  30
#  2  50  40
#  3  15   0
# there are 5 cells with non-zero values
# one draws 5 samples (1 sample in each stratum)
# the sample stratum sizes are 10,5,10,4,6, respectively
# the method is 'srswor' (equal probability, without replacement)
s=strata(data,c("region","state"),size=c(10,5,10,4,6), method="srswor")
# extracts the observed data
getdata(data,s)
# see the result using a contigency table
table(s$region,s$state)
```

#### 例2

```r
# The same data as in Example 1
# the method is 'systematic' (unequal probability, without replacement)
# the selection probabilities are computed using the variable 'income'
s=strata(data,c("region","state"),size=c(10,5,10,4,6), method="systematic",pik=data$income)
# extracts the observed data
getdata(data,s)
# see the result using a contigency table
table(s$region,s$state)
```

#### 例3

```r
# Uses the 'swissmunicipalities' data as population for drawing a sample of units
data(swissmunicipalities)
# the variable 'REG' has 7 categories in the population
# it is used as stratification variable
# Computes the population stratum sizes
table(swissmunicipalities$REG)
# do not run
#  1   2   3   4   5   6   7 
# 589 913 321 171 471 186 245 
# sort the data to obtain the same order of the regions in the sample
data=swissmunicipalities
data=data[order(data$REG),]
# the sample stratum sizes are given by size=c(30,20,45,15,20,11,44)
# 30 units are drawn in the first stratum, 20 in the second one, etc.
# the method is simple random sampling without replacement 
# (equal probability, without replacement)
st=strata(data,stratanames=c("REG"),size=c(30,20,45,15,20,11,44), method="srswor")
# extracts the observed data
getdata(data, st)
# see the result using a contingency table
table(st$REG)
```

## 大数据情况下的抽样

### 随机抽样

以搜狗用户搜索日志数据为例：

```r
# 大数据情况下爱的随机抽样
# 导入文件，只读
data <- file("SogouQ.reduced", open = "r")
# 读取一行（第一次运行即读取第一行）
line <- readLines(data, n=1)
line

# 设定抽样比例
p <- 0.01
i <- 0
# 定义指针
count_row <- 0
count_p <- 0
# 定义抽样取出的数据
df <- data.frame(c(0))
# 循环
while (length(line) != 0) {
  c <- runif(1)
  count_row <- count_row + 1
  if(c < p){
    print(line)
    i <- i + 1
    count_p <- count_p + 1
    df[i,] <- line
  }
  line <- readLines(data, n=1) # 这里写在后面是因为前面已经有了一行数据
}
```

注意以下几点：

1. 读取文件要只读参数，即`open = "r"`，否则，在`readLine()`时，会出现永远只读取第一行的情况；
2. 要在循环之前定义`dataframe`，如这里的`df <- data.frame(c(0))`，否则循环内是找不到对象的。

### grep\(\)

```r
> grep("a", "abc")
[1] 1
> grep("a", "aabc")
[1] 1
> grep("d", "aabc")
integer(0)
```

### 分层抽样

```r
# 读取数据
data <- file("titanic_data.csv", open = "r")
line <- readLines(data, n=1)
line

# 参数设定
p <- 0.1
keyword <- "female"
keycount <- 0
pcount <- 0
i <- 0
df <- data.frame(c(0))

# 开始循环和抽样
while (length(line != 0)) {
  c <- runif(1)
  if(length(grep(keyword, line)) != 0){
    keycount <- keycount + 1
    if(c < p){
      pcount <- pcount + 1
      i <- i + 1
      print(line)
      df[i,] <- line
    }
  }
  line <- readLines(data, n=1)
}
```

## 描述统计

### summary\(\)

```r
> # 使用mtcars数据集
> mtcars
                     mpg cyl  disp  hp drat    wt  qsec vs am gear carb
Mazda RX4           21.0   6 160.0 110 3.90 2.620 16.46  0  1    4    4
Mazda RX4 Wag       21.0   6 160.0 110 3.90 2.875 17.02  0  1    4    4
Datsun 710          22.8   4 108.0  93 3.85 2.320 18.61  1  1    4    1
Hornet 4 Drive      21.4   6 258.0 110 3.08 3.215 19.44  1  0    3    1
Hornet Sportabout   18.7   8 360.0 175 3.15 3.440 17.02  0  0    3    2
Valiant             18.1   6 225.0 105 2.76 3.460 20.22  1  0    3    1
Duster 360          14.3   8 360.0 245 3.21 3.570 15.84  0  0    3    4
Merc 240D           24.4   4 146.7  62 3.69 3.190 20.00  1  0    4    2
Merc 230            22.8   4 140.8  95 3.92 3.150 22.90  1  0    4    2
Merc 280            19.2   6 167.6 123 3.92 3.440 18.30  1  0    4    4
Merc 280C           17.8   6 167.6 123 3.92 3.440 18.90  1  0    4    4
Merc 450SE          16.4   8 275.8 180 3.07 4.070 17.40  0  0    3    3
Merc 450SL          17.3   8 275.8 180 3.07 3.730 17.60  0  0    3    3
Merc 450SLC         15.2   8 275.8 180 3.07 3.780 18.00  0  0    3    3
Cadillac Fleetwood  10.4   8 472.0 205 2.93 5.250 17.98  0  0    3    4
Lincoln Continental 10.4   8 460.0 215 3.00 5.424 17.82  0  0    3    4
Chrysler Imperial   14.7   8 440.0 230 3.23 5.345 17.42  0  0    3    4
Fiat 128            32.4   4  78.7  66 4.08 2.200 19.47  1  1    4    1
Honda Civic         30.4   4  75.7  52 4.93 1.615 18.52  1  1    4    2
Toyota Corolla      33.9   4  71.1  65 4.22 1.835 19.90  1  1    4    1
Toyota Corona       21.5   4 120.1  97 3.70 2.465 20.01  1  0    3    1
Dodge Challenger    15.5   8 318.0 150 2.76 3.520 16.87  0  0    3    2
AMC Javelin         15.2   8 304.0 150 3.15 3.435 17.30  0  0    3    2
Camaro Z28          13.3   8 350.0 245 3.73 3.840 15.41  0  0    3    4
Pontiac Firebird    19.2   8 400.0 175 3.08 3.845 17.05  0  0    3    2
Fiat X1-9           27.3   4  79.0  66 4.08 1.935 18.90  1  1    4    1
Porsche 914-2       26.0   4 120.3  91 4.43 2.140 16.70  0  1    5    2
Lotus Europa        30.4   4  95.1 113 3.77 1.513 16.90  1  1    5    2
Ford Pantera L      15.8   8 351.0 264 4.22 3.170 14.50  0  1    5    4
Ferrari Dino        19.7   6 145.0 175 3.62 2.770 15.50  0  1    5    6
Maserati Bora       15.0   8 301.0 335 3.54 3.570 14.60  0  1    5    8
Volvo 142E          21.4   4 121.0 109 4.11 2.780 18.60  1  1    4    2
> # 单列
> data <- mtcars$cyl
> s1 <- summary(data)
> s1[["Min."]]
[1] 4

> # 多列
> data2 <- mtcars[c("mpg", "cyl", "wt")]
> s2 <- summary(data2)
> s2
      mpg             cyl              wt       
 Min.   :10.40   Min.   :4.000   Min.   :1.513  
 1st Qu.:15.43   1st Qu.:4.000   1st Qu.:2.581  
 Median :19.20   Median :6.000   Median :3.325  
 Mean   :20.09   Mean   :6.188   Mean   :3.217  
 3rd Qu.:22.80   3rd Qu.:8.000   3rd Qu.:3.610  
 Max.   :33.90   Max.   :8.000   Max.   :5.424  
> s2[, 2][1]

"Min.   :4.000  " 
> as.numeric(unlist(strsplit(s2, ":"))[2])
[1] 10.4
```

### stat.desc\(\)

来自`pastecs`包。

```r
> s3 <- stat.desc(data2)
> s3
                     mpg         cyl          wt
nbr.val       32.0000000  32.0000000  32.0000000
nbr.null       0.0000000   0.0000000   0.0000000
nbr.na         0.0000000   0.0000000   0.0000000
min           10.4000000   4.0000000   1.5130000
max           33.9000000   8.0000000   5.4240000
range         23.5000000   4.0000000   3.9110000
sum          642.9000000 198.0000000 102.9520000
median        19.2000000   6.0000000   3.3250000
mean          20.0906250   6.1875000   3.2172500
SE.mean        1.0654240   0.3157093   0.1729685
CI.mean.0.95   2.1729465   0.6438934   0.3527715
var           36.3241028   3.1895161   0.9573790
std.dev        6.0269481   1.7859216   0.9784574
coef.var       0.2999881   0.2886338   0.3041285
> s3[,2][4]
[1] 4
```

### 频数统计

```r
> table(mtcars$cyl, mtcars$vs, dnn = c("cyl", "vs"))
   vs
cyl  0  1
  4  1 10
  6  3  4
  8 14  0
```

