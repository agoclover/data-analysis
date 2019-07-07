# notes\_R\_4\_normal\_distribution

## 生成正态分布数据

### rnorm\(\)

`rnorm(n, mean, sd)`

```r
# 模拟生成正态分布，μ=7000，σ=2000，数目为10000的男性毕业收入
> data1 <- rnorm(10000, 7000, 2000)
> data1 <- as.data.frame(data1)
> data1[, 2] <- "male"
> names(data1) <- c("income", "gender")
> head(data1)
    income gender
1 4053.738   male
2 9134.338   male
3 6738.822   male
4 7531.865   male
5 4475.896   male
6 7096.164   male

# 模拟生成正态分布，μ=5000，σ=2000，数目为10000的男性毕业收入data1 <- rnorm(10000, 7000, 2000)
> data2 <- rnorm(10000, 5000, 2000)
> data2 <- as.data.frame(data2)
> data2[, 2] <- "female"
> names(data2) <- c("income", "gender")
> head(data2)
     income gender
1 2577.5927 female
2  562.3029 female
3 5770.2223 female
4 2391.1798 female
5 5911.0615 female
6 7886.5115 female

# 将男性女性数据组合起来
> newdata <- rbind(data1, data2)
```

## 概率密度图

```r
ggplot(newdata, aes(income)) +
  geom_histogram(stat = "density") +
  facet_grid(gender~.)
```

作图结果如下：

