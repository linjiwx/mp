#  用气泡图进行对比分析

## 01

你好，我是林骥。

气泡图是在散点图的基础上，用气泡的大小来展示第三个变量。在一个气泡图中，能够同时对三个变量进行对比分析。

比如说，对比我写的 6 篇文章，截止到 2020 年 5 月 5 日，它们的阅读量、点赞率和分享率数据如下：

![ ](https://tva1.sinaimg.cn/large/007S8ZIlgy1gehemezxo0j30ls0ce0uo.jpg)

我用 matplotlib 画了一张气泡图：

![ ](https://tva1.sinaimg.cn/large/007S8ZIlgy1gehkyqo6f0j30wl0u0aeu.jpg)

从图中可以直观地看出，点赞率最高的文章是「[用子弹图进行对比分析](https://mp.weixin.qq.com/s/UGugJfL-cAa3ryP15VrmjQ)」；分享率最高的文章是「[如何用数据解决实际问题](https://mp.weixin.qq.com/s/yKOSjyKTDP-Q0nTkxVprJQ)」，其阅读量也是这 6 篇文章中最多的。

影响文章阅读量的因素有很多，首先是粉丝的数量，其次是被点赞（也就是文章右下角的「在看」）和分享的人数，最好是能被有影响力的人分享。另外，文章标题的吸引力，文章内容的质量，文章发送的时间等等，都有可能对阅读量产生比较大的影响。

对我来说，我比较看重的是「点赞率」这个指标，因为它代表读者对文章或作者的一种认可。我希望自己写的文章，能够对读者产生价值，所以我讨厌那些没有价值的「标题党」，看完之后感觉「上当受骗」，被「骗」的次数多了以后，也就不再信任了。

谨慎地选择标题，也是对读者的一种尊重，当作者花费很多时间和精力写出的好内容，如果因为标题没起好，让潜在的读者错过了好内容，那么也是挺可惜的一件事。



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
colors = {'蓝色':'#00589F', '深蓝色':'#003867', '浅蓝色':'#5D9BCF',
          '灰色':'#999999', '深灰色':'#666666', '浅灰色':'#CCCCCC',
          '橙色':'#F68F00', '深橙色':'#A05D00', '浅橙色':'#FBC171'}
```

其次，从 Excel 文件中读取数据，并定义画图用的数据。

```python
# 数据源路径
filepath='./data/气泡图数据源.xlsx'

# 读取 Excel文件
df = pd.read_excel(filepath)

# 定义画图用的数据
x = df.点赞率
y = df.分享率
s = df.阅读量
labels = df.文章
```

接下来，开始用「面向对象」的方法进行画图。

```python
# 使用「面向对象」的方法画图，定义图片的大小
fig, ax = plt.subplots(figsize=(8, 8))

# 设置标题
ax.text(-0.0035, y.max()+0.015, '\n文章的阅读量、点赞率与分享率\n', fontsize=26, color=colors['深灰色'])

# 定义气泡的基础颜色
color = [colors['灰色']]*6
# 突出显示点赞率最高的气泡
color[x.idxmax()] = colors['蓝色']
# 突出显示分享率最高的气泡
color[y.idxmax()] = colors['橙色']

# 画气泡图
ax.scatter(x, y, s, c=color)

# 设置刻度范围
ax.set_xlim(0, x.max()+0.005)
ax.set_ylim(0.01, y.max()+0.01)

# 用百分比格式显示坐标轴
def to_percent(temp, position=1):
    return '%.1f'%(100 * temp) + '%'
formatter = mpl.ticker.FuncFormatter(to_percent)
ax.yaxis.set_major_formatter(formatter)
ax.xaxis.set_major_formatter(formatter)

# 设置 X、Y 轴的标题，适当留白
ax.text(-0.001, 0, '点赞率', ha='left', fontsize=16, color=colors['深灰色'])
ax.text(-0.0035, y.max()+0.01, '分\n享\n率', va='top', fontsize=16, color=colors['深灰色'])

# 平均值线
ax.hlines(y.mean(), 0, 1, color=colors['浅灰色'], lw=1)
ax.vlines(x.mean(), 0, 1, color=colors['浅灰色'], lw=1)

# 显示标签
for i, a, b, c, d in zip(np.arange(len(x)), x, y, labels, s):
    ax.text(a-0.001, b, c+'('+str(d)+')', ha='right', va='center', fontsize=15, color=color[i])

# 设置边框颜色
ax.spines['top'].set_color(colors['灰色'])
ax.spines['bottom'].set_color(colors['灰色'])
ax.spines['left'].set_color(colors['灰色'])
ax.spines['right'].set_color(colors['灰色'])

# 隐藏刻度线
ax.tick_params(axis='x', which='major', length=0)
ax.tick_params(axis='y', which='major', length=0)

# 设置坐标标签字体大小和颜色
ax.tick_params(labelsize=16, colors=colors['深灰色'])

plt.show()
```

运行之后，便得到上面那张图。

你可以前往 https://github.com/linjiwx/mp 下载画图用的数据和完整代码。



## 03

气泡图可以展现多个维度的数据，包括 X 轴、Y 轴、气泡大小、气泡颜色等，还能把气泡做成随着时间动态变化的效果，也就是「动态气泡图」。

统计学家 Hans Rosling 曾经利用动态气泡图，展现了 200 个国家 200 年的兴衰历史数据，**将数据可视化与现实关联起来，让枯燥的数据变得更有意义**，令人敬佩。

最后，用一段 Hans Rosling 的视频，向他致敬。





##  作者简介

林骥，从 2008 年开始从事「数据化分析」，坚持用心做原创，期待你的关注。



![ ](https://tva1.sinaimg.cn/large/007S8ZIlgy1gefb1hj9krj30pv0b00u8.jpg)

