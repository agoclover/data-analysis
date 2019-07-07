# README

This repository records my personal experience of learning data analysis, which involves four main sections: **Statistics, Python, R and SQL**. 

All notes and files that you can easily get access to go through and download, have been uploaded to this repository. These notes refers to a large number of other scholars' research and blog materials which have been listed in detail.

Click this website to view the main notes: https://amos-pvt.gitbook.io/data-analysis/



# Statistics

统计学知识是数据分析的理论基础，尽管流行的数据分析工具比如Python和R都有很多现成的包可以使用，但理解其原理是至关重要的，否则很容易出现不考虑样本是否满足某些检验方法而错误套用所造成的严重后果。

这一部分主要包括三个方面，概率论和统计基础，以及线性回归——计量经济学。掌握好这些内容也就掌握了绝大多数数据分析所需要的理论知识。之后，所学习的数据挖掘内容会在其他仓库中记录和展示。

## Probability Theory

概率论是数理统计的基石。

本部分内容主要参考浙江大学《概率论与数理统计》和茆诗松《概率论与数理统计教程》，并以浙大教材目录为主。但笔记内容并不是全部照抄书本内容，而是对其中比较理论和抽象的概念和定理进行了比较形象和细致的阐述，算是个人经验的总结。

笔记 ↓

Probability Theory

## Statistics

这部分主要是统计学知识的复习，包含描述统计学和推论统计学。但是，为了更好地记录所学的知识，我将以下两部分的笔记内容放在了《概率论与数理统计》的框架内，以便形成更好的知识体系。

本部分内容主要参考优达学城数据分析的[入门课程](https://mubu.com/doc/2nOkGMljsl)和[进阶课程](https://mubu.com/doc/3rir3GN6Ll)，浙江大学《概率论与数理统计》和茆诗松《概率论与数理统计教程》。

笔记 ↓

[Statistics Notes](https://github.com/agoclover/data-analysis/blob/master/statistics/Statistics%20Notes.md)

## Econometrics

学习完统计学并理解和掌握参数估计和假设检验等重要工具之后，有必要学习计量经济学来**练兵**。而且，这一部分和凸优化的学习，能够很好地为机器学习做铺垫。

本部分内容以西南财经大学庞皓教授的[网课](https://www.bilibili.com/video/av16155564)为参考，以古扎拉蒂等的《计量经济学基础》第五版为参考教材。本部分内容以R语言实现，每一章结合若干案例进行分析和实现。笔记部分，由于这一章内容难度提高，本人使用xmind创建思维导图来更好地构建计量经济学框架。

笔记 ↓

1. [简单线性回归模型.xmind](https://github.com/agoclover/data-analysis/blob/master/statistics/2%E7%AE%80%E5%8D%95%E7%BA%BF%E6%80%A7%E5%9B%9E%E5%BD%92%E6%A8%A1%E5%9E%8B.xmind)
2. 多元线性回归模型.xmind
3. 多重共线性.xmind

案例以及R语言实现 ↓

Regression Models



# Python

### Fabio Nelli

refer to **Python Data Analytics** written by Fabio Nelli. Notes are below:

1. [chapter3\_NumPy](https://github.com/agoclover/data-analysis/blob/master/python/notes_chapter3_numpy.md)
2. [chapter4\_Pandas](https://github.com/agoclover/data-analysis/blob/master/python/notes_chapter4_pandas.md)
3. to be continued.

### Udacity

#### Python 数据分析入门

以udacity学生学习参与数据为例，数据集：

1. [enrollments.csv](https://github.com/agoclover/data-analysis/blob/master/python/enrollments.csv)
2. [daily\_engagement.csv](https://github.com/agoclover/data-analysis/blob/master/python/daily_engagement.csv)
3. [project\_submissions.csv](https://github.com/agoclover/data-analysis/blob/master/python/project_submissions.csv)

用Jupyter Notebook所做的数据分析报告：

[Getting started with data analysis.ipynb](https://github.com/agoclover/data-analysis/blob/master/python/Getting%20started%20with%20data%20analysis.ipynb)

#### NumPy 和 Pandas 分析一维数据

指导手册：

[numpy\_pandas\_cheatsheet.pdf](https://github.com/agoclover/data-analysis/blob/master/python/numpy_pandas_cheatsheet.pdf)

以就业率等数据为例，数据集：

1. [employment\_above\_15.csv](https://github.com/agoclover/data-analysis/blob/master/python/employment_above_15.csv)
2. [female\_completion\_rate.csv](https://github.com/agoclover/data-analysis/blob/master/python/female_completion_rate.csv)
3. [male\_completion\_rate.csv](https://github.com/agoclover/data-analysis/blob/master/python/male_completion_rate.csv)
4. [gdp\_per\_capita.csv](https://github.com/agoclover/data-analysis/blob/master/python/gdp_per_capita.csv)
5. [life\_expectancy.csv](https://github.com/agoclover/data-analysis/blob/master/python/life_expectancy.csv)

用Jupyter Notebook所做的数据分析报告：

[One\_dimensional\_data\_analysis.ipynb](https://github.com/agoclover/data-analysis/blob/master/python/One_dimensional_data_analysis.ipynb)

#### NumPy 和 Pandas 分析二维数据

以纽约地铁站客流量数据为例，数据集：

1. [nyc-subway-weather.csv](https://github.com/agoclover/data-analysis/blob/master/python/nyc-subway-weather.csv)
2. [nyc-subway-weather-descriptions.pdf](https://github.com/agoclover/data-analysis/blob/master/python/nyc-subway-weather-descriptions.pdf)

用Jupyter Notebook所做的数据分析报告：

[Two\_dimensional\_data\_analysis.ipynb](https://github.com/agoclover/data-analysis/blob/master/python/Two_dimensional_data_analysis.ipynb)

#### Project：探索数据集



# R

学习课程参考了[阿雷边学边教](https://www.bilibili.com/video/av6268508)，并自己做了大量的补充。从第六章回归分析开始都是自己总结的内容。

以下为课程笔记或资料：

1. [basic\_data\_management.md](https://github.com/agoclover/data-analysis/blob/master/R/notes_R_basic_data_management.md) 
2. [ggplot2.md](https://github.com/agoclover/data-analysis/blob/master/R/notes_R_ggplot2.md) 
3. [sample.md](https://github.com/agoclover/data-analysis/blob/master/R/notes_R_sample.md) 
4. [normal\_distribution.md](https://github.com/agoclover/data-analysis/blob/master/R/notes_R_4_normal_distribution.md) 
5. [parameter\_estimation\_hypothesis\_testing.md](https://github.com/agoclover/data-analysis/blob/master/R/notes_R_5_parameter_estimation_hypothesis%20_testing.md)
6. regression_models.md



# SQL

通过学习MySql数据库，并使用Navicat数据库图形化软件来学习SQL。

课程为[网易公开课-Mysql8数据库基础入门教程](https://study.163.com/course/courseMain.htm?courseId=1005932016)。

以下为课程笔记或资料：

1. [learning\_notes\_mysql.MD](https://github.com/agoclover/data-analysis/blob/master/sql/learning_notes_mysql.MD)
2. [GUIDE1.xmind](https://github.com/agoclover/data-analysis/blob/master/sql/GUIDE1.xmind)
3. [GUIDE2.xmind](https://github.com/agoclover/data-analysis/blob/master/sql/GUIDE2.xmind)

