# notes\_R\_5\_parameter\_estimation\_hypothesis \_testing

## 四个重要函数

| Name | Function | Aim |
| :--- | :--- | :--- |
| rnorm | 返回n个正态分布随机数构成的向量 | 随机生成正态分布数据 |
| qnorm | 返回给定概率对应的值 | 求出概率（不是概率密度）对应的x值 |
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

## 参数估计

### 点估计

### 区间估计

以下为求正态总体的均值的区间估计，已知总体标准差为1：

```r
# 设定统计参数
n <- 50 # 样本容量
α <- 0.05 # 弃真水平
people_sd <- 1 # 假设已知总体标准差
sig <- 2 # 精确度


# 样本
set.seed(1)
sample <- rnorm(n, 0, people_sd) # 假设我们不知道总体均值
sample_mean <- mean(sample)

# z值
z <- qnorm(α / 2)

# 区间半径
radius <- z * (people_sd / sqrt(n))

# 置信区间，并保留2位有效数字
in_l <- round(sample_mean + radius, sig)
in_r <- round(sample_mean - radius, sig)

# 求出置信区间
print(paste("置信区间为:[", in_l, ",", in_r, "]", sep = ""))
```

结果为：`[1] "置信区间为:[-0.18,0.38]"`

## 假设检验

### Z-test

正态总体$$N\left( μ,{ σ }^{ 2 } \right)$$当σ^2^已知时关于μ的检验问题，我们利用统计量$$Z=\frac { \bar { X } -{ μ }_{ 0 } }{ { σ }/{ \sqrt { n } } }$$来确定拒绝域的，这种检验法常称为**Z检验法**。

`BSDA`包中的z.test {BSDA}函数进行z检验。

#### Description

This function is based on the standard normal distribution and creates confidence intervals and tests hypotheses for both one and two sample problems.

这个函数是基于标准正态分布，用以对单样本或双样本问题，创建置信区间和进行假设检验。

#### Usage

```text
z.test(x, y = NULL, alternative = "two.sided", mu = 0, sigma.x = NULL,
  sigma.y = NULL, conf.level = 0.95)
```

#### Arguments

| Arguments | Meaning |
| :--- | :--- |
| `x` | numeric vector; `NA`s and `Inf`s are allowed but will be removed. |
| `y` | numeric vector; `NA`s and `Inf`s are allowed but will be removed. |
| `alternative` | character string, one of `"greater"`, `"less"` or `"two.sided"`, or the initial letter of each, indicating the specification of the alternative hypothesis. For one-sample tests, `alternative` refers to the true mean of the parent population in relation to the hypothesized value `mu`. For the standard two-sample tests, `alternative` refers to the difference between the true population mean for `x` and that for `y`, in relation to `mu`. 备择假设，输入字符串`"greater"`, `"less"` 或 `"two.sided"`，或起始字符。单样本来说，备择假设指总体均值；双样本来说，指均值差。比如，选择`"greater"`，就表示零假设是`mu <= number` |
| `mu` | a single number representing the value of the mean or difference in means specified by the null hypothesis. 一个数字，代表零假设指定的均值或均值差。具体单边假设还是双边假设由`alternative`参数反着来决定。 |
| `sigma.x` | a single number representing the population standard deviation for `x` x总体标准差 |
| `sigma.y` | a single number representing the population standard deviation for `y` y总体标准差 |
| `conf.level` | confidence level for the returned confidence interval, restricted to lie between zero and one 置信水平，相当于`1-α` |

#### Details

如果缺省`y`，则是单样本均值`mu`的假设检验，这时应该已知`x`的总体标准差；单边假设或双边假设由`alternative`参数反着来决定。

如果`x`与`y`均不缺省，则是双样本均值差`mu`的假设检验，这时应该已知`x`和`y`的总体标准差；单边假设或双边假设由`alternative`参数反着来决定。

结果给出了z值，`p-vlaue`，备择假设的内容，置信区间，样本均值等，可以通过`p-value`来判断是否拒绝原假设。

这里`conf.level`仅是针对于置信区间来说的，不同的置信水平会影响置信区间的长度，但并不改变`p-vlaue`。

### t-Test

#### Description

