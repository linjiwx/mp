#  用帕累托图进行数据分析

## 01

你好，我是每天都想学点新东西的林骥。

1897 年，意大利经济学家帕累托，在抽样调查的数据中发现，社会上 20% 的人拥有 80% 的财富。

后来，人们发现这种「关键少数」的现象非常普遍，比如说：20% 的原因导致 80% 的问题，20% 的产品贡献 80% 的业绩，20% 的员工贡献 80% 的业绩，20% 的客户贡献 80% 的业绩 …… 因此，简称为「二八法则」。

为了纪念帕累托，我们把展现「二八法则」的图表，称之为帕累托图。

下面举个例子，我们汇总导致质量问题的原因，计算每种原因出现的频次，然后按照从大到小进行排列，制作成一张帕累托图如下：

![ ](https://tva1.sinaimg.cn/large/007S8ZIlgy1ggfp2l5fhnj30so0l0gok.jpg)

与常见的帕累托图不同，我对图表细节做了一些调整：

（1）线条从坐标原点开始，代表累计百分比从 0 开始；

（2）没有使用双坐标轴，线条的高度就是频次的累计；

（3）灰色边框的高度就是频次的总和，以便展现部分与整体之间的占比关系；

（4）用虚线标记大致符合「二八法则」的位置；

（5）用文字标签说明累计百分比的具体数字，在标题中体现图表想要传递的信息。

**借助帕累托图，有助于我们抓住问题的关键，从而解决核心的问题。**

## 02

接下来，我们看看用 matplotlib 画图的具体步骤。

首先，导入所需的库，并设置中文字体和定义颜色等。

```python
# 导入所需的库
import numpy as np
import pandas as pd
import matplotlib as mpl
import matplotlib.pyplot as plt
import matplotlib.image as image

# 正常显示中文标签
mpl.rcParams['font.sans-serif'] = ['SimHei']

# 自动适应布局
mpl.rcParams.update({'figure.autolayout': True})

# 正常显示负号
mpl.rcParams['axes.unicode_minus'] = False

# 禁用科学计数法
pd.set_option('display.float_format', lambda x: '%.2f' % x) 

# 定义颜色，主色：蓝色，辅助色：灰色，互补色：橙色
c = {'蓝色':'#00589F', '深蓝色':'#003867', '浅蓝色':'#5D9BCF',
     '灰色':'#999999', '深灰色':'#666666', '浅灰色':'#CCCCCC',
     '橙色':'#F68F00', '深橙色':'#A05D00', '浅橙色':'#FBC171'}
```

其次，从 Excel 文件中读取数据，并定义画图用的数据。

```python
# 数据源路径
filepath='./data/帕累托图数据源.xlsx'

# 读取 Excel文件
df = pd.read_excel(filepath)

# 定义画图所需的数据
x = df['原因']
y = df['频次']

# 让折线图从坐标原点开始
x2 = np.arange(len(x)+1) - 0.5
# 计算累计频次
y_cumsum = [0] + list(y.cumsum())
# 计算累计百分比
y2 = y.cumsum()/y.sum()
```

接下来，开始用「面向对象」的方法进行画图。

```python
# 使用「面向对象」的方法画图
fig, ax = plt.subplots(figsize=(8, 6))

# 设置标题
ax.set_title('\n%.1f%%' % (y_cumsum[2]/y.sum()*100) + '的质量问题是由20%的原因引起的\n', 
             fontsize=26, loc='left', color=c['深灰色'])

# 用灰色方框代表总体的大小，体现每个数据的占比关系
ax.bar(x, y.sum(), width=1, color='w', edgecolor=c['浅灰色'], zorder=0)

# 画柱形图
ax.bar(x, y, width=1, color=c['蓝色'], edgecolor=c['浅灰色'], zorder=1)

# 画折线图
ax.plot(x2, y_cumsum, ls='-', lw=2, color=c['橙色'], label='累计百分比', zorder=2)

# 标记体现二八法则的虚线
ax.hlines(y_cumsum[2], -0.5, 1.5, color=c['橙色'], ls='--')
ax.vlines(1.5, 0, y_cumsum[2], color=c['橙色'], ls='--')

# 隐藏边框
ax.spines['top'].set_visible(False)
ax.spines['right'].set_visible(False)
ax.spines['bottom'].set_visible(False)
ax.spines['left'].set_visible(False)

# 设置图形的位置，减少空白
ax.spines['left'].set_position(('data', -0.51))

# 隐藏 X 轴的刻度线
ax.tick_params(axis='x', which='major', length=0)
ax.tick_params(axis='y', which='major', length=0)
ax.set_yticklabels([])

# 设置坐标标签字体大小和颜色
ax.tick_params(labelsize=16, colors=c['深灰色'])

# 设置数据标签
for a, a2, b, b2, b3 in zip(x, x2[1:], y, y_cumsum[1:], y2):
    ax.text(a, b, '%.0f' % b, ha='center', va= 'bottom', fontsize=16, color=c['蓝色'])
    

# 标记 Y 轴标题
ax.text(-1, y.sum(), '频\n次', fontsize=16, va='top', color=c['蓝色'])

# 标记线条含义
ax.text(1.5, y_cumsum[2]+10, '累计%.1f%% ' % (y_cumsum[2]/y.sum()*100), fontsize=16, color=c['橙色'], va='bottom', ha='right', zorder=5)

plt.show()
```

你可以前往 https://github.com/linjiwx/mp 下载画图用的数据和完整代码。

## 03

通过广泛寻找问题的原因，会发现影响因素有很多，但是各种因素对问题的影响程度并不相同，因此需要缩小范围，找出导致问题的主要原因。

**要识别问题的主要原因，可以借助帕累托图，对各种原因进行优先级排序，多问几个「为什么」，逐级分析，以确定根本原因。**

1951 年，管理学家戴克将帕累托图应用于库存管理，命名为 ABC 分析法。

1963 年，德鲁克将 ABC 分析法进一步推广，使其成为企业提高效益的管理方法。

对于个人来讲，我把自己的工作清单分成 3 大类：

A 类：需要投入巨大精力的长期工作。

B 类：需要及时响应并完成的工作。

C 类：需要快速跟进处理的工作。

坚持「要事优先」的原则，每天分配时间给重要的事情，我认为这也算是「二八法则」的一种实际应用。

