# about ggplot2

更多关于ggplot2作图请参考：[r-statistics.co]([http://r-statistics.co/Top50-Ggplot2-Visualizations-MasterList-R-Code.html](http://r-statistics.co/Top50-Ggplot2-Visualizations-MasterList-R-Code.html))

## 功能

自带`plot()`函数也可以作图，但不好看。如`plot(mtcars$mpg, mtcars$wt)`:

![image-20190607102621087](http://strawberryamoszc.oss-cn-shanghai.aliyuncs.com/2019-06-08-160913.jpg)

而ggplot2包中的函数可以做出好看的图像。如`ggplot(mtcars, aes(mpg, wt)) + geom_point()`:

![image-20190607102955634](http://strawberryamoszc.oss-cn-shanghai.aliyuncs.com/2019-06-08-160926.jpg)

## 要素

### 主要要素

背景、坐标轴、图形、标题、图例、分面、文本注释

### 标准代码

`ggplot(data, aes(x, y)) + geom_xx() + annotate() + labs() + facet_grid() +... `

## 理念

- **核心理念**：将绘图与数据分离；
- 命令式作图的调整函数，可以随时更换参数来调整图形，更加具有灵活性。

1. `ggplot(data, aes(x, y))` ：初始化图形并指定数据源和作图变量；只有背景框，没有图形。
2. `geom_xx()` : 指定图形的类型。
3. `geom_xx() `：添加文本注释。
4. `labs()` ：修改主标题和坐标轴标题。
5. `facet_grid()` ：分面函数，ggplot2的数据分面就是根据数据中的不同分组,绘制多个图形。

## 逻辑

- ggplot2 按照图层叠加作图，通过 + 号叠加，越到后面图层越高；
- ggplot2 作图符合人的认知，有明确的起点和终点，由ggplot开始做图，可以在后面的任一叠加函数停止做图。

# 作图类型

## 散点图和气泡图

描绘两组变量的关系情况。

**散点图**：`ggplot(data, aes(x, y)) + geom_point()`

**气泡图**：`ggplot(mtcars, aes(wt, mpg, size=qsec)) + geom_point(color="orange")`

更多和气泡图请查看[博客园](https://www.cnblogs.com/ljhdo/p/5042726.html)。

## 线图

`ggplot(mtcars, aes(wt, mpg)) + geom_line()`

点线图：

`ggplot(mtcars, aes(wt, mpg)) + geom_line() +geom_point()`

## 柱状图

查看变量的频数分布情况，一般是非连续型的，这和直方图有区分。

`ggplot(mtcars, aes(cyl)) + geom_bar()`

![image-20190607105332213](http://strawberryamoszc.oss-cn-shanghai.aliyuncs.com/2019-06-08-160930.jpg)

这里我们可以用`table()`函数求频数分布表来检查一下：

```R
> table(mtcars$cyl)

 4  6  8 
11  7 14 
```

但如上所述，柱状图必须是分类变量而不是连续型（数值型），所以首先要改变成类型`factor`：`ggplot(mtcars, aes(factor(cyl))) + geom_bar()` ：

![image-20190607105851484](http://strawberryamoszc.oss-cn-shanghai.aliyuncs.com/2019-06-08-160916.jpg)

**分组变量**

`ggplot(mtcars, aes(factor(cyl), fill=factor(am))) + geom_bar()` :

![image-20190607110433541](http://strawberryamoszc.oss-cn-shanghai.aliyuncs.com/2019-06-08-160936.jpg)

**纵向排列**

`ggplot(mtcars, aes(factor(cyl), fill=factor(am))) + geom_bar(position = "dodge")`

![image-20190607110703990](http://strawberryamoszc.oss-cn-shanghai.aliyuncs.com/2019-06-08-160919.jpg)

**百分比堆积柱形图**

`ggplot(mtcars, aes(factor(cyl), fill=factor(am))) + geom_bar(position = "fill")`

![image-20190607110918997](http://strawberryamoszc.oss-cn-shanghai.aliyuncs.com/2019-06-08-160935.jpg)

## 直方图

查看变量的频数分布情况。

`ggplot(mtcars, aes(mpg)) + geom_histogram(binwidth = 1)`

![image-20190607113409974](http://strawberryamoszc.oss-cn-shanghai.aliyuncs.com/2019-06-08-160922.jpg)

## 密度图

查看变量的频率分布情况

### 默认情况

`ggplot(mtcars, aes(mpg)) + geom_density()`

![image-20190607113821343](http://strawberryamoszc.oss-cn-shanghai.aliyuncs.com/2019-06-08-160923.jpg)

### color

以边界点、线来分组。

`ggplot(mtcars, aes(mpg, color = factor(vs))) + geom_density()`

![image-20190607113932812](http://strawberryamoszc.oss-cn-shanghai.aliyuncs.com/2019-06-08-160914.jpg)

### fill

填充来分组，可以设置透明度`alpha`来查看。

`ggplot(mtcars, aes(mpg, fill = factor(vs))) + geom_density(alpha = 0.5)`

![image-20190607114104190](http://strawberryamoszc.oss-cn-shanghai.aliyuncs.com/2019-06-08-160918.jpg)

## 箱线图

查看变量的统计值分布情况，描述统计。

`ggplot(mtcars, aes(factor(vs), mpg)) + geom_boxplot()`

![image-20190607115337961](http://strawberryamoszc.oss-cn-shanghai.aliyuncs.com/2019-06-08-160927.jpg)

更多设置请参考[博客园](https://www.cnblogs.com/ljhdo/p/4921588.html)。

# 分组作图

## 分组变量的数据类型

`ggplot()`中，`aes()`内参数是按照向量进行填充的，可以有x，y，fill/color，因为一般数据可视化也就三维。

这三维均可是离散型或连续型，有时候，可以将数值通过`factor()`函数转变成离散型变量。比如：

`ggplot(mtcars, aes(wt, mpg, color=qsec)) + geom_point()`

![image-20190607203839936](http://strawberryamoszc.oss-cn-shanghai.aliyuncs.com/2019-06-08-160925.jpg)



这里`qsec`就是连续型的，`ggplot()`用颜色深浅来表示数值大小；离散型的上面有提到。

## 映射参数

使用`aes()`函数来设置映射参数，`geom_point()`函数可以使用的映射有：

- x
- y
- alpha：设置点重叠部分的透明度
- colour：点的颜色
- fill：点的填充色
- group：分组
- shape：点形状
- size：点的大小
- stroke：描边

这些参数用于修改散点图的图形属性。 

比如气泡图：`ggplot(mtcars, aes(wt, mpg, size=qsec)) + geom_point(color="orange")`

![image-20190607205122055](http://strawberryamoszc.oss-cn-shanghai.aliyuncs.com/2019-06-08-160932.jpg)

# 分面作图

## 使轴刻度一致

坐标轴刻度是一致的。

### 单变量分面

#### 纵轴分面

`ggplot(mtcars, aes(wt, mpg)) + geom_point() + facet_grid(vs~.)`

![image-20190607231229633](http://strawberryamoszc.oss-cn-shanghai.aliyuncs.com/2019-06-08-160928.jpg)

#### 横轴分面

`ggplot(mtcars, aes(wt, mpg)) + geom_point() + facet_grid(.~vs)`

![image-20190607231326756](http://strawberryamoszc.oss-cn-shanghai.aliyuncs.com/2019-06-08-160920.jpg)

### 双变量分面

`ggplot(mtcars, aes(wt, mpg)) + geom_point() + facet_grid(am~vs)`

![image-20190607231444670](http://strawberryamoszc.oss-cn-shanghai.aliyuncs.com/2019-06-08-160929.jpg)

## 使轴刻度不一致

即有独立的坐标轴。

### 不同纵轴刻度

`ggplot(mtcars, aes(wt, mpg)) + geom_point() + facet_grid(vs~., scale="free_y")`

![image-20190607231749201](http://strawberryamoszc.oss-cn-shanghai.aliyuncs.com/2019-06-08-160934.jpg)

### 不同横轴刻度

`ggplot(mtcars, aes(wt, mpg)) + geom_point() + facet_grid(.~vs, scale="free_x")`

`ggplot(mtcars, aes(wt, mpg)) + geom_point() + facet_grid(.~vs, scale="free")`

![image-20190607231832308](http://strawberryamoszc.oss-cn-shanghai.aliyuncs.com/2019-06-08-160917.jpg)

# 调整图形元素

## 形状大小

`ggplot(mtcars, aes(wt, mpg, color=factor(vs))) + geom_point(shape=4, size=3)`

![image-20190607232239031](http://strawberryamoszc.oss-cn-shanghai.aliyuncs.com/2019-06-08-160924.jpg)

## 颜色

### color

描绘点线以及图形边缘的颜色、

### fill

填充图形内部的颜色(柱形图、密度图等)

### 统一指定颜色

指定填充一种颜色：直接在aes外部写`color="red"`

## 文本注释

`annotate()`函数

在`(4, 20)`位置注释一个`yes`：

```R
ggplot(mtcars, aes(wt, mpg, color=factor(vs))) + 
  geom_point() + 
  annotate("text", x=4, y=20, label="yes")
```

![image-20190607232912339](http://strawberryamoszc.oss-cn-shanghai.aliyuncs.com/2019-06-08-160915.jpg)

## 标题

`labs(title = " ", x = " ", y = " ")`

设定图表标题和x、y轴标题：

```R
ggplot(mtcars, aes(wt, mpg, color=factor(vs))) + 
  geom_point() + 
  annotate("text", x=4, y=20, label="yes") +
  labs(title = "This is a title", x = "x axis", y = "y axis")
```

![image-20190607233254772](http://strawberryamoszc.oss-cn-shanghai.aliyuncs.com/2019-06-08-160933.jpg)



## 添加线条

`geom_vline(xintercept = )`

`geom_hline(yintercept = )`

```R
ggplot(mtcars, aes(wt, mpg, color=factor(vs))) + 
  geom_point() + 
  annotate("text", x=4, y=20, label="yes") +
  labs(title = "Demo graph", x = "x axis", y = "y axis") +
  geom_vline(xintercept = 3.5) +
  geom_hline(yintercept = 22.5)
```

![image-20190607233621475](http://strawberryamoszc.oss-cn-shanghai.aliyuncs.com/2019-06-08-160921.jpg)



## 转换x，y轴

`coord_flip()`

<!--coordiante 协调；flip 翻转。-->

```R
ggplot(mtcars, aes(wt, mpg, color=factor(vs))) + 
  geom_point() + 
  annotate("text", x=4, y=20, label="yes") +
  labs(title = "Demo graph", x = "x axis", y = "y axis") +
  geom_vline(xintercept = 3.5) +
  geom_hline(yintercept = 22.5) +
  coord_flip()
```

![image-20190607233942048](http://strawberryamoszc.oss-cn-shanghai.aliyuncs.com/2019-06-08-160912.jpg)

## 调整轴刻度范围

`xlim(a, b)`

`ylim(a, b)`

注意，可能会丢掉部分数据，会发出`warning`。

```R
ggplot(mtcars, aes(wt, mpg, color=factor(vs))) + 
  geom_point() + 
  annotate("text", x=4, y=20, label="yes") +
  labs(title = "Demo graph", x = "x axis", y = "y axis") +
  geom_vline(xintercept = 3.5) +
  geom_hline(yintercept = 22.5) +
  xlim(3, 4) +
  ylim(20, 25)
```

![image-20190607234427928](http://strawberryamoszc.oss-cn-shanghai.aliyuncs.com/2019-06-08-160937.jpg)

## 修改轴上的值

`scale_x_continuous(breaks=c(), labels=c())`

`scale_y_continuous(breaks=c(), labels=c())`

最好是

```R
ggplot(mtcars, aes(wt, mpg, color=factor(vs))) + 
  geom_point() + 
  annotate("text", x=4, y=20, label="yes") +
  labs(title = "Demo graph", x = "x axis", y = "y axis") +
  geom_vline(xintercept = 3.5) +
  geom_hline(yintercept = 22.5) +
  scale_x_continuous(breaks=c(2, 3, 4, 5), labels=c("a", "b", "c", "d"))
```

![image-20190607234951046](http://strawberryamoszc.oss-cn-shanghai.aliyuncs.com/2019-06-08-160931.jpg)

更多关于标尺scale的设置，请参考[博客](https://www.zybuluo.com/K1999/note/426338)。