Performs one and two sample t-tests on vectors of data.

#### Usage

```text
t.test(x, ...)

## Default S3 method:
t.test(x, y = NULL,
       alternative = c("two.sided", "less", "greater"),
       mu = 0, paired = FALSE, var.equal = FALSE,
       conf.level = 0.95, ...)

## S3 method for class 'formula'
t.test(formula, data, subset, na.action, ...)
```

#### Arguments

| Arguments | Meaning |
| :--- | :--- |
| `x` | a \(non-empty\) numeric vector of data values. |
| `y` | an optional \(non-empty\) numeric vector of data values. |
| `alternative` | a character string specifying the alternative hypothesis, must be one of `"two.sided"` \(default\), `"greater"` or `"less"`. You can specify just the initial letter. |
| `mu` | a number indicating the true value of the mean \(or difference in means if you are performing a two sample test\). |
| `paired` | a logical indicating whether you want a paired t-test. |
| `var.equal` | a logical variable indicating whether to treat the two variances as being equal. If `TRUE` then the pooled variance is used to estimate the variance otherwise the Welch \(or Satterthwaite\) approximation to the degrees of freedom is used. |
| `conf.level` | confidence level of the interval. |
| `formula` | a formula of the form `lhs ~ rhs` where `lhs` is a numeric variable giving the data values and `rhs` a factor with two levels giving the corresponding groups. |
| `data` | an optional matrix or data frame \(or similar: see `model.frame`\) containing the variables in the formula `formula`. By default the variables are taken from `environment(formula)`. |
| `subset` | an optional vector specifying a subset of observations to be used. |
| `na.action` | a function which indicates what should happen when the data contain `NA`s. Defaults to `getOption("na.action")`. |
| `...` | further arguments to be passed to or from methods. |

#### Details

The formula interface is only applicable for the 2-sample tests.

`alternative = "greater"` is the alternative that `x` has a larger mean than `y`.

If `paired` is `TRUE` then both `x` and `y` must be specified and they must be the same length. Missing values are silently removed \(in pairs if `paired` is `TRUE`\). If `var.equal`is `TRUE` then the pooled estimate of the variance is used. By default, if `var.equal` is `FALSE` then the variance is estimated separately for both groups and the Welch modification to the degrees of freedom is used.

If the input data are effectively constant \(compared to the larger of the two means\) an error is generated.

### χ^2^-test

Pearson's Chi-squared Test for Count Data

#### Description

`chisq.test` performs chi-squared contingency table tests and goodness-of-fit tests.

#### Usage

```text
chisq.test(x, y = NULL, correct = TRUE,
           p = rep(1/length(x), length(x)), rescale.p = FALSE,
           simulate.p.value = FALSE, B = 2000)
```

#### Arguments

| Arguments | Meaning |
| :--- | :--- |
| `x` | a numeric vector or matrix. `x` and `y` can also both be factors. |
| `y` | a numeric vector; ignored if `x` is a matrix. If `x` is a factor, `y`should be a factor of the same length. |
| `correct` | a logical indicating whether to apply continuity correction when computing the test statistic for 2 by 2 tables: one half is subtracted from all _\|O - E\|_ differences; however, the correction will not be bigger than the differences themselves. No correction is done if `simulate.p.value = TRUE`. |
| `p` | a vector of probabilities of the same length of `x`. An error is given if any entry of `p` is negative. |
| `rescale.p` | a logical scalar; if TRUE then `p` is rescaled \(if necessary\) to sum to 1. If `rescale.p` is FALSE, and `p` does not sum to 1, an error is given. |
| `simulate.p.value` | a logical indicating whether to compute p-values by Monte Carlo simulation. |
| `B` | an integer specifying the number of replicates used in the Monte Carlo test. |

### 正太总体-均值

#### 单个正太总体-均值

**方差已知-Z检验**

正态总体$$N\left( μ,{ σ }^{ 2 } \right)$$当σ^2^已知时关于μ的检验问题，我们利用统计量$$Z=\frac { \bar { X } -{ μ }_{ 0 } }{ { σ }/{ \sqrt { n } } }$$来确定拒绝域的，这种检验法常称为**Z检验法**。

**手动判断**

