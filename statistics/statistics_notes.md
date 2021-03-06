---
description: >-
  此篇为统计部分笔记，选取了几个重要部分进行比较详细的阐述，关于假设检验部分，由于笔者认为其思路与参数估计很相似，于是就暂未详述。这部分请参考R笔记的parameter
  estimation and hypothesis testing。
---

# Statistics\_Notes

## 统计学概述

在概率论中，我们多研究的随机变量，它的分布都是假设已知的。如果你已经知道了随机变量X是的分布和参数，你去推导它的期望、方差等数字特征，去推导它其他一些性质，去推导X的平方是什么分布，或推导和另一个随机变量Y相加又是什么分布。这些工作属于**概率论**范畴。

但在数理统计中，我们研究的随机变量，它的分布是未知的，或者是某些参数不知道，人们通过对所研究的随机变量进行重复独立的观察，得到许多观察值，对这些数据进行分析，从而对所研究的随机变量的分布做出种种推断。比如，实际工作中有个随机变量Z，你不知道是什么分布，你看到了一些试验值，觉得Z可能是正态分布，于是你假设Z是正态分布，你用试验数据，推断出它的均值可能是1，方差可能是4，然后做假设检验，看看这一结论在多大程度上可靠，如果认为可靠，用这个结论来做分析，或者预测将要进行的试验结果。这叫**统计**。

概率论是统计推断的基础，在给定数据生成过程下观测、研究数据的性质，是**推理**；而统计推断则根据观测的数据，反向思考其数据生成过程。预测、分类、聚类、估计等，都是统计推断的特殊形式，强调对于数据生成过程的研究，是**归纳**。

## 抽样、整理、抽样分布

### 抽样：总体与样本

#### 总体