![image-20190611232946021](http://strawberryamoszc.oss-cn-shanghai.aliyuncs.com/2019-06-11-152949.png)

## 检验正态分布

|  | qq图检验 | 夏皮罗检验 | KS检验 |
| :--- | :--- | :--- | :--- |
| 适用场景 | 感性的初步判断 | 数量在5000以内 | 任意 |
| 优点 | 简单直观 | 简单精确 | 适用于任意数量 |
| 缺点 | 不精确 | 有数量限制 | 操作步骤相对多 |
| 函数 | qqnorm, qqline | shapiro.test | ks.test |

### qq图检验

#### qq图

`qqnorm(data1[, 1])`

![image-20190611233916455](http://strawberryamoszc.oss-cn-shanghai.aliyuncs.com/2019-06-11-153918.png)

#### qqline

`qqline(data1[, 1], col="blue")`

![image-20190611233954352](http://strawberryamoszc.oss-cn-shanghai.aliyuncs.com/2019-06-11-153956.png)

### 夏皮罗检验

当w接近1，p&gt;0.05时，说明数据符合正态分布：

```r
> data3 <- sample(data1[, 1], 5000)
> shapiro.test(data3)

    Shapiro-Wilk normality test

data:  data3
W = 0.99981, p-value = 0.9542
```

我们这里假设符合，P-value = 0.9542，说明结果显著。接下来我们做1000次试验，看看这1000次中`p-value < 0.05`有多少：

```r
> i <- 0
> p_data <- data.frame(c(0))
> names(p_data) <- "p"
> while(i <= 1000){
+   i <- i + 1
+   data3 <- sample(data1[, 1], 5000)
+   p <- shapiro.test(data3)
+   p_data[i,] <- p$p.value
+ }
> nrow(subset(p_data, p < 0.05))
[1] 2
```

或：

```r
> p_data <- c()
> for(i in 1:1000){
+   data3 <- sample(data1[, 1], 5000)
+   p <- shapiro.test(data3)
+   p_data <- append(p_data, p$p.value)
+ }
> length(p_data[p_data < 0.05])
[1] 2
```

1000次只有2次小于0.05，所以结果是显著的。

### ks检验

D接近0，且p&gt;0.5时，说明数据符合正态分布

```r
> ks.test(
+   data1[, 1],
+   rnorm(10000, mean = mean(data1[, 1]), sd = sd(data1[, 1]))
+ )

    Two-sample Kolmogorov-Smirnov test

data:  data1[, 1] and rnorm(10000, mean = mean(data1[, 1]), sd = sd(data1[, 1]))
D = 0.0071, p-value = 0.9626
alternative hypothesis: two-sided
```

## 正态分布四个函数

| Name | Function | Aim |
| :--- | :--- | :--- |
| rnorm | 返回n个正态分布随机数构成的向量 | 随机生成正态分布数据 |
| qnorm | 返回给定概率对应的值 | 求出概率对应的x值 |
| dnorm | 返回正态分布概率密度函数 | 求出某个点的概率密度 |
| pnorm | 返回正态分布的分布函数 | 求出一段区间的累计概率值 |

```r
> # μ=7000，σ=2000
> # 1. 若甲的收入>80%的人，那么它的收入是多少？
> qnorm(0.8, 7000, 2000)
[1] 8683.242
> ?qnorm
> # 2. 已知乙收入8500左右的概率密度是多少？（概率密度density）
> dnorm(8500, 7000, 2000)
[1] 0.0001505687
> # 3. 已知丙同学收入9000，他的收入会比多少人高？（累积概率）
> pnorm(9000, 7000, 2000)
[1] 0.8413447
```

## 标准正态分布

### 标准正态分布

标准正态分布概率密度函数：φ\(x\)，相当于`dnorm()`；

分布函数：Φ\(x\)，相当于`pnorm()`。

Φ\(-x\)=1-Φ\(x\)

### 标准化

```r
> zdata1 <- (data$income - mean(data$income)) / sd(data$income)
> library("ggplot2")
> ggplot(data = NULL, aes(zdata1)) +
+   geom_histogram(stat = "density")
```

### 标准正态分布表

| μ | ＋0.00 | ＋0.01 | ＋0.02 | ＋0.03 | ＋0.04 | ＋0.05 | ＋0.06 | ＋0.07 | ＋0.08 | ＋0.09 |
| :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: |
| 0.0 | 0.5000 | 0.5040 | 0.5080 | 0.5120 | 0.5160 | 0.5199 | 0.5239 | 0.5279 | 0.5319 | 0.5359 |
| 0.1 | 0.5398 | 0.5438 | 0.5478 | 0.5517 | 0.5557 | 0.5596 | 0.5636 | 0.5675 | 0.5714 | 0.5753 |
| 0.2 | 0.5793 | 0.5832 | 0.5871 | 0.5910 | 0.5948 | 0.5987 | 0.6026 | 0.6064 | 0.6103 | 0.6141 |
| 0.3 | 0.6179 | 0.6217 | 0.6255 | 0.6293 | 0.6331 | 0.6368 | 0.6406 | 0.6443 | 0.6480 | 0.6517 |
| 0.4 | 0.6554 | 0.6591 | 0.6628 | 0.6664 | 0.6700 | 0.6736 | 0.6772 | 0.6808 | 0.6844 | 0.6879 |
| 0.5 | 0.6915 | 0.6950 | 0.6985 | 0.7019 | 0.7054 | 0.7088 | 0.7123 | 0.7157 | 0.7190 | 0.7224 |
| 0.6 | 0.7257 | 0.7291 | 0.7324 | 0.7357 | 0.7389 | 0.7422 | 0.7454 | 0.7486 | 0.7517 | 0.7549 |
| 0.7 | 0.7580 | 0.7611 | 0.7642 | 0.7673 | 0.7704 | 0.7734 | 0.7764 | 0.7794 | 0.7823 | 0.7852 |
| 0.8 | 0.7881 | 0.7910 | 0.7939 | 0.7967 | 0.7995 | 0.8023 | 0.8051 | 0.8078 | 0.8106 | 0.8133 |
| 0.9 | 0.8159 | 0.8186 | 0.8212 | 0.8238 | 0.8264 | 0.8289 | 0.8315 | 0.8340 | 0.8365 | 0.8389 |
| 1.0 | 0.8413 | 0.8438 | 0.8461 | 0.8485 | 0.8508 | 0.8531 | 0.8554 | 0.8577 | 0.8599 | 0.8621 |
| 1.1 | 0.8643 | 0.8665 | 0.8686 | 0.8708 | 0.8729 | 0.8749 | 0.8770 | 0.8790 | 0.8810 | 0.8830 |
| 1.2 | 0.8849 | 0.8869 | 0.8888 | 0.8907 | 0.8925 | 0.8944 | 0.8962 | 0.8980 | 0.8997 | 0.9015 |
| 1.3 | 0.9032 | 0.9049 | 0.9066 | 0.9082 | 0.9099 | 0.9115 | 0.9131 | 0.9147 | 0.9162 | 0.9177 |
| 1.4 | 0.9192 | 0.9207 | 0.9222 | 0.9236 | 0.9251 | 0.9265 | 0.9279 | 0.9292 | 0.9306 | 0.9319 |
| 1.5 | 0.9332 | 0.9345 | 0.9357 | 0.9370 | 0.9382 | 0.9394 | 0.9406 | 0.9418 | 0.9429 | 0.9441 |
| 1.6 | 0.9452 | 0.9463 | 0.9474 | 0.9484 | 0.9495 | 0.9505 | 0.9515 | 0.9525 | 0.9535 | 0.9545 |
| 1.7 | 0.9554 | 0.9564 | 0.9573 | 0.9582 | 0.9591 | 0.9599 | 0.9608 | 0.9616 | 0.9625 | 0.9633 |
| 1.8 | 0.9641 | 0.9649 | 0.9656 | 0.9664 | 0.9671 | 0.9678 | 0.9686 | 0.9693 | 0.9699 | 0.9706 |
| 1.9 | 0.9713 | 0.9719 | 0.9726 | 0.9732 | 0.9738 | 0.9744 | 0.9750 | 0.9756 | 0.9761 | 0.9767 |
| 2.0 | 0.9772 | 0.9778 | 0.9783 | 0.9788 | 0.9793 | 0.9798 | 0.9803 | 0.9808 | 0.9812 | 0.9817 |
| 2.1 | 0.9821 | 0.9826 | 0.9830 | 0.9834 | 0.9838 | 0.9842 | 0.9846 | 0.9850 | 0.9854 | 0.9857 |
| 2.2 | 0.9861 | 0.9864 | 0.9868 | 0.9871 | 0.9875 | 0.9878 | 0.9881 | 0.9884 | 0.9887 | 0.9890 |
| 2.3 | 0.9893 | 0.9896 | 0.9898 | 0.9901 | 0.9904 | 0.9906 | 0.9909 | 0.9911 | 0.9913 | 0.9916 |

## 抽样分布

### 总体为正态分布，统计量为样本均值

```r
# 正态总体数据
data1 <- rnorm(10000, 7000, 2000)
data1 <- as.data.frame(data1)
data1[, 2] <- "male"
names(data1) <- c("income", "gender")
# 非正态总体数据
data2 <- as.data.frame(sample.int(100, 10000, replace = TRUE))
names(data2) <- "rnum"

# 抽样参数
n <- 10 # 样本容量
f <- mean # 统计量
c_data <- c() # 保存抽样后的统计量
times <- 1000

# 选取数据
data <- data1

# 进行1000次抽样
for(i in 1:times){
  sampledata <- sample(data[,1], n)
  c_data <- append(c_data, f(sampledata))
}

# 绘出统统计量分布
ggplot(data=NULL, aes(c_data)) +
  geom_histogram(stat = "density")
```

![image-20190615103531276](http://strawberryamoszc.oss-cn-shanghai.aliyuncs.com/2019-06-15-023533.png)

检验样本统计量分布：

```r
> # 检测分布
> ## 夏皮罗检测
> shapiro.test(c_data)

    Shapiro-Wilk normality test

data:  c_data
W = 0.99794, p-value = 0.26

> ## ks检验
> ks.test(c_data, 
+         rnorm(times, mean = mean(data[,1]), (sd(data[,1]) / sqrt(n))))

    Two-sample Kolmogorov-Smirnov test

data:  c_data and rnorm(times, mean = mean(data[, 1]), (sd(data[, 1])/sqrt(n)))
D = 0.056, p-value = 0.08691
alternative hypothesis: two-sided
```

通过检测可以发现，样本均值分布满足正态分布。

同样，当我们将n的值改为10时，虽然样本容量减少，但样本均值仍服从正态分布。

### 总体为非正态总体，统计量为样本均值

当我们将数据换成非正态分布数据:

```r
# 非正态分布数据
> data2 <- as.data.frame(sample.int(100, 10000, replace = TRUE))
> names(data2) <- "rnum"
```

样本容量取50时，得到的结果依然具有显著性:

```r
> ks.test(c_data, rnorm(times, mean = mean(data[,1]), (sd(data[,1]) / sqrt(n))))

    Two-sample Kolmogorov-Smirnov test

data:  c_data and rnorm(times, mean = mean(data[, 1]), (sd(data[, 1])/sqrt(n)))
D = 0.032, p-value = 0.6852
alternative hypothesis: two-sided

Warning message:
In ks.test(c_data, rnorm(times, mean = mean(data[, 1]), (sd(data[,  :
  并列的时候P-值将近似
```

注意，这里因为`data2`中数据重复比较多，所以`c_data`重复也很多，而重复数据对KS检验是会发出警告的，如上。所以我们可以考虑，使用`jitter()`函数给`c_data`加一个小的数据扰动，从而使`c_data`不再存在重复数据，这样就不会有重复：

```r
> ks.test(jitter(c_data), rnorm(times, mean = mean(data[,1]), (sd(data[,1]) / sqrt(n))))

    Two-sample Kolmogorov-Smirnov test

data:  jitter(c_data) and rnorm(times, mean = mean(data[, 1]), (sd(data[, 1])/sqrt(n)))
D = 0.032, p-value = 0.6852
alternative hypothesis: two-sided
```

> 这一小节内容参考[炼数成金](http://f.dataguru.cn/thread-120859-1-1.html)。