```r
> sample  # 样本
 [1] -0.62645381  0.18364332 -0.83562861  1.59528080  0.32950777 -0.82046838  0.48742905
 [8]  0.73832471  0.57578135 -0.30538839  1.51178117  0.38984324 -0.62124058 -2.21469989
[15]  1.12493092 -0.04493361 -0.01619026  0.94383621  0.82122120  0.59390132  0.91897737
[22]  0.78213630  0.07456498 -1.98935170  0.61982575 -0.05612874 -0.15579551 -1.47075238
[29] -0.47815006  0.41794156  1.35867955 -0.10278773  0.38767161 -0.05380504 -1.37705956
[36] -0.41499456 -0.39428995 -0.05931340  1.10002537  0.76317575 -0.16452360 -0.25336168
[43]  0.69696338  0.55666320 -0.68875569 -0.70749516  0.36458196  0.76853292 -0.11234621
[50]  0.88110773
> ### 需要输入的数据
> h0_μ <- 0  # 原假设
> people_sd <- 1  # 总体标准差
> α <- 0.05

> ### 检验过程
> n <- length(sample)  # 样本容量
> s_mean <- mean(sample)  # 样本均值
> z <- ( s_mean - h0_μ ) / people_sd
> z_0.5α <- abs(qnorm(α / 2, 0, 1))

> ### 判断
> if(z < z_α){
+   print("未拒绝原假设，95%把握原假设成立。")
+ } else{
+   print("拒绝原假设")
+ }
[1] "未拒绝原假设，95%把握原假设成立。"
```

**p-value检验法**

```r
# 双侧p-value
> p_value <- 2 * (1 - pnorm(abs(z), 0, 1))
> p_value
[1] 0.9199884
```

因为p-value&gt;0.05，所以结果更倾向于接受假定的参数取值。

**z.test\(\)**

```r
> z.test(x = sample, alternative = "two.sided", mu = 0, sigma.x = 1, conf.level = 0.95)

    One-sample z-Test

data:  sample
z = 0.71028, p-value = 0.4775
alternative hypothesis: true mean is not equal to 0
95 percent confidence interval:
 -0.1767325  0.3776290
sample estimates:
mean of x 
0.1004483
```

结果给出了z值，`p-vlaue`，备择假设的内容，置信区间，样本均值等，由于`p-vlaue > 0.05`，所以取原假设。

这里`conf.level = 0.95`仅是针对于置信区间来说的，并不改变`p-vlaue`。

**方差未知-t检验**

设总体$$X\sim N\left( μ,{ σ }^{ 2 } \right)$$，其中μ，σ^2^未知，H~0~: μ=μ~0~，H~1~: μ≠μ~0~，显著性水平为α。

由于总体方差未知，不能利用$$Z=\frac { \bar { X } -{ μ }_{ 0 } }{ { σ }/{ \sqrt { n } } }$$来确定拒绝域了。注意到S^2^是σ^2^的无偏估计，用S代替σ作为检验统计量$$t=\frac { \bar { X } -{ μ }_{ 0 } }{ { S }/{ \sqrt { n } } }$$。

而当H~0~为真时，$$t=\frac { \bar { X } -{ μ }_{ 0 } }{ { S }/{ \sqrt { n } } } \sim t\left( n-1 \right)$$，所以$$P\{ 当{ H }_{ 0 }为真拒绝{ H }_{ 0 }\} ={ P }_{ { μ }_{ 0 } }\{ \left| \frac { \bar { X } -{ μ }_{ 0 } }{ { S }/{ \sqrt { n } } } \right| \ge k\} =\alpha$$，从而$$k={ t }_{ { \alpha }/{ 2 } }\left( n-1 \right)$$，拒绝域为$$\left| t \right| =\left| \frac { \bar { X } -{ μ }_{ 0 } }{ { S }/{ \sqrt { n } } } \right| \ge { t }_{ { \alpha }/{ 2 } }\left( n-1 \right)$$。

上述利用t统计量得出的检验法称为**t检验法**。

**t.test\(\)**

这里使用`t.test()`函数来进行t检验：

