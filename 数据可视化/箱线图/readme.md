#  用箱线图进行对比分析

## 01

你好，我是林骥。

箱线图，形状有点像箱子连着两条须线，主要用于反映数据的分布特征，可以看出数据的对称性和分散度等信息，适合用于对比分析多组数据的分布情况。

比如说，调查用户对某产品不同功能的满意度，得到的数据如下表：

![ ](https://tva1.sinaimg.cn/large/007S8ZIlgy1gficrtx3wdj30j30dwdhh.jpg)

我们可以用箱线图来进行对比分析，效果图如下：

![ ](https://tva1.sinaimg.cn/large/007S8ZIlgy1gfif37p3t7j30g00c0mxr.jpg)

在上面的图中，箱体的颜色，代表用户满意度的高低，橙色代表满意度较低，灰色代表满意度一般，蓝色代表满意度较高。

箱体中的橙色线条，代表中位数，它一般不会受到极大或极小值的影响，所以提高了对数据分布的代表性。

箱体中的灰色虚线，代表平均值，是一个常用的统计量，它的缺点是容易受极端数据的影响。

比如说，把 10 个普通人和 1 个世界首富，放在一起计算平均收入，如果用这个平均收入，去说明大家的收入很高，那么结果就很难让人信服。

箱体两头的须线，代表数据的离散程度，须线越长，代表数据越分散，须线越短，代表数据越集中。



## 02

接下来，我们看看用 matplotlib 画图的具体步骤。

首先，导入所需的库，并设置中文字体和定义颜色等。

```python
# 导入所需的库
import numpy as np
import pandas as pd
import matplotlib as mpl
import matplotlib.pyplot as plt

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
filepath='./data/问卷调查结果3.xlsx'

# 读取 Excel文件
df = pd.read_excel(filepath, index_col='调查用户')

# 定义画图用的数据
labels = df.columns
data = df.values
```

接下来，开始用「面向对象」的方法进行画图。

```python
# 使用「面向对象」的方法画图，定义图片的大小
fig, ax = plt.subplots(figsize=(8, 6))

# 设置背景颜色
fig.set_facecolor('w')
ax.set_facecolor('w')

# 设置箱体的属性
boxprops = {'color':c['灰色']}
# 设置中位线参数
medianprops = {'color':c['橙色']}
# 设置均值的属性
meanprops = {'color':c['灰色']}
# 设置箱线图顶端和末端线条的属性
capprops = {'color':c['灰色']}
# 设置须线的属性
whiskerprops = {'color':c['灰色']}

# 绘制箱线图，whis 用来控制箱须包含数据的范围，这里设置得比较大，是为了不让图中出现异常点
bplot = ax.boxplot(data, vert=False, patch_artist=True, labels=labels, showmeans=True, whis=5, meanline=True, 
                   boxprops=boxprops, medianprops=medianprops, meanprops=meanprops, capprops=capprops, whiskerprops=whiskerprops)

# 设置标题
ax.set_title('\n用户满意度的分布情况\n', loc='left', size=26, color=c['深灰色'])

# 倒转 Y 轴，让第一个功能排在最上面
ax.invert_yaxis()

# 隐藏 Y 轴的刻度线
ax.tick_params(axis='y', which='major', length=0)

# 设置 X 轴范围
ax.set_xlim(0.76, 1.02)

# 隐藏 X 轴
ax.xaxis.set_visible(False)

# 标记箱线代表的含义
ax.text(min(data[:,0]), 1, '%.1f%% '%(min(data[:,0])*100)+'\n最小值 ', ha='right', fontsize=12, color=c['深灰色'], va='center')
ax.text(np.mean(data[:,0]), 0.7, ' %.1f%%'%(np.mean(data[:,0])*100)+'\n 平均值', ha='center', fontsize=12, color=c['深灰色'], va='bottom')
ax.text(np.median(data[:,0]), 1.3, ' %.1f%%'%(np.median(data[:,0])*100)+'\n中位数', ha='center', fontsize=12, color=c['橙色'], va='top')
ax.text(max(data[:,0]), 1, ' %.1f%%'%(max(data[:,0])*100)+'\n 最大值', ha='left', fontsize=12, color=c['深灰色'], va='center')

# 循环添加数据标签
for i in range(1, len(labels)):
    ax.text(min(data[:,i]), i+1, '%.1f%% '%(min(data[:,i])*100), ha='right', fontsize=12, color=c['深灰色'], va='center')
    ax.text(np.mean(data[:,i]), i+0.7, ' %.1f%%'%(np.mean(data[:,i])*100), ha='center', fontsize=12, color=c['深灰色'], va='bottom')
    ax.text(np.median(data[:,i]), i+1.3, ' %.1f%%'%(np.median(data[:,i])*100), ha='center', fontsize=12, color=c['橙色'], va='top')
    ax.text(max(data[:,i]), i+1, ' %.1f%%'%(max(data[:,i])*100), ha='left', fontsize=12, color=c['深灰色'], va='center')

# 填充箱体的颜色
colors = [c['浅橙色'], c['浅橙色'], c['浅灰色'], c['蓝色'], c['蓝色']]
for patch, color in zip(bplot['boxes'], colors):
    patch.set_facecolor(color)

# 隐藏边框
ax.spines['top'].set_visible(False)
ax.spines['right'].set_visible(False)
ax.spines['bottom'].set_visible(False)
ax.spines['left'].set_visible(False)

# 设置坐标标签字体大小和颜色
ax.tick_params(labelsize=16, colors=c['深灰色'])

# 用百分比格式显示坐标轴
def to_percent(temp, position=1):
    return '%.0f'%(100 * temp) + '%'
formatter = mpl.ticker.FuncFormatter(to_percent)
ax.xaxis.set_major_formatter(formatter)

plt.savefig('用户满意度的分布情况.jpeg')

plt.show()
```

你可以前往 https://github.com/linjiwx/mp 下载画图用的数据和完整代码。

下面是用 matplotlib 画箱线图 boxplot 的主要参数说明，以备查询参考：

x：指定要绘制箱线图的数据；

notch：是否是凹口的形式展现箱线图，默认非凹口；

sym：指定异常点的形状，默认为 + 号显示；

vert：是否需要将箱线图垂直摆放，默认垂直摆放；

whis：指定上下须与上下四分位的距离，默认为 1.5 倍的四分位差；

positions：指定箱线图的位置，默认为 [0, 1, 2 …]；

widths：指定箱线图的宽度，默认为 0.5；

patch_artist：是否填充箱体的颜色；

meanline：是否用线的形式表示均值，默认用点来表示；

showmeans：是否显示均值，默认不显示；

showcaps：是否显示箱线图顶端和末端的两条线，默认显示；

showbox：是否显示箱线图的箱体，默认显示；

showfliers：是否显示异常值，默认显示；

boxprops：设置箱体的属性，如边框色，填充色等；

labels：为箱线图添加标签，类似于图例的作用；

filerprops：设置异常值的属性，如异常点的形状、大小、填充色等；

medianprops：设置中位数的属性，如线的类型、粗细等；

meanprops：设置均值的属性，如点的大小、颜色等；

capprops：设置箱线图顶端和末端线条的属性，如颜色、粗细等；

whiskerprops：设置须线的属性，如颜色、粗细、线的类型等。



## 03

在一个箱线图中，通常包含以下 5 个统计量：

1、第一个四分位数（Q1）；

2、中位数（Median）；

3、第三个四分位数（Q3）；

4、「最小值」（Q1 - 1.5 * IQR）；

5、「最大值」（Q3 + 1.5 * IQR）。

其中  IQR 代表四分位间距，等于 Q3 - Q1，即从 Q1 到 Q3 的距离，1.5 是 matplotlib 中 boxplot 函数 whis 参数的默认值。

需要注意的是，箱线图中的「最小值」未必是真的最小值，因为箱线图可能会把离群值排除在外。

比如下面这张图，绿色圆圈代表离群值。

![ ](https://tva1.sinaimg.cn/large/007S8ZIlgy1gfid4gbhgej30ua0u0tl1.jpg)

上面把箱线图与正态分布图进行对比，应该能帮你更好地理解箱线图中各统计量代表的含义。