**总体**，是指由许多有某种共同性质的事物组成的集合，会在此集合中选出样本进行[统计推断](https://zh.wikipedia.org/wiki/统计推断)，选取样本的方式可能会用乱数或是其他[抽样](https://zh.wikipedia.org/wiki/抽樣)方式。

例如要针对所有乌鸦的共有特性进行研究，总体是目前存在、以前曾经存在或是未来可能存在的所有乌鸦。**但是，因为时间的限制、地域可取得性的限制、以及研究者的有限资源等，不可能观测总体中的每一个，因此研究者会从总体中产生样本，再由样本的特性去了解总体的特性。**

产生样本的目的之一就是为了要知道**总体的特性**，包括

* 总体均值：$$\mu =\frac { \sum _{ i=1 }^{ N }{ { x }_{ i } }  }{ N }$$
* 总体标准差：$$\sigma =\sqrt { \frac { 1 }{ N } \sum _{ i=1 }^{ N }{ { \left( { x }_{ i }-\mu  \right)  }^{ 2 } }  }$$

#### 样本

研究中，从总体中抽取（观察或调查）一部分的个体称为样本。

**样本容量**

样本容量是指一个样本中所包含的单位数，一般用n表示，它是抽样推断中非常重要的概念。样本容量的大小与推断估计的准确性有着直接的联系，即在总体既定的情况下，样本容量越大其统计估计量的代表性误差就越小，反之,样本容量越小其估计误差也就越大。

**最常用的样本统计量——样本均值**

根据样本构造的不含未知参数的函数为**统计量**，样本均值是一个统计量。我们可以用样本均值描述一个样本，多个样本则会有多个样本均值。

### 整理：统计数据的整理和显示

#### 直方图和箱线图

**直方图**

略

**分位**

**Q1：四分位**

**Q3：四分之三分位**

**IQR：四分位差**

1. 几乎 50% 的数据在 IQR 间。
2. IQR 受到数据集中每一个值的影响。
3. IQR 不受异常值的影响。
4. 均值不一定在IQR中；

**异常值Outlier**

Outlier &lt; Q1 - 1.5 \* IQR

Outlier &gt; Q3 + 1.5 \* IQR

**箱线图Boxplot**

![&#x7BB1;&#x7EBF;&#x56FE;&#x56FE;&#x793A;](http://strawberryamoszc.oss-cn-shanghai.aliyuncs.com/2019-06-15-152014.jpg)

#### 集中趋势

**均值Mean**

当数据中出现异常值时，均值无法描述分布中心；

**中位数Medium**

众数也很难描述分布中心；

**众数Mode**

中位数不会考虑到所有的数据，对异常值的鲁棒性更好。在处理高偏斜分布时，中位数通常能够最好地反映出集中趋势。

**正偏斜分布与负斜分布**

正斜分布靠左：mode&lt;medium&lt;mean

负斜分布靠右：mean&lt;medium&lt;mode

**鲁棒性Robust**

即使偏离了基准也不会受太大的影响。

#### 离散程度

**方法**

找出任意两个值之间差的平均值：数值过多

找出每个值与最大值或最小值之间差的平均值：容易受异常值干扰

找出每个值与数据集均值之间差的平均值：适合

**概念**

**离均差**：$${ x }_{ i }-\bar { x }$$

**平均偏差**：$$\sum { \frac { { x }_{ i }-\bar { x } }{ n } }=0$$

**平均绝对偏差**：$$\sum { \frac { \left| { x }_{ i }-\bar { x } \right| }{ n } } =0$$

**（总体）方差\(平均平方偏差**\)：$$DX=\sum { \frac { { \left( { x }_{ i }-\bar { x } \right) }^{ 2 } }{ n } } =E{ \left( X-EX \right) }^{ 2 }=E{ X }^{ 2 }-{ \left( EX \right) }^{ 2 }$$

**（总体）标准差**：$$\sigma =\sqrt { DX }$$

**贝塞尔校正**

比如在高斯分布（正态分布）中，我们抽取一部分的样本，用样本的方差来估计总体的方差。由于样本主要是落在x=μ中心值附近，那么样本方差一定小于总体的方差（因为高斯分布的边沿抽取的数据很少）。为了能弥补这方面的缺陷，那么我们把公式的n改为n-1,以此来提高方差的数值。这种方法叫做贝塞尔校正系数。

当我们用小样本数据的标准差去估计总体的标准差的时候采用 n-1,但是这个小样本数据的实际标准差还是用 n 的那个公式 的，不要混淆了数据的实际标准差。

**无偏性证明**

对于一个随机变量$$X$$进行$$n$$次抽样，获得样本$${ x }_{ 1 },{ x }_{ 2 },...,{ x }_{ n }$$，那么样本均值为:$$\bar { x } =\frac { 1 }{ n } \sum _{ i=1 }^{ n }{ { x }_{ i } }$$

有偏的样本方差为:

$${ s }_{ n }^{ 2 }=\frac { 1 }{ n } \sum _{ i=1 }^{ n }{ { ({ x }_{ i }-\bar { x } ) }^{ 2 } } =\frac { \sum _{ i=1 }^{ n }{ { x }_{ i }^{ 2 } } }{ n } -\frac { { \left( \sum _{ i=1 }^{ n }{ { x }_{ i } } \right) }^{ 2 } }{ { n }^{ 2 } }$$

无偏的样本方差为：

$${ s }^{ 2 }=\frac { 1 }{ n-1 } \sum _{ i=1 }^{ n }{ { ({ x }_{ i }-\bar { x } ) }^{ 2 } } =\left( \frac { n }{ n-1 } \right) { s }_{ n }^{ 2 }$$

为了证明$${s}^{2}$$的无偏性， 我们拿出样本方差种的一部分来进行单独分析,

$$\quad \sum _{ i=1 }^{ n }{ { \left( { x }_{ i }-\bar { x } \right) }^{ 2 } } \\ =\sum _{ i=1 }^{ n }{ \left( { x }_{ i }^{ 2 }-2{ x }_{ i }\bar { x } +{ \bar { x } }^{ 2 } \right) } \\ =\sum _{ i=1 }^{ n }{ { x }_{ i }^{ 2 } } -2\bar { x } \sum _{ i=1 }^{ n }{ { x }_{ i } } +\sum _{ i=1 }^{ n }{ { \bar { x } }^{ 2 } } \\ =\sum _{ i=1 }^{ n }{ { x }_{ i }^{ 2 } } -2n{ \bar { x } }^{ 2 }+n{ \bar { x } }^{ 2 }\\ =\sum _{ i=1 }^{ n }{ { x }_{ i }^{ 2 } } -n{ \bar { x } }^{ 2 }$$

同理，我们有

![&#x5C4F;&#x5E55;&#x5FEB;&#x7167; 2019-06-15 &#x4E0B;&#x5348;11.07.45](http://strawberryamoszc.oss-cn-shanghai.aliyuncs.com/2019-06-15-150803.png)

对上式两侧取期望，我们有

![&#x5C4F;&#x5E55;&#x5FEB;&#x7167; 2019-06-15 &#x4E0B;&#x5348;11.08.24](http://strawberryamoszc.oss-cn-shanghai.aliyuncs.com/2019-06-15-150834.png)

因为$$\overline{x}=\frac{1}{n}\sum_{i=1}^n x_i$$，于是我们有$$\begin{align*} \operatorname{Var}{(\overline {x})} = \frac{1}{n}\operatorname{Var}{{x}} \end{align*}$$

因此

![&#x5C4F;&#x5E55;&#x5FEB;&#x7167; 2019-06-15 &#x4E0B;&#x5348;11.08.53](http://strawberryamoszc.oss-cn-shanghai.aliyuncs.com/2019-06-15-150901.png)

最后，我们有

![&#x5C4F;&#x5E55;&#x5FEB;&#x7167; 2019-06-15 &#x4E0B;&#x5348;11.09.23](http://strawberryamoszc.oss-cn-shanghai.aliyuncs.com/2019-06-15-150929.png)

可见$${ S }^{ 2 }$$是对$$Var\left( X \right)$$ 的无偏估计。

**3σ原则**

数值分布在（μ-σ,μ+σ\)中的概率为0.6827

数值分布在（μ-2σ,μ+2σ\)中的概率为0.9545

数值分布在（μ-3σ,μ+3σ\)中的概率为0.9973

#### 归一化：标准正态分布

**样本均值的频数直方图**的数字不能直接看出比例排名，所以引入**频率直方图**，但直方图固有弊端在于会缺少部分信息，所以需要缩小组距以增加信息，但过小又没有了直方图意义，所以引入**概率分布图**——**标准正态分布**。

**Z 值：标准差数量**

**公式**

$$z=\frac { x-\mu }{ \delta }$$

**含义**

无论值是多少，我们都可以将其转换为与均值的标准差。通过将正态分布中的值转换为z，就可以知道小于或大于该值得百分比。

例如某个值与平均值相差1个标准偏差σ，则无论是哪种正态分布，我们都知道大约80%的值&lt;该值。

**标准正态分布**

我们可以将任何正态分布转化为标准正态分布，通过Z值进行分析，再按照任何方式扩展。

**Z 值表**

[链接](https://s3.amazonaws.com/udacity-hosted-downloads/ZTable.jpg)。

### 抽样分布

#### 样本统计量

根据样本构造的不含未知参数的函数为统计量。

**样本均值**

$$\bar { X } =\frac { 1 }{ n } \sum _{ i=1 }^{ n }{ { X }_{ i } }$$

**样本方差**

$${ S }^{ 2 }=\frac { 1 }{ n-1 } \sum _{ i=1 }^{ n }{ { { \left( { X }_{ i }-\bar { X } \right) }^{ 2 } } } =\frac { 1 }{ n-1 } \left( \sum _{ i=1 }^{ n }{ { X }_{ i }^{ 2 } } -n{ \bar { X } }^{ 2 } \right)$$

**样本标准差**

$$S=\sqrt { { S }^{ 2 } } =\sqrt { \frac { 1 }{ n-1 } \left( \sum _{ i=1 }^{ n }{ { X }_{ i }^{ 2 } } -n{ \bar { X } }^{ 2 } \right) }$$

**样本K阶\(原点\)矩**

$${ A }_{ k }=\frac { 1 }{ n } \sum _{ i=1 }^{ n }{ { X }_{ i }^{ k } } ,\quad k=1,2,...$$

**样本K阶中心矩**

$${ B }_{ k }=\frac { 1 }{ n } \sum _{ i=1 }^{ n }{ { \left( { X }_{ i }-\bar { X } \right) }^{ k } } ,\quad k=1,2,…$$

#### 抽样分布

在使用统计量进行统计推断时，需要知道统计量的分布，比如样本均值的分布。

**统计量的分布**，叫做**抽样分布**。总体分布函数已知时，样本分布是确定的，但是：

1. 通常，我们是不知道总体分布的；
2. 要求出统计量的精确分布是困难的。

虽然总体不知道时，我们很难确定，解决这种问题需要学习非参数统计。然而，有两种情况是比较好研究的：

1. 对于正太总体分布，其常用的统计量的分布是可以推断出来的。
2. 对于一般总体分布，我们可以由大数定律和中心极限定理得到其样本均值统计量的期望、分布和方差等。

#### 正态总体的常用统计量的分布

假设总体$$X\sim N\left( \mu ,{ \sigma }^{ 2 } \right)$$。

我们可能会用到各种各样的**统计量**，但归根结底是这些统计量满足**四种典型的分布**，即**z（即正态分布）、**$${ \chi }^{ 2 }$$**、t和F分布**，**每一个分布对应一种检验方法**，即z检验、检$${ \chi }^{ 2 }$$验、t检验和F检验。

这些统计量大多都是一个样本或多个样本的样本均值$$\bar { X }$$、样本方差$${ S }^{ 2 }$$、总体均值$$\mu$$和总体方差$${\sigma}^{2}$$这些元素组成的，比如$$\frac { \bar { X } -\mu }{ { \sigma }/{ \sqrt { n } } } ， \frac { \bar { X } -\mu }{ { S }/{ \sqrt { n } } }$$等，但我们在选择时，一定要只有被检验一个参数不知道，所以，如果我们想用第一个统计量，那么除了$$\bar { X }$$，n这俩一定知道的参数之外，如果我们要检验μ，那么就必须知道总体标准差σ。换句话说，如果我们知道总体标准差σ，那么我们就可以选择$$\frac { \bar { X } -\mu }{ { \sigma }/{ \sqrt { n } } }$$这个统计量，并根据其满足的标准正态分布规律对总体均值μ进行假设检验（或求置信区间）。

但是实际情况中，总体标准差σ我们大多不知道，这个时候就不能使用$$\frac { \bar { X } -\mu }{ { \sigma }/{ \sqrt { n } } }$$这个统计量了，而由于我们能求出样本标准差S，那么就可以选择$$\frac { \bar { X } -\mu }{ { S }/{ \sqrt { n } } }$$这个统计量，这个统计量需要知道的关于总体的信息\(参数\)更少，但也服从t\(n-1\)分布。也就是只需要知道总体满足正态分布即可，而不需要知道其总体方差σ，就可以对总体均值μ进行检验。

我们了解并学习这4种分布，是因为这4种分布，其分布函数和密度函数都能很好地进行量化，正态分布就是最好的例子，其他三种只是学习之前我们不常接触而已。

以下是对这四种分布的详细的介绍。

**样本均值的正态分布**

样本均值是最常用的统计量之一，一般用于**z-检验**，用以检验总体均值。

**统计量**：$$\bar { X } =\frac { 1 }{ n } \sum _{ i=1 }^{ n }{ { X }_{ i } }$$，或$$z=\frac { \bar { X } -\mu }{ { \sigma }/{ \sqrt { n } } }$$

**统计量分布**：$$\bar { X } \sim N\left( μ,\frac { { \sigma }^{ 2 } }{ n } \right)$$，或$$z\sim N\left( 0,1 \right)$$。

**卡方分布**

**定义**

设$$X\sim N\left( 0 ,1 \right)$$，则称统计量

$${ \chi }^{ 2 }={ X }_{ 1 }^{ 2 }+{ X }_{ 2 }^{ 2 }+...+{ X }_{ n }^{ 2 }$$

服从自由度为n的$${ \chi }^{ 2 }$$分布，记为$${ \chi }^{ 2 }\sim { \chi }^{ 2 }\left( n \right)$$，自由度指上式右端包含的独立变量的个数。

**密度函数**

$${ f }_{ n }\left( y \right) =\frac { 1 }{ { 2 }^{ n/2 }\Gamma \left( n/2 \right) } { y }^{ n/2-1 }{ e }^{ -y/2 }$$，其中x≥0；

当x≤0时$${\displaystyle f_{k}(x)=0}$$。这里$$\Gamma$$代表[Gamma函数](https://zh.wikipedia.org/wiki/Γ函数)。

> 推导见书P139

**图形**

![image-20190618091209620](http://strawberryamoszc.oss-cn-shanghai.aliyuncs.com/2019-06-18-011209.png)

**分布的可加性**

设$${ \chi }_{ 1 }^{ 2 }\sim { \chi }^{ 2 }\left( { n }_{ 1 } \right) ,{ \chi }_{ 2 }^{ 2 }\sim { \chi }^{ 2 }\left( { n }_{ 2 } \right)$$，并且$${ \chi }_{ 1 }^{ 2 },{ \chi }_{ 2 }^{ 2 }$$相互独立，则有$${ \chi }_{ 1 }^{ 2 }+{ \chi }_{ 2 }^{ 2 }\sim { \chi }^{ 2 }\left( { n }_{ 1 }+{ n }_{ 2 } \right)$$

**分布的数学期望和方差**

$${ \chi }^{ 2 }\sim { \chi }^{ 2 }\left( { n } \right)$$，则$$E({ \chi }^{ 2 })=n, D({ \chi }^{ 2 })=2n$$。

**分布上的分位点**

对于给定的α，0&lt;α &lt;1，满足条件：

$$P\{ { \chi }^{ 2 }>{ \chi }_{ \alpha }^{ 2 }(n)\} =\int _{ { \chi }_{ \alpha }^{ 2 }(n) }^{ \infty }{ f(y)dy=\alpha }$$

**分布表**

[卡方分布表](http://strawberryamoszc.oss-cn-shanghai.aliyuncs.com/2019-06-18-kaiFang-distribution-LinjiezhiBiao.xls)

费希尔曾证明，当n充分大时，近似地有$${ \chi }_{ \alpha }^{ 2 }(n)\approx \frac { 1 }{ 2 } { ({ z }_{ \alpha }+\sqrt { 2n-1 } ) }^{ 2 }$$。

利用前式可以求得当n&gt;40时卡方分布上α分位点的近似值。

**t分布**

**定义**

设$$X\sim N\left( 0 ,1 \right)$$，$$Y\sim { \chi }^{ 2 }\left( n \right)$$，且X，Y相互独立，则称随机变量\(统计量\)

$$t=\frac { X }{ \sqrt { Y/n } }$$

服从自由度为n的t分布，即为$$t\sim t(n)$$，t分布又称学生氏\(student\)分布。

**密度函数**

$$h(t)=\frac { \Gamma [(n+1)/2] }{ \sqrt { \pi n } \Gamma \left( n/2 \right) } { (1+\frac { { t }^{ 2 } }{ n } ) }^{ -(n+1)/2 },-\infty <t<\infty$$

**图像**

![image-20190618124716219](http://strawberryamoszc.oss-cn-shanghai.aliyuncs.com/2019-06-18-044716.png)

h\(t\)的图形关于t=0对称，当n充分大时，其图形类似于标准正态变量概率密度的图形关于t=0对称，当n充分大时其图形类似于标准正态变量概率密度的图形。但对于较小n，t分布于N\(0,1\)分布相差较大。

**t分布分位点**

对于给定的α，0&lt;α &lt;1，满足条件：

$$P\{ t>{ t }_{ \alpha }(n)\} =\int _{ { t }_{ \alpha }(n) }^{ \infty }{ h(t)dt } =\alpha$$

的点$${ t }_{ a }\left( n \right)$$就是t\(n\)分布上的α分位点。

$${ t }_{ 1-a }\left( n \right) ={ -t }_{ a }\left( n \right)$$

且当n&gt;45时，对于常用的α的值，就用正态近似：$${ t }_{ a }\left( n \right) ={ z }_{ \α }$$

**F分布**

**定义**

$$U\sim { \chi }^{ 2 }({ n }_{ 1 }),V\sim { \chi }^{ 2 }({ n }_{ 2 })$$，且U，V相互独立，则称随机变量 $$F=\frac { U/{ n }_{ 1 } }{ V/{ n }_{ 2 } }$$ 服从自由度为\(n~1~, n~2~\)的F分布，记为$$F\sim F({ n }_{ 1 }{ ,n }_{ 2 })$$。

**概率密度**

$$\varphi (y)=\frac { \Gamma [({ n }_{ 1 }+{ n }_{ 2 })/2]{ ({ n }_{ 1 }/{ n }_{ 2 }) }^{ { n }_{ 1 }/2 }{ y }^{ ({ n }_{ 1 }/2)-1 } }{ \Gamma ({ n }_{ 1 }/2)\Gamma ({ n }_{ 2 }/2){ [1+({ n }_{ 1 }y/{ n }_{ 2 })] }^{ ({ n }_{ 1 }+{ n }_{ 2 })/2 } } ,y>0$$，其他为0。

**图形**

![image-20190618133528674](http://strawberryamoszc.oss-cn-shanghai.aliyuncs.com/2019-06-18-053530.png)

由定义可知，若$$F\sim F({ n }_{ 1 }{ ,n }_{ 2 })$$，则$$\frac { 1 }{ F } \sim F({ n }_{ 2 },{ n }_{ 1 })$$

还有性质：$${ { F }_{ 1-\alpha } }({ n }_{ 1 },{ n }_{ 2 })=\frac { 1 }{ { { F }_{ \alpha } }({ n }_{ 2 },{ n }_{ 1 }) }$$

**分位点**

对于给定的α，0&lt;α &lt;1，满足条件：

$$P\{ F>{ F }_{ \alpha }({ n }_{ 1 },{ n }_{ 2 })\} =\int _{ { F }_{ \alpha }({ n }_{ 1 },{ n }_{ 2 }) }^{ \infty }{ \varphi (y)dy=\alpha }$$

的点$${ F }_{ \alpha }({ n }_{ 1 },{ n }_{ 2 })$$就是$${ F }({ n }_{ 1 },{ n }_{ 2 })$$分布的**上α分位点**。

类似地有卡方分布，t分布，F分布的**下分位点**。

**常用统计量的分布**

1. $$\frac { \bar { X } -\mu }{ { \sigma }/{ \sqrt { n } } } \sim N\left( 0,1 \right)$$
2. $$\frac { \left( n-1 \right) { S }^{ 2 } }{ { \sigma }^{ 2 } } \sim { \chi }^{ 2 }\left( n-1 \right)$$
3. $$\bar { X }$$与S^2^相互独立
4. $$\frac { \bar { X } -\mu }{ { S }/{ \sqrt { n } } }\sim t(n-1)$$
5. $$\frac { { { S }_{ 1 }^{ 2 } }/{ { S }_{ 2 }^{ 2 } } }{ { { \sigma }_{ 1 }^{ 2 } }/{ { \sigma }_{ 2 }^{ 2 } } } \sim F({ n }_{ 1 }-1,{ n }_{ 2 }-1)$$
6. 当$${ \sigma }_{ 1 }^{ 2 }={ \sigma }_{ 2 }^{ 2 }={ \sigma }^{ 2 }$$时，$$\frac { \left( \bar { X } -\bar { Y } \right) -\left( { \mu }_{ 1 }-{ \mu }_{ 2 } \right) }{ { S }_{ \omega }\sqrt { \frac { 1 }{ { n }_{ 1 } } +\frac { 1 }{ { n }_{ 2 } } } } \sim t\left( { n }_{ 1 }+{ n }_{ 2 }-2 \right)$$，其中$${ S }_{ \omega }^{ 2 }=\frac { \left( { n }_{ 1 }-1 \right) { S }_{ 1 }^{ 2 }+\left( { n }_{ 2 }-1 \right) { S }_{ 2 }^{ 2 } }{ { n }_{ 1 }+{ n }_{ 2 }-2 } ,{ S }_{ \omega }=\sqrt { { S }_{ \omega }^{ 2 } }$$

#### 一般总体样本均值的分布

**大数定律和中心极限定理回顾**

在概率论中，我们已经了解了**大数定律**和**中心极限定理**（详见前面的章节）：

**大数定律**讲的是样本均值收敛到总体均值（就是期望），像这个图一样：

![v2-5dcd7a13095ba29c6a5c8717d83a5ff5\_hd](http://strawberryamoszc.oss-cn-shanghai.aliyuncs.com/2019-06-15-150958.jpg)

而中心极限定理告诉我们，当样本量足够大时，样本均值的分布慢慢变成正态分布，就像这个图：

![v2-8755dc22a65c4ef475cc391f9302161d\_hd](http://strawberryamoszc.oss-cn-shanghai.aliyuncs.com/2019-06-15-151015.jpg)

**示例**

X代表掷骰子点数的随机变量，X=1,2,…,6，EX=3.5，我们做一次试验时掷2次骰子，即样本容量为2，做一次实验的话是一个样本，2个数字的均值是一个统计量，叫样本均值。

对于这个实验，我们知道总体分布或分布律，为比如一个样本\(1, 4\)，样本均值=2.5，也就是观察值=2.5。我们可以发现，只做一次试验，样本统计量的观察值是不等于总体X的均值EX=3.5的。

但是，只要我们试验的次数足够多，比如又做了100次试验，得到100个样本：\(4, 6\), \(3, 1\), \(1, 2\)… 样本均值的观察值依次为：5，2，1.5，… 大数定律说的就是这些样本均值依概率收敛于总体期望，即$$\frac { 1 }{ 100 } \left( 2.5+5+2+1.5... \right) \approx 3.5=EX$$ ，用依概率收敛的符号表示即$$\bar { { X }_{ n } } \overset { p }{ \rightarrow } μ$$。

中心极限定理是说，当样本量足够大时，这些样本均值的观察值是满足正态分布的。

**总结**

随机变量$$X$$，$$EX=μ$$，$$DX={ \sigma }^{ 2 }$$。则独立同分布情况下，若样本量很大，由中心极限定理，样本均值$$\bar { X }$$近似地服从参数为$$N(\mu ,\frac { { \sigma }^{ 2 } }{ n } )$$的正态分布。

样本容量如果增大n倍，其标准差会缩小为$$\frac { 1 }{ \sqrt { n } }$$，分布也会变窄。

**应用**

1. 对于一个随机变量$$X$$，$$EX=\mu$$，$$DX={ \sigma }^{ 2 }$$。若设定样本容量为n，我们可以得到样本均值$$\bar { X }$$的满足参数为$$N(\mu ,\frac { { \sigma }^{ 2 } }{ n } )$$的正态分布，换个说法，$$\frac { \bar { X } -\mu }{ { \sigma }/{ \sqrt { n } } } \sim N\left( 0,1 \right)$$.
2. 在分布确定、有了抽样分布的基础上，当我们实际得到一个样本，我们想检验这个样本是否正常。
3. 既然μ，σ和n均已知，那么$$\frac { \bar { X } -\mu  }{ { \sigma  }/{ \sqrt { n }  } }$$是一个统计量，即z，由于单位正态分布天然的计算和观察优势，我们可以利用z得到出现此样本的概率。
4. 比如，我们得到z&gt;z~0~的概率只有0.01，那么我们可以认为这是不正常的。因为小概率事件在一次试验中是很难发生的，但也确实有可能发生，比如这里发生的几率就是0.01。
5. 所以我们如果假定，一次试验当原假设为真时，我们不接受它的概率为0.05，也就是说弃真错误α=0.05，我们就会抛弃这个样本，觉得它是假的，也就是说我们认为这个样本不正常。另一种说法是，我们有0.95的把握认为这个样本是不正常的。
6. 这就是假设检验的基本思想，具体会在之后的章节提到。

## 参数估计

> 参数估计导读来自[Junyi Hou](https://www.zhihu.com/question/21871331/answer/22225104).

**点估计**就是用一个数据（data）的函数（通常称为估计统计量，estimator）来给出一个未知参数的估计值。

即使是固定的参数真值（虽然我们不知道这个值），由于数据的随机性，不同的数据代入这个函数往往会得出不同的估计值（estimation ）。所以我们往往在点估计的基础上包裹上一个邻域，即得到一个**区间估计**。

那么点估计周围的这个邻域的大小是怎么确定的呢？一个最直接的答案就是：确定一个百分比，p%，使得给定任意数据集，参数的估计值（estimation）落在这个邻域内的概率为p%。那么，确定邻域大小的问题就变成了确定**参数估计量**（estimator）的分布的问题了。

首先，如果我们假设总体服从**正态分布**。那么可以证明，统计量作为随机变量的函数，往往会服从从正态分布中推导出来的一系列分布（如t分布，chi-square分布和F分布），那么通过统计量（estimator）的分布，我们可以很轻松的得到所求邻域的大小。

但是，在日常生活中，数据并不一定服从正态分布的。如果总体不是正态分布的，那么估计统计量（estimator）很可能也不服从正态分布，t分布，chi-square分布和F分布这些我们已知的分布。如果我们不知道统计量的分布，就无法确定应该给这个点估计包裹一个多大的邻域。

于是我们退而求其次，由于在满足一定正则条件的情况下，很多数据的分布都会在数据量趋近于无穷的情况下趋近于正态分布。比如，**中心极限定理**（Central Limit Theorem，以下简称CLT）说的是样本均值的极限分布为正态分布。而估计量一般可以表示成样本均值的函数（e.g. OLS，GMM）， 所以知道了样本均值的极限（正态）分布也就知道了这些估计量的极限分布。于是我们就可以计算区间估计中的置信区间了。

最后，如果正则条件不满足，CLT无法适用。数据分布即使在数据量趋于无穷的情况下仍然不是正态分布，这时候，采用传统方法得到区间估计的办法就行不通了。需要采用更加先进的方法（比如bootstrapping寻找区间估计；比如彻底抛弃parametric model转用semi- non-parametric model等等）。

其实CLT不单单在找区间估计的时候用到。很多**假设检验**的问题都依赖于统计量（或者数据等）的分布是正态分布这一假设。所以如果假设统计量本身就是正态的，那么当然可以以这些统计量为基础进行假设检验。但是如果分布不是正态的，那很有可能就需要CLT来帮助（至少建立在极限状态下的正态性）证明假设检验（包括区间估计）的正当性：因为如果统计量不是正态的，那么得出来的东西根本对不上号，假设检验也就没啥大意义了。

### 点估计

#### 矩估计

#### 极大似然估计

#### 估计量的评选标准

### 区间估计

#### 基本思路

1. 如果已知总体分布为正态分布，那么从抽样分布一节我们知道，其某些样本统计量满足正态分布，chi-square分布，t分布和F分布。
2. 知道样本统计量的分布对我们来说是非常有帮助的，即使在我们不知道分布参数的情况下。比如我们知道了总体$$X\sim N\left( \mu ,{ \sigma  }^{ 2 } \right)$$，那么样本均值$$\bar { X } \sim N\left( \mu ,\frac { { \sigma  }^{ 2 } }{ n }  \right)$$，如果我们知道σ和n，那么只有一个未知参数μ不知道，这个μ也是我们要估计的未知参数。
3. 由3σ原则我们知道，如果我们得到一个样本，并求出样本均值的观察值$$\bar { { X }_{ 0 } }$$，那么$$P\left( \mu -2\frac { \sigma  }{ \sqrt { n }  } <\bar { { X }_{ 0 } } <\mu +2\frac { \sigma  }{ \sqrt { n }  }  \right) \approx 95\%$$，我们再将这个式子变形即可得$$P\left( \bar { { X }_{ 0 } } -2\frac { \sigma  }{ \sqrt { n }  } <\mu <\bar { { X }_{ 0 } } +2\frac { \sigma  }{ \sqrt { n }  }  \right) \approx 95\%$$，这个式子的意思就是：**我们要估计的未知参数μ在**$$\left( \bar { { X }_{ 0 } } -2\frac { \sigma  }{ \sqrt { n }  } ,\bar { { X }_{ 0 } } +2\frac { \sigma  }{ \sqrt { n }  }  \right)$$**这个区间内的概率约等于95%。**
4. 3中的结论，我们可以换一种等价的说法或思考的角度，也就是说：**如果我们要求未知参数的真值在某个区间内的概率为95%，那么这个区间就得是**$$\left( \bar { { X }_{ 0 } } -2\frac { \sigma  }{ \sqrt { n }  } ,\bar { { X }_{ 0 } } +2\frac { \sigma  }{ \sqrt { n }  }  \right)$$。这个95%叫做**置信水平1-α**，α为**弃真错误**，得到的区间成为**置信区间（Confidence interval）**，区间的上下限称为**置信上限**和**置信下限**。
5. 但是3σ原则毕竟有局限性，我们也不能每次估计时都要求是95%的置信水平，所以为了更精确地估计，我们需要知道置信水平和置信区间的具体关系。
6. 我们回到3中，当我们知道$$\bar { X } \sim N\left( \mu ,\frac { { \sigma  }^{ 2 } }{ n }  \right)$$，$$\bar{X}$$是一个统计量，我们可以进一步知道$\frac { \bar { X } -\mu  }{ { \sigma  }/{ \sqrt { n }  } } \sim N\left\( 0,1 \right\) $$，$$\frac { \bar { X } -\mu  }{ { \sigma  }/{ \sqrt { n }  } } $$是一个我们构造的统计量z，因为其分布是更加简单的单位正态分布，所以我们用它来分析。其概率密度图如下：

![&#x5C4F;&#x5E55;&#x5FEB;&#x7167; 2019-05-06 &#x4E0A;&#x5348;11.31.18](http://strawberryamoszc.oss-cn-shanghai.aliyuncs.com/2019-06-15-093635.png)

1. 那么当我们有了置信水平α以及z的定义，我们可以得到一个类似的等式：$$P\left( -{ z }_{ { \frac { \alpha  }{ 2 }  } }<z<{ z }_{ { \frac { \alpha  }{ 2 }  } } \right) =1-\alpha$$，这个等式就将α和z联系起来，将$$z=\frac { \bar { X } -\mu  }{ { \sigma  }/{ \sqrt { n }  } }$$代入该式，并做等价变换可得$$P\left( \bar { X } -{ z }_{ { \frac { \alpha  }{ 2 }  } }\frac { \sigma  }{ \sqrt { n }  } <\mu <\bar { X } +{ z }_{ { \frac { \alpha  }{ 2 }  } }\frac { \sigma  }{ \sqrt { n }  }  \right) =1-\alpha$$，这样在$$\alpha$$，$$\bar {X}$$，$$\sigma$$，和n都已知的情况下，用这四个量表示出了未知参数μ的置信区间。
2. 整理思路，我们如果知道总体分布为正态分布，并且知道其标准差σ和样本容量n，当我们给出置信区间1-α情况下，利用得到的一个样本计算出其样本均值$$\bar {X}$$，我们通过构造统计量z结合其分布性质，就可以得到未知参数μ的置信区间，也就是对μ进行了区间估计：

![&#x533A;&#x95F4;&#x4F30;&#x8BA1;2](http://strawberryamoszc.oss-cn-shanghai.aliyuncs.com/2019-06-15-093758.png)

1. 以上是我们对特定总体情况的区间估计，这个特定情况是总体服从正态分布且已知方差，但是，实际中可能我们并不知道方差，或者我们知道均值想估计方差，亦或是均值和方差都不知道，甚至总体可能都不服从正态分布，这个时候我们就不能通过样本均值这个统计量来进行参数估计了，就需要通过其他一些统计量，比如chi-squre、t等等。但我们有[区间估计与假设检验表](http://strawberryamoszc.oss-cn-shanghai.aliyuncs.com/2019-06-15-%E5%8C%BA%E9%97%B4%E4%BC%B0%E8%AE%A1%E4%B8%8E%E5%81%87%E8%AE%BE%E6%A3%80%E9%AA%8C%E5%85%AC%E5%BC%8F%E8%A1%A8.doc)，通过这个表我们可以快速判断自己应该使用哪个统计量以及估计或统计方法，这是十分方便的。
2. 样本容量越大，置信区间越窄；置信度越高，置信区间越宽。

## 假设检验

### 基本思路

假设检验的基本思路其实与参数估计很相似，所以笔者暂时没有详述，后续会补充完整。笔者将参数估计的重点放在了代码实现上，也就是诸多假设检验如何在R语言中实现，请参考R部分的parameter estimation and hypothesis testing部分，另外，假设检验学习完之后，笔者认为，学习回归分析或计量经济学是对参数估计和假设检验这两部分知识最好的应用，笔者也在Github上上传了计量经济学的学习笔记的思维导图文件，有需要的同学可以下载参考。具体链接请查阅主目录README。

### 正太总体-均值

#### 单个正太总体-均值

**方差已知-Z检验**

**方差未知-t检验**

#### 两个正太总体-均值差-t检验

#### 基于成对数据的检验-t检验

### 正太总体-方差

#### 单个总体-chi-square检验

#### 两个总体-F检验

### 置信区间与假设检验之间的关系

### 样本容量的选取

### 分布拟合检验

上面介绍的各种检验方法都是在总体分布形式为已知的前提下进行的讨论，但在实际问题中，有时不能知道总体服从什么类型的分布，这时就要根据样本来检验关于分布的假设。

这里通过$${ \chi }^{ 2 }$$拟合检验法，来检验总体是否具有某一个指定的分布或属于某一个分布族（即分布中有未知参数）。

还介绍专用于检验分布是否为正态的偏度和峰度检验法。

#### 卡方拟合检验法

**假设前提**

**记：**$$F\left( x \right)$$为总体X的位置的分布函数。

**假设：**$${ F }_{ 0 }\left( x \right)$$是形式已知，但可能含有若干个未知参数的分布函数。

**检验假设：**$${ H }_{ 0 }:F\left( x \right) ={ F }_{ 0 }\left( x \right) \quad \forall x\in R$$

**注意：**

1. 一般比如检验是否符合孟德尔遗传定律这种没有参数的，但大多是检验有参数的分布，比如检验是否符合泊松分布，泊松分布是有参数λ的，这类有参数的分布拟合假设也叫做分布族的χ^2^拟合检验。
2. 若总体X为离散型，则原假设$${H}_{ 0 }$$：总体X的分布律为$$P\left\{ X={ t }_{ i } \right\} ={ p }_{ i },i=1,2,…$$
3. 若总体X为连续型，则原假设$${H}_{ 0 }$$：总体X的概率密度为$$f\left( x \right)$$
4. 备择假设是除了这个分布之外所有的分布，故不用写出。

**拟合优度检验的基本原理和步骤**

1. 在$${H}_{ 0 }$$下，总体X取值的全体分成k组，即k个两两不想交的子集$${ A }_{ 1 },...,{ A }_{ k }$$.
2. 以$${ n }_{ i }\left( i=1,...k \right)$$记样本观察值$${ x }_{ 1 },...,{ x }_{ n }$$中落$${ A }_{ i }$$内的个数（**实际频数**），且$$\sum _{ i=1 }^{ k }{ { n }_{ i } } =n$$.
3. 当$${H}_{ 0 }$$为真且$${ F }_{ 0 }\left( x \right)$$完全已知时，计算事件$${ A }_{ i }$$发生的概率$${ p }_{ i }={ P }_{ { F }_{ 0 } }\left( { A }_{ i } \right) ,i=1,...k$$；

   当$${ F }_{ 0 }\left( x \right)$$含有r个未知参数时，先利用**极大似然法**估计r个未知参数，然后求得$${ p }_{ i }$$的估计$${ \hat { p } }_{ i }$$.

   此时称$${ n }{ p }_{ i }$$（或$${ n\hat { p } }_{ i }$$）为**理论频数**.

4. 直观来看，如果$${H}_{ 0 }$$~成立，实际频数$${n}_{i}$$与理论频数$${ n }{ p }_{ i }$$相差不会太大，基于这种想法，我们会选择：

   **检验统计量形式**： $$\sum _{ i=1 }^{ k }{ { h }_{ i }{ \left( { n }_{ i }-n{ p }_{ i } \right) }^{ 2 } } ,{ h }_{ i }=?$$

   **检验的拒绝域形式**： $$\sum _{ i=1 }^{ k }{ { h }_{ i }{ \left( { n }_{ i }-n{ p }_{ i } \right) }^{ 2 } } \ge c$$

   **统计量分布**：若n充分大，则当$${H}_{ 0 }$$~为真时，统计量

   ​ $${ \chi }^{ 2 }=\sum _{ i=1 }^{ k }{ \frac { { h }_{ i }{ \left( { n }_{ i }-n{ p }_{ i } \right) }^{ 2 } }{ n{ p }_{ i } } } \overset { 近似 }{ \sim } { \chi }^{ 2 }\left( k-1 \right)$$

   ​ $${ \chi }^{ 2 }=\sum _{ i=1 }^{ k }{ \frac { { h }_{ i }{ \left( { n }_{ i }-n{ \hat { p } }_{ i } \right) }^{ 2 } }{ n{ \hat { p } }_{ i } } } \overset { 近似 }{ \sim } { \chi }^{ 2 }\left( k-r-1 \right)$$

   其中k为分类数，r为$${ F }_{ 0 }\left( x \right)$$中被估未知参数的个数。

   **检验统计量**:

   $${ \chi }^{ 2 }=\sum _{ i=1 }^{ k }{ \frac { { h }_{ i }{ \left( { n }_{ i }-n{ p }_{ i } \right) }^{ 2 } }{ n{ p }_{ i } } } =\sum _{ i=1 }^{ k }{ \frac { { { n }_{ i } }^{ 2 } }{ n{ p }_{ i } } } -n$$ 或

   $${ \chi }^{ 2 }=\sum _{ i=1 }^{ k }{ \frac { { h }_{ i }{ \left( { n }_{ i }-n{ \hat { p } }_{ i } \right) }^{ 2 } }{ n{ \hat { p } }_{ i } } } =\sum _{ i=1 }^{ k }{ \frac { { { n }_{ i } }^{ 2 } }{ n{ \hat { p } }_{ i } } } -n$$

   **显著水平**$$\alpha$$**下拒绝域**：

   $${ \chi }^{ 2 }=\sum _{ i=1 }^{ k }{ \frac { { { n }_{ i } }^{ 2 } }{ n{ p }_{ i } } } -n\ge { \chi }_{ \alpha }^{ 2 }\left( k-1 \right)$$，（没有参数需要估计）

   $${ \chi }^{ 2 }=\sum _{ i=1 }^{ k }{ \frac { { { n }_{ i } }^{ 2 } }{ n{ \hat { p } }_{ i } } } -n\ge { \chi }_{ \alpha }^{ 2 }\left( k-r-1 \right)$$， （有r个参数需要估计）

5. **卡方拟合检验使用时必须注意：**
   * n要足够大，n≥50；
   * $$n{ p }_{ i }(或n{ { \hat { p }  }_{ i } })\ge 5$$；
   * 否则应适当合并相邻的类（组），以满足要求。

#### 偏度、峰度检验

### 秩和检验

### 假设检验问题的P值法

`p-value`是指在一个概率模型中，统计摘要（如两组样本均值差）与实际观测数据相同，或甚至更大这一事件发生的概率。换言之，是检验假设零假设成立或表现更严重的可能性。p-value若与选定显著性水平（0.05或0.01）相比更小，则零假设会被否定而不可接受。然而这并不直接表明原假设正确。通常在零假设下，`p-value`是一个服从 \[0,1\] 区间均匀分布的随机变量，在实际使用中因样本等各种因素存在不确定性。产生的结果可能会带来争议。

简单来说，`p-value`就是**在假设原假设（**$${H}_{ 0 }$$**）正确时，出现现状或更差的情况的概率。**

从研究总体中抽取一个随机样本计算检验统计量的值计算概率`p-value`或者说观测的显著水平，即在假设为真时的前提下，检验统计量大于或等于实际观测值的概率，当然要看假设的情况选择单侧`p-value`和双侧`p-value`:

1. 如果`p-value<0.01`，说明是较强的判定结果，拒绝假定的参数取值。
2. 如果`0.01<p-value<0.05`，说明较弱的判定结果，拒绝假定的参数取值。
3. 如果`p-value>0.05`，说明结果更倾向于接受假定的参数取值。

可是，那个年代，由于硬件的问题，计算`p-value`并非易事，人们就采用了统计量检验方法，也就是我们最初学的t值和t临界值比较的方法。统计检验法是在检验之前确定显著性水平$$\alpha$$，也就是说事先确定了拒绝域。但是，如果选中相同的$$\alpha$$，所有检验结论的可靠性都一样，无法给出观测数据与原假设之间不一致程度的精确度量。只要统计量落在拒绝域，假设的结果都是一样，即结果显著。但实际上，统计量落在拒绝域不同的地方，实际上的显著性有较大的差异。因此，随着计算机的发展，P值的计算不再是个难题，使得P值变成最常用的统计指标之一。

> 以上关于p-value内容分别摘录自[Wikipedia](https://zh.wikipedia.org/wiki/P%E5%80%BC)，[知乎](https://www.zhihu.com/question/23149768/answer/23751377)和[百度百科](https://baike.baidu.com/item/P%E5%80%BC)。

## 方差分析

### 单因素试验的方差分析

> 本小节内容摘录自[知乎](https://zhuanlan.zhihu.com/p/57896471)。

#### 背景

首先来说说我们为什么要用单因素方差分析（one-way ANOVA\)。在做一些实验时，我们通常会把样本分成不同的组，给予不同的对待。例如，我们想研究某种药物在不同剂量下对人们的作用。我们可能会将病人随机分为同等大小的三组，A组每天吃一片，B组每天吃两片，C组每天吃三片。因为我们只研究这个药品计量对病人的影响，所以是单因素分析，如果想要加入别的因素，例如，年龄，就需要用到多因素分析了。在上述实验中，我们给了三种不同的计量，所以这个药物计量因素下有三个水平（level）。实验结束以后，你老板问你，这三组病人的表现有显著的区别吗？这个时候，你就可以使用ANOVA来回答你老板的问题啦。

虽然ANOVA叫做方差分析，但是他的目的是**检验每个组的平均数是否相同**（敲黑板！）。也就是说，ANOVA的零假设（null hypothesis）是 ![\[&#x516C;&#x5F0F;\]](https://www.zhihu.com/equation?tex=H_0%3A+\mu_A+%3D+\mu_B+%3D+\mu_C) 。现在，我们换一个角度考虑这个问题，如果这三组病人的表现并没有显著的区别，那他们其实是同一个总体的三次随机抽样。反过来说，我们想要分析，是不是有一组病人他们的表现非常与众不同，让这组病人不是来自同一个总体。

#### 为什么是方差分析？

**为什么不直接比较均值？**

举个例子，$${A}_{1}$$组：29，30，31；$${A}_{2}$$组：3，31，41。$${A}_{1}$$组均值为30，$${A}_{2}$$组均值25，看起来$${A}_{1}$$组大一些，但实际上$${A}_{2}$$组有两个值都大于$${A}_{2}$$组。

这是因为，不同组极端值可能会影响到均值，从而给判断造成误导。

**为什么不用t检验？**

我们有一个样本后进行一次t检验，在这里，每一组就相当于一个样本，那么比如有三组， $${ C }_{ 3 }^{ 1 }$$就要做3次独立的t检验。但是t检验是每次给定一个显著性水平，比如我们给定α=0.05，也就是每次犯错的概率为0.05，那么每次不犯错的概率是0.95，三次不犯错的概率为0.95^3^=0.857375，那么我们犯错的概率就高达0.142625。

而方差分析是一次检验，犯错的概率就小很多，但方差分析也有局限性，它只能检验各组之间的均值是否有差异，并不能给出谁大谁小，所以，适当时候有必要方差分析后，再进行t检验。

#### 前提假设

在具体说如何理解ANOVA之前，我们先来说ANOVA有哪些假设。如果你的实验不能满足ANOVA的假设，那你需要考虑别的分析方法或者改变实验设计。ANOVA有主要有以下3个假设：

1. 方差的同质性（homogeneity of variance）。可以理解为每组样本背后的总体（也叫族群）都有相同的方差；
2. 族群遵循正态分布；
3. 每一次抽样都是独立的。在我们的例子中，每一个病人只能提供一个数据。对于一些实验一个样本需要提供多个数据，有其他相应的ANOVA分析方法。

#### 原假设

$${ H }_{ 0 }:{ \mu }_{ 1 }={ \mu }_{ 2 }=...={ \mu }_{ s }$$

$${ H }_{ 1 }:{ \mu }_{ 1 },{ \mu }_{ 2 },...,{ \mu }_{ s }$$不全相等

#### 平方和的分解

假设我们得到的抽样结果是这样的：

![v2-3979bc8df2c120d2515c1823104d9c94\_hd](http://strawberryamoszc.oss-cn-shanghai.aliyuncs.com/2019-06-19-142543.jpg)

![v2-25030ed4eae7894cfdc4370325383bb5\_hd](http://strawberryamoszc.oss-cn-shanghai.aliyuncs.com/2019-06-19-142837.jpg)

现在，我们终于可以来看方差分析。首先我们来看**单因素试验方差分析表**：

| 方差来源 | 平方和 | 自由度 | 均方 | F比 |
| :--- | :--- | :--- | :--- | :--- |
| 因素A | $${S}_{A}$$ | s-1 | $${ \bar { S }  }_{ A }=\frac { { S }_{ A } }{ s-1 }$$ | $$F=\frac { { \bar { S }  }_{ A } }{ { \bar { S }  }_{ E } }$$ |
| 误差 | $${S}_{E}$$ | n-s | $${ \bar { S }  }_{ E }=\frac { { S }_{ E } }{ n-s }$$ |  |
| 总和 | $${A}_{T}$$ | n-1\(即总样本方差自由度\) |  |  |

**总偏差平方和**：$${ S }_{ T }=\sum _{ j=1 }^{ s }{ \sum _{ i=1 }^{ { n }_{ j } }{ { \left( { { X }_{ ij } }-\bar { X } \right) }^{ 2 } } }$$

我们可以将其分解：$${ S }_{ T }=\sum _{ j=1 }^{ s }{ \sum _{ i=1 }^{ { n }_{ j } }{ { \left( { { X }_{ ij } }-\bar { X } \right) }^{ 2 } } } \\ =\sum _{ j=1 }^{ s }{ \sum _{ i=1 }^{ { n }_{ j } }{ { \left[ \left( { { X }_{ ij } }-{ \bar { X } }_{ \bullet j } \right) +\left( { \bar { X } }_{ \bullet j }-\bar { X } \right) \right] }^{ 2 } } } \\ =\sum _{ j=1 }^{ s }{ \sum _{ i=1 }^{ { n }_{ j } }{ { \left( { { X }_{ ij } }-{ \bar { X } }_{ \bullet j } \right) }^{ 2 } } } +\sum _{ j=1 }^{ s }{ \sum _{ i=1 }^{ { n }_{ j } }{ { \left( { \bar { X } }_{ \bullet j }-\bar { X } \right) }^{ 2 } } } \\ ={ S }_{ E }+{ S }_{ A }$$

$${S}_{E}$$**：误差平方和**

$${S}_{A}$$**：效应平方和**

#### 自由度

$${S}_{E}$$**：**比较简单的理解方法是，每组\(即每个j\)是 $${ n }_{ j }-1$$，s个组一共 **n-s** ；

$${S}_{A}$$**：**比较简单的理解方法是，将每组数据的均值看成一个数据，共s个，求这s个数据的方差，方差自由度为 **s-1** （实际需要严谨的证明）；

#### 分布与期望

由于$$\frac { \sum _{ i=1 }^{ { n }_{ 1 } }{ { \left( { X }_{ ij }-{ \bar { X } }_{ \bullet j } \right) }^{ 2 } } }{ { \sigma }^{ 2 } } \sim { \chi }^{ 2 }\left( { n }_{ j }-1 \right)$$，且各X~ij~相互独立，由卡方分布可加性知$$\frac { { S }_{ E } }{ { \sigma }^{ 2 } } \sim { \chi }^{ 2 }\left( { n }-s \right)$$ ，这也再次说明误差平方和的自由度为 **n-s** 。

故 $$E\left( { S }_{ E } \right) =\left( n-s \right) { \sigma }^{ 2 }$$

可以推出\(过程略\) $$E\left( { S }_{ A } \right) =\left( s-1 \right) { \sigma }^{ 2 }+\sum _{ j=1 }^{ s }{ { n }_{ j }{ \delta }_{ j }^{ 2 } }$$，其中 $${ \delta }_{ j }={ \mu }_{ j }-\mu$$ 。

进一步还有：

1. $${S}_{A}$$与$${S}_{E}$$独立；
2. 当$${H}_{0}$$为真时，$$\frac { { S }_{ A } }{ { \sigma  }^{ 2 } } \sim { \chi  }^{ 2 }\left( s-1 \right)$$ 。

#### 拒绝域

从上一小节分布和期望，我们可以总结以下几点：

1. $${S}_{A}$$与$${S}_{E}$$独立；
2. $$\frac { { S }_{ E } }{ { \sigma  }^{ 2 } } \sim { \chi  }^{ 2 }\left( { n }-s \right)$$，因此无论H~0~是否为真， $$E\left( { S }_{ E } \right) =\left( n-s \right) { \sigma  }^{ 2 }$$；
3. 只有当H~0~为真时，$$\frac { { S }_{ A } }{ { \sigma  }^{ 2 } } \sim { \chi  }^{ 2 }\left( s-1 \right)$$ ，$$E\left( { S }_{ A } \right) =\left( s-1 \right) { \sigma  }^{ 2 }$$；而当H~1~为真时，$$E\left( { S }_{ A } \right) =\left( s-1 \right) { \sigma  }^{ 2 }+\sum _{ j=1 }^{ s }{ { n }_{ j }{ \delta  }_{ j }^{ 2 } } >\left( s-1 \right) { \sigma  }^{ 2 }$$；

而两个独立方差一般用F检验，所以我们考虑**统计量**：

$$F=\frac { { { S }_{ A } }/{ \left( s-1 \right) } }{ { { S }_{ E } }/{ \left( n-s \right) } } ={ \frac { { { S }_{ A } }/{ { \sigma }^{ 2 } } }{ s-1 } }/{ \frac { { { { S }_{ E } }/{ { \sigma }^{ 2 } } } }{ n-s } }$$

也就是说，当$${H}_{0}$$不真$${H}_{1}$$为真时，分子的取值有偏大的趋势，于是**拒绝域形式**：

$$F=\frac { { { S }_{ A } }/{ \left( s-1 \right) } }{ { { S }_{ E } }/{ \left( n-s \right) } } \ge k$$

而由$${S}_{A}$$与$${S}_{E}$$独立，当$${H}_{0}$$为真时，统计量所满足的分布:

$$F\sim F\left( s-1,n-s \right)$$

于是，我们加上弃真概率α，可以得到**拒绝域**：

$$F=\frac { { { S }_{ A } }/{ \left( s-1 \right) } }{ { { S }_{ E } }/{ \left( n-s \right) } } \ge F\left( s-1,n-s \right)$$

其实，这里的分子和分母就是其他常见解释中的MSB和MSE：

| 平方和 | 表达式 | 均方 | 方差 | 简称 | 表达式 | 缩写 |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| $${S}_{A}$$ | $$\sum _{ i=1 }^{ s }{ \sum _{ j=1 }^{ { n }_{ j } }{ { \left( { X }_{ ij }-{ \bar { X }  }_{ \bullet j } \right)  }^{ 2 } }  }$$ | $$\frac { { S }_{ A } }{ s-1 }$$ | MSB | 组间方差 | $$\frac { \sum _{ i=1 }^{ s }{ \sum _{ j=1 }^{ { n }_{ j } }{ { \left( { X }_{ ij }-{ \bar { X }  }_{ \bullet j } \right)  }^{ 2 } }  }  }{ n-s }$$ | Mean Square Between |
| $${S}_{E}$$ | $$\sum _{ i=1 }^{ s }{ \sum _{ j=1 }^{ { n }_{ j } }{ { \left( { \bar { X }  }_{ \bullet j }-\bar { X }  \right)  }^{ 2 } }  }$$ | $$\frac { { S }_{ E } }{ n-s }$$ | MSE | 组内方差 | $$\frac { \sum _{ i=1 }^{ s }{ \sum _{ j=1 }^{ { n }_{ j } }{ { \left( { \bar { X }  }_{ \bullet j }-\bar { X }  \right)  }^{ 2 } }  }  }{ s-1 }$$ | Mean Square Error |

### 双因素试验的方差分析

#### 双因素等重复试验的方差分析—有相互作用

**前提**

1. A，B两因素作用与试验的指标；
2. A有r个水平；
3. B有s个水平；
4. 对A，B的水平的每对组合都做t（t≥2）次试验（称为等重复试验）。
5. A，B之间可能有相互作用。

**参数与记号**

$${ X }_{ ijk }\sim N\left( { \mu }_{ ij },{ \sigma }^{ 2 } \right) ,i=1,2,...,r;j=1,2,...,s;k=1,2,...t$$

μ~ij~，σ^2^均为未知参数

总平均 $$\mu =\frac { 1 }{ rs } \sum _{ i=1 }^{ r }{ \sum _{ j=1 }^{ s }{ { \mu }_{ ij } } }$$

$${ \mu }_{ i\bullet }=\frac { 1 }{ s } \sum _{ j=1 }^{ s }{ { \mu }_{ ij } } ,i=1,2,...r$$

$${ \mu }_{ \bullet j }=\frac { 1 }{ r } \sum _{ i=1 }^{ r }{ { \mu }_{ ij } } ,j=1,2,...s$$

水平A~i~的效应 $${ \alpha }_{ i }={ \mu }_{ i\bullet }-\mu ,i=1,2,...r$$

水平B~j~的效应 $${ \beta }_{ j }={ \mu }_{ \bullet j }-\mu ,j=1,2,...s$$

$${ \mu }_{ ij }=\mu +{ \alpha }_{ i }+{ \beta }_{ j }+\left( { \mu }_{ ij }-{ \mu }_{ i\bullet }-{ \mu }_{ \bullet j }+\mu \right) =\mu +{ \alpha }_{ i }+{ \beta }_{ j }+{ \gamma }_{ ij }$$

$$\sum _{ i=1 }^{ r }{ { a }_{ i } } =0$$

$$\sum _{ j=1 }^{ s }{ { \beta }_{ j } } =0$$

$$\sum _{ i=1 }^{ r }{ { \gamma }_{ ij } } =0,j=1,2,...,s$$

$$\sum _{ j=1 }^{ s }{ { \gamma }_{ ij } } =0,i=1,2,...,r$$

**总结**：

$${ X }_{ ijk }=\mu +{ \alpha }_{ i }+{ \beta }_{ j }+{ \gamma }_{ ij }+{ \varepsilon }_{ ijk }$$

$${ \varepsilon }_{ ijk }\sim N\left( 0,{ \sigma }^{ 2 } \right)$$，各 ε~ijk~ 独立

$$i=1,2,...,r;j=1,2,...,s;k=1,2,...t$$

$$\sum _{ i=1 }^{ r }{ { a }_{ i } } =0,\sum _{ j=1 }^{ s }{ { \beta }_{ j } } =0,\sum _{ i=1 }^{ r }{ { \gamma }_{ ij } } =0,\sum _{ j=1 }^{ s }{ { \gamma }_{ ij } } =0$$

**假设**

$${ H }_{ 01 }:{ \alpha }_{ 1 }={ \alpha }_{ 2 }=...={ \alpha }_{ r }=0\\ { H }_{ 11 }:{ \alpha }_{ 1 },{ \alpha }_{ 2 },...{ \alpha }_{ r }不全为0$$

$${ H }_{ 02 }:{ \beta }_{ 1 }={ \beta }_{ 2 }=...={ \beta }_{ s }=0\\ { H }_{ 12 }:\beta _{ 1 },{ \beta }_{ 2 },...{ \beta }_{ s }不全为0$$

$${ H }_{ 03 }:{ \gamma }_{ 11 }={ \gamma }_{ 12 }=...={ \gamma }_{ rs }=0\\ { H }_{ 13 }:{ \gamma }_{ 11 },{ \gamma }_{ 12 },...{ \gamma }_{ rs }不全为0$$

**双因素试验的方差分析表**

与单因素情况类似，对这些问题的检验方法也是建立在平方和的分解上的，思路是一样的，但由于较复杂，我们直接给出方差分析表：

| 方差来源 | 平方和 | 自由度 | 均方 | F比 |
| :--- | :--- | :--- | :--- | :--- |
| 因素A | $${S}_{A}$$ | r-1 | $${ \bar { S }  }_{ A }=\frac { { S }_{ A } }{ r-1 }$$ | $${ F }_{ A }=\frac { { \bar { S }  }_{ A } }{ { \bar { S }  }_{ E } }$$ |
| 因素B | $${S}_{B}$$ | s-1 | $${ \bar { S }  }_{ B }=\frac { { S }_{ B } }{ s-1 }$$ | $${ F }_{ B }=\frac { { \bar { S }  }_{ B } }{ { \bar { S }  }_{ E } }$$ |
| 交互作用 | $${S}_{A×B}$$ | \(r-1\)\(s-1\) | $${ \bar { S }  }_{ A×B }=\frac { { S }_{ A×B } }{ \left( r-1 \right) \left( s-1 \right)  }$$ | $${ F }_{ A×B }=\frac { { \bar { S }  }_{ A×B } }{ { \bar { S }  }_{ E } }$$ |
| 误差 | $${S}_{E}$$ | rs\(t-1\) | $${ \bar { S }  }_{ E }=\frac { { S }_{ E } }{ rs\left( t-1 \right)  }$$ |  |
| 总和 | $${S}_{T}$$ | rst-1 |  |  |

**拒绝域**

同单因素方差分析类似，这里只做总结：

1. 当$${H}_{01}$$为真时，可以证明 $${ F }_{ A }=\frac { { { S }_{ A } }/{ \left( r-1 \right) } }{ { { S }_{ E } }/{ \left( rs\left( t-1 \right) \right) } } \sim F\left( r-1,rs\left( t-1 \right) \right)$$

   取显著性水平为α，得到假设$${H}_{01}$$的拒绝域为：$${ F }_{ A }=\frac { { { S }_{ A } }/{ \left( r-1 \right) } }{ { { S }_{ E } }/{ \left( rs\left( t-1 \right) \right) } } \ge F\left( r-1,rs\left( t-1 \right) \right)$$

2. 当$${H}_{02}$$为真时，可以证明 $${ F }_{ B }=\frac { { { S }_{ B } }/{ \left( s-1 \right) } }{ { { S }_{ E } }/{ \left( rs\left( t-1 \right) \right) } } \sim F\left( s-1,rs\left( t-1 \right) \right)$$

   取显著性水平为α，得到假设$${H}_{02}$$的拒绝域为：$${ F }_{ B }=\frac { { { S }_{ B } }/{ \left( s-1 \right) } }{ { { S }_{ E } }/{ \left( rs\left( t-1 \right) \right) } } \ge F\left( s-1,rs\left( t-1 \right) \right)$$

3. 当$${H}_{03}$$为真时，可以证明 $${ F }_{ A×B }=\frac { { { S }_{ A×B } }/{ \left( \left( r-1 \right) \left( s-1 \right) \right) } }{ { { S }_{ E } }/{ \left( rs\left( t-1 \right) \right) } } \sim F\left( \left( r-1 \right) \left( s-1 \right) ,rs\left( t-1 \right) \right)$$

   取显著性水平为α，得到假设$${H}_{03}$$的拒绝域为：$${ F }_{ A×B }=\frac { { { S }_{ A×B } }/{ \left( \left( r-1 \right) \left( s-1 \right) \right) } }{ { { S }_{ E } }/{ \left( rs\left( t-1 \right) \right) } } \ge F\left( \left( r-1 \right) \left( s-1 \right) ,rs\left( t-1 \right) \right)$$

#### 双因素无重复试验的方差分析—无相互作用

**前提**

1. A，B两因素作用与试验的指标；
2. A有r个水平；
3. B有s个水平；
4. 对A，B的水平的每对组合都做1次试验。
5. A，B之间不存在相互作用或很小可以忽略。

**参数与记号**

$${ X }_{ ij }=\mu +{ \alpha }_{ i }+{ \beta }_{ j }+{ \varepsilon }_{ ij }$$

$${ \varepsilon }_{ ijk }\sim N\left( 0,{ \sigma }^{ 2 } \right)$$，各 ε~ijk~ 独立

$$i=1,2,...,r;j=1,2,...,s$$

$$\sum _{ i=1 }^{ r }{ { a }_{ i } } =0,\sum _{ j=1 }^{ s }{ { \beta }_{ j } } =0$$

**假设**

$${ H }_{ 01 }:{ \alpha }_{ 1 }={ \alpha }_{ 2 }=...={ \alpha }_{ r }=0\\ { H }_{ 11 }:{ \alpha }_{ 1 },{ \alpha }_{ 2 },...{ \alpha }_{ r }不全为0$$

$${ H }_{ 02 }:{ \beta }_{ 1 }={ \beta }_{ 2 }=...={ \beta }_{ s }=0\\ { H }_{ 12 }:\beta _{ 1 },{ \beta }_{ 2 },...{ \beta }_{ s }不全为0$$

**双因素试验的方差分析表**

与单因素情况类似，对这些问题的检验方法也是建立在平方和的分解上的，思路是一样的，但由于较复杂，我们直接给出方差分析表：

| 方差来源 | 平方和 | 自由度 | 均方 | F比 |
| :--- | :--- | :--- | :--- | :--- |
| 因素A | $${S}_{A}$$ | r-1 | $${ \bar { S }  }_{ A }=\frac { { S }_{ A } }{ r-1 }$$ | $${ F }_{ A }=\frac { { \bar { S }  }_{ A } }{ { \bar { S }  }_{ E } }$$ |
| 因素B | $${S}_{B}$$ | s-1 | $${ \bar { S }  }_{ B }=\frac { { S }_{ B } }{ s-1 }$$ | $${ F }_{ B }=\frac { { \bar { S }  }_{ B } }{ { \bar { S }  }_{ E } }$$ |
| 误差 | $${S}_{E}$$ | **\(r-1\)\(s-1\)** | $${ \bar { S }  }_{ E }=\frac { { S }_{ E } }{ \left( r-1 \right) \left( s-1 \right)  }$$ |  |
| 总和 | $${S}_{T}$$ | **rs-1** |  |  |

**拒绝域**

同单因素方差分析类似，这里只做总结：

1. 当$${H}_{01}$$为真时，可以证明 $${ F }_{ A }=\frac { { { S }_{ A } }/{ \left( r-1 \right) } }{ { { S }_{ E } }/{ \left( \left( r-1 \right) \left( s-1 \right) \right) } } \sim F\left( r-1,\left( r-1 \right) \left( s-1 \right) \right)$$

   取显著性水平为α，得到假设$${H}_{01}$$的拒绝域为：$${ F }_{ A }=\frac { { { S }_{ A } }/{ \left( r-1 \right) } }{ { { S }_{ E } }/{ \left( \left( r-1 \right) \left( s-1 \right) \right) } } \ge F\left( r-1,\left( r-1 \right) \left( s-1 \right) \right)$$

2. 当$${H}_{02}$$为真时，可以证明 $${ F }_{ B }=\frac { { { S }_{ B } }/{ \left( s-1 \right) } }{ { { S }_{ E } }/{ \left( \left( r-1 \right) \left( s-1 \right) \right) } } \sim F\left( s-1,\left( r-1 \right) \left( s-1 \right) \right)$$

   取显著性水平为α，得到假设$${H}_{02}$$的拒绝域为：$${ F }_{ B }=\frac { { { S }_{ B } }/{ \left( s-1 \right) } }{ { { S }_{ E } }/{ \left( \left( r-1 \right) \left( s-1 \right) \right) } } \ge F\left( s-1,\left( r-1 \right) \left( s-1 \right) \right)$$