```r
> t.test(x = sample, alternative = "two.sided", mu = 0, conf.level = 0.95)

    One Sample t-test

data:  sample
t = 0.85432, df = 49, p-value = 0.3971
alternative hypothesis: true mean is not equal to 0
95 percent confidence interval:
 -0.1358313  0.3367278
sample estimates:
mean of x 
0.1004483
```

结果给出了t值，自由度df，p-value，备择假设，置信区间，样本均值等。

#### 两个正太总体-均值差

**总体**：$$X\sim N\left( { \mu }_{ 1 },{ \sigma }^{ 2 } \right) ，Y\sim N\left( { \mu }_{ 2 },{ \sigma }^{ 2 } \right)$$

**假设**：$${ H }_{ 0 }:{ \mu }_{ 1 }-{ \mu }_{ 2 }=\delta ,{ H }_{ 1 }:{ \mu }_{ 1 }-{ \mu }_{ 2 }\neq \delta$$

**统计量**：$$t=\frac { \left( \bar { X } -\bar { Y } \right) -\delta }{ { S }_{ \omega }\sqrt { \frac { 1 }{ { n }_{ 1 } } +\frac { 1 }{ { n }_{ 2 } } } } \sim t\left( { n }_{ 1 }+{ n }_{ 2 }-2 \right)$$，其中$${ S }_{ \omega }^{ 2 }=\frac { \left( { n }_{ 1 }-1 \right) { S }_{ 1 }^{ 2 }+\left( { n }_{ 2 }-1 \right) { S }_{ 2 }^{ 2 } }{ { n }_{ 1 }+{ n }_{ 2 }-2 } ,{ S }_{ \omega }=\sqrt { { S }_{ \omega }^{ 2 } }$$

**拒绝域形式**：$$\left| \frac { \left( \bar { X } -\bar { Y } \right) -\delta }{ { S }_{ \omega }\sqrt { \frac { 1 }{ { n }_{ 1 } } +\frac { 1 }{ { n }_{ 2 } } } } \right| \ge k$$

**拒绝域**：由$$P\left\{ 当{ H }_{ 0 }为真拒绝{ H }_{ 0 } \right\} ={ P }_{ { \mu }_{ 1 }-{ \mu }_{ 2 }=\delta }\left\{ \left| \frac { \left( \bar { X } -\bar { Y } \right) -\delta }{ { S }_{ \omega }\sqrt { \frac { 1 }{ { n }_{ 1 } } +\frac { 1 }{ { n }_{ 2 } } } } \right| \ge k \right\} =\alpha$$，可得$$k={ t }_{ \alpha /2 }({ n }_{ 1 }+{ n }_{ 2 }-2)$$，于是拒绝域为:

$$\left| t \right| =\frac { \left| \left( \bar { X } -\bar { Y } \right) -\delta \right| }{ { S }_{ \omega }\sqrt { \frac { 1 }{ { n }_{ 1 } } +\frac { 1 }{ { n }_{ 2 } } } } \ge { t }_{ \alpha /2 }\left( { n }_{ 1 }+{ n }_{ 2 }-2 \right)$$

**举例**

先画出箱线图：

```r
> c1 <- c(79.98, 80.04, 80.02, 80.04, 80.03, 80.03, 80.04, 79.97, 80.05, 80.03, 80.02, 80.00, 80.02)
> c2 <- c(80.02, 79.94, 79.98, 79.97, 79.97, 80.03, 79.95, 79.97)
> data1 <- data.frame("A", c1)
> names(data1) <- c("type", "value")
> data2 <- data.frame("B", c2)
> names(data2) <- c("type", "value")
> data3 <- rbind(data1, data2)
> 
> ggplot(data3, aes(factor(type), value)) + geom_boxplot()
```

得到箱线图：

