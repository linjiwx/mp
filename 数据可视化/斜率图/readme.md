#  用斜率图进行对比分析

## 01

你好，我是林骥。

斜率图，可以快速展现两组数据之间各维度的变化，特别适合用于对比两个时间点的数据。

比如说，为了对比分析某产品不同功能的用户满意度，经过问卷调查和数据统计，得到下面这个调查结果：

![ ](https://tva1.sinaimg.cn/large/007S8ZIlgy1gf1zmnpsmyj30j405oglz.jpg)

你不妨自己先思考一下，如何对这组数据进行可视化，才能让信息传递变得更加高效？

下面是我用 matplotlib 制作的图表：

![ ](https://tva1.sinaimg.cn/large/007S8ZIlgy1gf1zwor06zj30bt0bsaal.jpg)



从图中可以直观地看出，功能 C 的用户用户满意度明显下降，我们用比较鲜明的橙色来表示，以便引起观众重点关注；功能 D 和功能 E 的用户满意度明显提升，我们用蓝色表示，代表数据正在向好的方向发展；功能 A 和功能 B 的用户满意度变化不大，我们用浅灰色表示，以便削弱观众对这两个功能的注意力，把更多的精力用于分析用户满意度明显下降的功能点，从而让图表起到提升信息传递效率的目的。



## 02

下面是用 matplotlib 画图的详细步骤。

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

# 定义颜色，主色：蓝色，辅助色：灰色，互补色：橙色
c = {'蓝色':'#00589F', '深蓝色':'#003867', '浅蓝色':'#5D9BCF',
     '灰色':'#999999', '深灰色':'#666666', '浅灰色':'#CCCCCC',
     '橙色':'#F68F00', '深橙色':'#A05D00', '浅橙色':'#FBC171'}
```

其次，从 Excel 文件中读取随机模拟的数据，并定义画图用的数据。

```python
# 数据源路径
filepath='./data/问卷调查结果.xlsx'

# 读取 Excel文件
df = pd.read_excel(filepath, index_col='调查年度')

# 定义画图用的数据
category_names = df.columns
labels = df.index
data = df.values
data_cum = data.cumsum(axis=1)
```

接下来，开始用「面向对象」的方法进行画图。

```python
# 使用「面向对象」的方法画图，定义图片的大小
fig, ax=plt.subplots(figsize=(6, 6))

# 设置背景颜色
fig.set_facecolor('w')
ax.set_facecolor('w')

# 设置标题
ax.set_title('\n用户满意度随时间的变化\n', fontsize=26, loc='left', color=c['深灰色'])

# 定义颜色
category_colors = [c['浅灰色'], c['浅灰色'], c['橙色'], c['蓝色'], c['蓝色']]

# 画斜率图
for i, color in zip(np.arange(len(df.columns)), category_colors):
    ax.plot(df.index, df.iloc[:, i], marker='o', color=color)
    
    # 设置数据标签及其文字颜色
    ax.text(-0.03, df.iloc[0, i], df.columns[i] + ' ' + '{:.0%}'.format(df.iloc[0, i]), ha='right', va='center', color=color, fontsize=16)
    ax.text(1.06, df.iloc[1, i], '{:.0%}'.format(df.iloc[1, i]), ha='left', va='center', color=color, fontsize=16)

# 设置 Y 轴刻度范围
ax.set_ylim(df.values.min()-0.02, df.values.max()+0.01)

# 隐藏 Y 轴
ax.yaxis.set_visible(False)

# 隐藏边框
ax.spines['top'].set_visible(False)
ax.spines['right'].set_visible(False)
ax.spines['left'].set_visible(False)
ax.spines['bottom'].set_visible(False)

# 隐藏 X 轴的刻度线
ax.tick_params(axis='x', which='major', length=0)

# 设置坐标标签字体大小和颜色
ax.tick_params(labelsize=16, colors=c['灰色'])

plt.show()
```

运行之后，便得到上面那张图。

你可以前往 https://github.com/linjiwx/mp 下载画图用的数据和完整代码。



## 03

对于同一组数据，不同的人可能会有不同的观察视角，对它们进行可视化，往往也存在多种不同的解决方案，这里介绍的方法，并不是唯一正确的答案。关键在于，图表的设计者想要表达什么信息？是否让观众正确且快速地理解了想要表达的信息？

不同类型的图表，有着不同的优势和劣势。

**斜率图的优势，是能快速看到每个类别前后发生的变化，并能根据线条的陡峭程度，直观地感受到变化的幅度。**

斜率图的劣势，是看不出整体与部分的占比关系。另外，如果类别的顺序很重要，那么也不适合使用斜率图，因为类别会根据数值大小自动进行排列。

最后，留给你一道思考题：在你看到过的各种数据中，有哪些数据是适合用斜率图进行对比分析的？

当你不知道该选择什么类型的图表时，不妨停下来想一想，你希望让观众了解什么或者做什么？



##  作者简介

林骥，从 2008 年开始从事「数据化分析」，坚持用心做原创，期待你的关注。



![ ](https://tva1.sinaimg.cn/large/007S8ZIlgy1gefb1hj9krj30pv0b00u8.jpg)