![image-20190618193429212](http://strawberryamoszc.oss-cn-shanghai.aliyuncs.com/2019-06-18-113429.png)

之后进行t-检验：

```r
> t.test(x = c1, y = c2, alternative = "greater", mu = 0,paired = FALSE,var.equal = TRUE, conf.level = 0.95)

    Two Sample t-test

data:  c1 and c2
t = 3.4722, df = 19, p-value = 0.001276
alternative hypothesis: true difference in means is greater than 0
95 percent confidence interval:
 0.0210942       Inf
sample estimates:
mean of x mean of y 
 80.02077  79.97875
```

由于`p-value = 0.001276 < 0.05`，所以拒绝H~0~。

#### 基于成对数据的检验\(t检验\)-逐对比较法

这种情况不是两个独立的随机变量的观察值，不能用上检验方法检验。而一般成对数据需要将每一对数据转换成一个新的数据，新数据之间互相独立，那么新数据之间的差异则是由试验本身引起的差异。

$${ D }_{ i }={ X }_{ i }-{ Y }_{ i },{ D }_{ 1 },{ D }_{ 2 },...,{ D }_{ n }$$相互独立。又由于$${ D }_{ 1 },{ D }_{ 2 },...,{ D }_{ n }$$是由同一因素所引起的，可认为他们服从同一分布。我们需要假设的前提是，$${ D }_{ i }\sim N\left( { \mu }_{ D },{ \sigma }_{ D }^{ 2 } \right) ,i=1,2,…,n$$，这就是说$${ D }_{ 1 },{ D }_{ 2 },...,{ D }_{ n }$$构成正态总体$$N\left( { \mu }_{ D },{ \sigma }_{ D }^{ 2 } \right)$$的一个样本，其中$${ \mu }_{ D },{ { \sigma }_{ D }^{ 2 } }$$未知，则需要使用t-检验。

**示例**

```r
> t.test(x = c1, y = c2,alternative = "less", mu = 0, paired = TRUE, conf.level = 0.05)

    Paired t-test

data:  c1 and c2
t = -2.3113, df = 7, p-value = 0.02704
alternative hypothesis: true difference in means is less than 0
5 percent confidence interval:
       -Inf -0.1137325
sample estimates:
mean of the differences 
                -0.0625
```

`p-value = 0.02704 < 0.05`，所以拒绝H~0~。

### 正态总体-方差

#### 单个总体

**总体**：$$X\sim N\left( { \mu },{ \sigma }^{ 2 } \right)$$，μ和σ^2^均未知

**假设**：$${ H }_{ 0 }:{ \sigma }^{ 2 }={ \sigma }_{ 0 }^{ 2 },{ H }_{ 1 }:{ \sigma }^{ 2 }\neq { \sigma }_{ 0 }^{ 2 }$$

**统计量**：$${ \chi }^{ 2 }=\frac { \left( n-1 \right) { S }^{ 2 } }{ { \sigma }_{ 0 }^{ 2 } } \sim { \chi }^{ 2 }\left( n-1 \right)$$

**拒绝域形式**：$$\frac { \left( n-1 \right) { S }^{ 2 } }{ { \sigma }_{ 0 }^{ 2 } } \le { k }_{ 1 }\quad 或\quad \frac { \left( n-1 \right) { S }^{ 2 } }{ { \sigma }_{ 0 }^{ 2 } } \ge { k }_{ 2 }$$

**拒绝域**：$$\frac { \left( n-1 \right) { S }^{ 2 } }{ { \sigma }_{ 0 }^{ 2 } } \le { \chi }_{ 1-\alpha /2 }^{ 2 }\left( n-1 \right) \quad 或\quad \frac { \left( n-1 \right) { S }^{ 2 } }{ { \sigma }_{ 0 }^{ 2 } } \ge { \chi }_{ \alpha /2 }^{ 2 }\left( n-1 \right)$$

**函数**

其实用的是卡方检验，只不过没有专门的函数来对单样本方差检验，可以用如下的自定义函数计算单正态总体方差的检验：

```r
var.test.1s <- function(x, n=length(x), var0, alternative="two.sided"){
  if(length(x)==1){ # 输入的是方差
    varx <- x
  } else {
    varx <- var(x)
  }
  xi <- (n-1)*varx/var0

  if(alternative=="less"){
    pvalue <- pchisq(xi, n-1)
  } else if (alternative=="greater"){
    pvalue <- pchisq(xi, n-1, lower.tail=FALSE)
  } else if(alternative=="two.sided"){
    pvalue <- 2*min(pchisq(xi, n-1), pchisq(xi, n-1, lower.tail=FALSE))
  }

  c(statistic=xi, pvalue=pvalue)
}
```

利用这个函数来进行单样本方差检验：

```r
> data <- rnorm(20, mean = 10, sd = 5.1)
> var.test.1s(x = data, var0 = 5, alternative = "two.sided")
   statistic       pvalue 
8.205815e+01 1.639003e-09
```

`p-value < 0.05`，则保留原假设。

#### 两个总体

**总体**：$$X\sim N\left( { \mu }_{ 1 },{ { { \sigma }_{ 1 }^{ 2 } } } \right) ，Y\sim N\left( { \mu }_{ 2 },{ \sigma }_{ 2 }^{ 2 } \right)$$，参数均未知

**假设**：$${ H }_{ 0 }:{ \sigma }_{ 1 }^{ 2 }\le { \sigma }_{ 2 }^{ 2 },{ H }_{ 1 }:{ \sigma }_{ 1 }^{ 2 }>{ \sigma }_{ 2 }^{ 2 }$$

**统计量**：$$\frac { { { S }_{ 1 }^{ 2 } }/{ { S }_{ 2 }^{ 2 } } }{ { { \sigma }_{ 1 }^{ 2 } }/{ { \sigma }_{ 2 }^{ 2 } } } \sim F({ n }_{ 1 }-1,{ n }_{ 2 }-1)$$

**拒绝域形式**：$$\frac { { S }_{ 1 }^{ 2 } }{ { S }_{ 2 }^{ 2 } } \ge k$$

**拒绝域**：$${ P }_{ { \sigma }_{ 1 }^{ 2 }\le { \sigma }_{ 2 }^{ 2 } }\left\{ \frac { { { S }_{ 1 }^{ 2 } }/{ { S }_{ 2 }^{ 2 } } }{ { { \sigma }_{ 1 }^{ 2 } }/{ { \sigma }_{ 2 }^{ 2 } } } \ge k \right\} =\alpha$$，$$k={ F }_{ \alpha }\left( { n }_{ 1 }-1,{ n }_{ 2 }-1 \right)$$，所以$$F=\frac { { { S }_{ 1 }^{ 2 } }/{ { S }_{ 2 }^{ 2 } } }{ { { \sigma }_{ 1 }^{ 2 } }/{ { \sigma }_{ 2 }^{ 2 } } } \ge { F }_{ \alpha }\left( { n }_{ 1 }-1,{ n }_{ 2 }-1 \right)$$，又因为$${ \sigma }_{ 1 }^{ 2 }\le { \sigma }_{ 2 }^{ 2 }$$，所以$$\frac { { { S }_{ 1 }^{ 2 } }/{ { S }_{ 2 }^{ 2 } } }{ { { \sigma }_{ 1 }^{ 2 } }/{ { \sigma }_{ 2 }^{ 2 } } } \ge \frac { { S }_{ 1 }^{ 2 } }{ { S }_{ 2 }^{ 2 } }$$，所以，我们只需要保证$$F=\frac { { S }_{ 1 }^{ 2 } }{ { S }_{ 2 }^{ 2 } } \ge { F }_{ \alpha }\left( { n }_{ 1 }-1,{ n }_{ 2 }-1 \right)$$即可，这也就是拒绝域。

**var.test\(\)**

可以使用`var.test()`函数来检验两个样本的方差比，用法与以上的函数类似：

```r
> data1 <- rnorm(30, mean = 10, sd = 4.95)
> data2 <- rnorm(40, mean = 10, sd = 5.05)
> var.test(x = data1, y = data2, ratio = 1, alternative = "greater", conf.level = 0.05)

    F test to compare two variances

data:  data1 and data2
F = 0.7109, num df = 29, denom df = 39, p-value = 0.8288
alternative hypothesis: true ratio of variances is greater than 1
5 percent confidence interval:
 1.286293      Inf
sample estimates:
ratio of variances 
         0.7109045
```

这里设定备择假设为方差比&gt;1，即原假设≤1，`p-value = 0.8288 > 0.05`所以我们认为保留原假设。

## 单因素方差分析

### 生成数据并进行描述统计

```r
> # 生成数据
> data1 <- rnorm(50, mean = 1000, sd = 400)
> data2 <- rnorm(60, mean = 999, sd = 399)
> data3 <- rnorm(70, mean = 1001, sd = 401)
> 
> data1 <- data.frame("A1", data1)
> names(data1) <- c("factor", "value")
> data2 <- data.frame("A2", data2)
> names(data2) <- c("factor", "value")
> data3 <- data.frame("A3", data3)
> names(data3) <- c("factor", "value")
> 
> data <- rbind(data1, data2, data3)
> 
> # 查看数据
> table(data$factor)

A1 A2 A3 
50 60 70 
> str(data)
'data.frame':    180 obs. of  2 variables:
 $ factor: Factor w/ 3 levels "A1","A2","A3": 1 1 1 1 1 1 1 1 1 1 ...
 $ value : num  974 296 1228 1645 345 ...
> summary(data)
 factor      value       
 A1:50   Min.   :-203.2  
 A2:60   1st Qu.: 695.6  
 A3:70   Median : 977.3  
         Mean   : 975.9  
         3rd Qu.:1239.3  
         Max.   :2519.3  
> # 数据透视
> aggregate(value~factor, data, mean)
  factor     value
1     A1  909.9914
2     A2  988.9353
3     A3 1011.8342
> aggregate(value~factor, data, sd)
  factor    value
1     A1 456.8581
2     A2 458.6332
3     A3 392.5218
> # 作图
> ggplot(data, aes(value)) +
+   geom_histogram(binwidth = 20) +
+   facet_grid(factor~.)
> ggplot(data, aes(factor(factor), value)) + geom_boxplot()
```

生成的直方图如下：

![image-20190621202222075](http://strawberryamoszc.oss-cn-shanghai.aliyuncs.com/2019-06-21-122222.png)

生成的箱线图如下：

![image-20190621202600310](http://strawberryamoszc.oss-cn-shanghai.aliyuncs.com/2019-06-21-122601.png)

### 方差齐次性检验

```r
> bartlett.test(value~factor, data)

    Bartlett test of homogeneity of variances

data:  value by factor
Bartlett's K-squared = 1.929, df = 2, p-value = 0.3812
```

一般显著性水平会选择α=0.1，由`p-value = 0.3812 > 0.1`，所以可以认为方差是没有显著差异的。

### 方差分析

```r
> # 方差分析
> data_aov <- aov(value~factor, data)
> View(data_aov)
> summary(data_aov)
             Df   Sum Sq Mean Sq F value Pr(>F)
factor        2   317781  158890   0.845  0.431
Residuals   177 33268632  187958
```

上面结果其实给出的是单因素方差分析表\(除了最后一行\)，给出了F值和`p-value`\(即Pr\)，这里我们可以直接通过`p-value = 0.431 > 0.05`来进行判断，个组均值之间没有明显的差距。

如果我们将数据更换如下：

```r
data1 <- rnorm(50, mean = 800, sd = 200)
data2 <- rnorm(60, mean = 850, sd = 199)
data3 <- rnorm(70, mean = 900, sd = 201)
```

再进行方差分析：

```r
> # 方差分析
> data_aov <- aov(value~factor, data)
> summary(data_aov)
             Df  Sum Sq Mean Sq F value  Pr(>F)   
factor        2  505361  252681   5.687 0.00404 **
Residuals   177 7864344   44431                   
---
Signif. codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1
```

可以看出不同组织间差异是非常明显的，接着我们需要进行多重比较。

### 多重比较

```r
> # 多重比较
> TukeyHSD(data_aov)
  Tukey multiple comparisons of means
    95% family-wise confidence level

Fit: aov(formula = value ~ factor, data = data)

$factor
           diff       lwr      upr     p adj
A2-A1  78.56096 -16.84027 173.9622 0.1288628
A3-A1 131.61418  39.36238 223.8660 0.0026251
A3-A2  53.05322 -34.59948 140.7059 0.3274080
```

`TukeyHSD()`给出了均值差，最小差和最大差，还给出了`p-value`。

可以发现，A~3~和A~1~之间区别是很显著的，其他两组之间区别就没那么显著。

我们将以上数据作图：

```r
> par(las = 1)
> par(mar=c(3,5,3,4))
> plot(TukeyHSD(data_aov))
```

作图结果如下：

![image-20190621225104405](http://strawberryamoszc.oss-cn-shanghai.aliyuncs.com/2019-06-21-145104.png)

**注意**：

1. `par()`函数用于[设定作图参数](https://zhuanlan.zhihu.com/p/30175392)；
2. `las`和`mar`参数均可以在[PLOTB](https://www.plob.org/article/8622.html)找到。

## 双因素方差分析

### 描述统计

以`ToothGrowth`数据为例：

```r
> data <- ToothGrowth
> attach(data)
> View(data)
> # 描述统计
> table(supp, dose)
    dose
supp 0.5  1  2
  OJ  10 10 10
  VC  10 10 10
> # 查看变量数和观测数
> str(data)
'data.frame':    60 obs. of  3 variables:
 $ len : num  4.2 11.5 7.3 5.8 6.4 10 11.2 11.2 5.2 7 ...
 $ supp: Factor w/ 2 levels "OJ","VC": 2 2 2 2 2 2 2 2 2 2 ...
 $ dose: num  0.5 0.5 0.5 0.5 0.5 0.5 0.5 0.5 0.5 0.5 ...
> # 数据基本统计
> summary(data)
      len        supp         dose      
 Min.   : 4.20   OJ:30   Min.   :0.500  
 1st Qu.:13.07   VC:30   1st Qu.:0.500  
 Median :19.25           Median :1.000  
 Mean   :18.81           Mean   :1.167  
 3rd Qu.:25.27           3rd Qu.:2.000  
 Max.   :33.90           Max.   :2.000  
> # 数据透视，分组查看数据
> aggregate(len~supp+dose, data, mean)
  supp dose   len
1   OJ  0.5 13.23
2   VC  0.5  7.98
3   OJ  1.0 22.70
4   VC  1.0 16.77
5   OJ  2.0 26.06
6   VC  2.0 26.14
> aggregate(len~supp+dose, data, sd)
  supp dose      len
1   OJ  0.5 4.459709
2   VC  0.5 2.746634
3   OJ  1.0 3.910953
4   VC  1.0 2.515309
5   OJ  2.0 2.655058
6   VC  2.0 4.797731
```

作图观察数据透视的结果：

```r
> detach(data)
> ggplot(aggr_mean, aes(factor(dose), len, fill=factor(supp))) + geom_col(position = "dodge")
```

![image-20190621233516084](http://strawberryamoszc.oss-cn-shanghai.aliyuncs.com/2019-06-21-153516.png)

```r
> ggplot(aggr_sd, aes(factor(dose), len, fill=factor(supp))) + geom_col(position = "dodge")
```

![image-20190621233540492](http://strawberryamoszc.oss-cn-shanghai.aliyuncs.com/2019-06-21-153540.png)

### 方差齐次性检验

我们仍然使用`bartlett.test()`函数进行方差齐次性检验，这里，由于是双因素，所以我们可以设定interaction这个参数，将多个自变量折叠为一个单一的变量用于表示不同变量因素之间的组合。

```r
> bartlett.test(len~interaction(supp, dose), data)

    Bartlett test of homogeneity of variances

data:  len by interaction(supp, dose)
Bartlett's K-squared = 6.9273, df = 5, p-value = 0.2261
```

`p-value = 0.2261 > 0.1`故我们可以认为方差相同。

### 方差分析

```r
> # 方差分析
> data$dose <- factor(data$dose)
> data_aov <- aov(len~dose+supp+dose:supp, data)
# 这里也可以写成乘号： data_aov <- aov(len~dose * supp, data)
> summary(data_aov)
            Df Sum Sq Mean Sq F value   Pr(>F)    
dose         2 2426.4  1213.2  92.000  < 2e-16 ***
supp         1  205.4   205.4  15.572 0.000231 ***
dose:supp    2  108.3    54.2   4.107 0.021860 *  
Residuals   54  712.1    13.2                     
---
Signif. codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1
```

给出了相应的结论，我们通过`p-value`就可以进行判断，`summary`中其实已经给出了判断。

**有交互作用时**：`data_aov <- aov(len~dose+supp+dose:supp, data)`

**无交互作用时**：`data_aov <- aov(len~dose+supp, data)`

