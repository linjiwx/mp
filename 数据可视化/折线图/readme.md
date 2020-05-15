#  改进折线图的 12 个细节

## 01

你好，我是林骥。

以前我用 Python 中的 matplotlib 画过这样一张[折线图](https://mp.weixin.qq.com/s/007mJKVqAIE1PK1NNFghuw)：

![ ](https://tva1.sinaimg.cn/large/007S8ZIlgy1gec68iim3wj30h10fiwfw.jpg)

从设计的角度来看，这张图显得有些杂乱。

为了降低观察者的认知负担，我们可以对它做些改进，比如改成下面这样：

![ ](https://tva1.sinaimg.cn/large/007S8ZIlgy1ged8wr70e6j314i0n275r.jpg)

其中改进的细节包括：

1、把标题变成**左对齐**，更加符合设计的审美；

2、把标题颜色换成深灰色，让观察者更加关注数据；

3、删除不必要的边框和网格线，避免它们消耗观察者的精力；

4、只保留最大值和最小值的标记，让对比更加明显；

5、**去掉图例**，直接在线条的附近标注，避免观察者在图例和数据之间来回移动；

6、去掉平均值线，让图表显得更加简洁；

7、更加谨慎而且有策略地使用颜色，去掉那些花花绿绿的颜色，换成只有蓝色和灰色，这样反而能够让重要的信息显得更加突出；

8、坐标轴和标签文字统一换成深灰色，让它们更自然地融入背景，在视觉上不与数据进行竞争；

9、把竖直的日期标签，换成横向的简化日期格式，以便提高阅读的体验，有研究表明，阅读 90 度角倾斜的文字，速度比阅读正常方向的文字平均慢 205%；

10、去掉最大值和最小值的具体数字，因为这里更加关心的是数据背后的事件，而不是数字本身；

11、增加 X 轴的标题「日期」，让它与最左侧的标签对齐；

12、增加 Y 轴的标题「产品销量」，让它与最上方的标签对齐，为了更加方便阅读，采用换行的方法，把 Y 轴的标题文字变成竖直的方向。



## 02

下面是画图的详细代码。

首先，导入所需的库，并设置中文字体和定义颜色等。

```python
# 导入所需的库
import numpy as np
import pandas as pd
import matplotlib as mpl
import matplotlib.pyplot as plt
from datetime import timedelta

# 正常显示中文标签
mpl.rcParams['font.sans-serif'] = ['SimHei']

# 自动适应布局
mpl.rcParams.update({'figure.autolayout': True})

# 正常显示负号
mpl.rcParams['axes.unicode_minus'] = False

# 定义颜色，主色：蓝色，辅助色：灰色，互补色：橙色
colors = {'蓝色':'#00589F', '深蓝色':'#003867', '浅蓝色':'#5D9BCF',
          '灰色':'#999999', '深灰色':'#666666', '浅灰色':'#CCCCCC',
          '橙色':'#F68F00', '深橙色':'#A05D00', '浅橙色':'#FBC171'}
```

其次，从 Excel 文件中读取随机模拟的数据，并定义画图用的数据。

```python
# 数据源路径
filepath='./data/2019年9月每日销售.xlsx'

# 读取 Excel文件
df = pd.read_excel(filepath)

# 定义画图用的数据
x = [x.strftime('%m-%d') for x in df.日期]
y = df.实际销量
```

接下来，开始用「面向对象」的方法进行画图。

```python
# 使用「面向对象」的方法画图，定义图片的大小
fig, ax=plt.subplots(figsize=(10, 6))

# 设置标题
ax.text(-3.9, y.max()+6, '\n2019年9月每天产品销量的变化趋势\n', fontsize=26, color=colors['深灰色'])

# 绘制折线图
ax.plot(x, y, marker='o', color=colors['蓝色'], markevery=[y.idxmin(), y.idxmax()], mfc='w')

# 标注最大值对应的事件
ax.annotate('打折促销', xy=(x[y.idxmax()], y.max()), color=colors['蓝色'],
             xytext=(y.idxmax()-4.2, y.max()), 
             arrowprops=dict(arrowstyle='->', color=colors['蓝色']), fontsize=16)

# 标注最小值对应的事件
ax.annotate('中秋节', xy=(x[y.idxmin()], y.min()), color=colors['深灰色'],
             xytext=(y.idxmin()-3.6, y.min()), 
             arrowprops=dict(arrowstyle='->', color=colors['深灰色']), fontsize=16)

# 计算 7 天移动平均
y2 = y.rolling(7).mean()
# 绘制趋势线
ax.plot(x, y2, ls='-', color=colors['灰色'], label='七天移动平均')
# 绘制趋势线末端的箭头
plt.annotate('', xy=(x[-1:], y2[-1:]), xytext=(x[-2:-1], y2[-2:-1]), 
             arrowprops=dict(arrowstyle='->', color=colors['灰色'], shrinkB=0))
# 文字说明移动平均
ax.text(len(x)-7.8, y2[-8:-7], '七天移动平均', color=colors['深灰色'], fontsize=12, bbox=dict(facecolor='w', edgecolor='w'))

# 隐藏边框
ax.spines['top'].set_visible(False)
ax.spines['right'].set_visible(False)
ax.spines['bottom'].set_color(colors['灰色'])
ax.spines['left'].set_color(colors['灰色'])

# 隐藏 X 轴的刻度线
ax.tick_params(axis='x', which='major', length=0)

# 设置 X 轴显示的标签
xt = []
for i in np.arange(len(x)):
    if i % 5 == 0:
        xt.append(x[i])
    else:
        xt.append('')
xt[-1] = x[-1]
ax.set_xticklabels(xt, ha='left')

# 设置 X、Y 轴的标题，适当留白
ax.text(-0.1, -5, '日期', ha='left', fontsize=16, color=colors['深灰色'])
ax.text(-3.9, y.max()+6.8, '产\n品\n销\n量', va='top', fontsize=16, color=colors['深灰色'])

# 设置坐标标签字体大小和颜色
ax.tick_params(labelsize=16, colors=colors['深灰色'])

# 设置 y 轴的刻度范围
ax.set_ylim(0, y.max()+6)

plt.show()
```

运行之后，便得到上面那张改进之后的折线图。

你可以前往 https://github.com/linjiwx/mp 下载画图用的数据和完整代码，其中包含详细的注释。



## 03

一幅好看又好用的数据可视化作品，应考虑用户的需求，把用户放在第一位，**让形式服从功能**。在突出重要内容的同时，还需要尽可能消除干扰，保持图表的简单易懂，去掉不必要的复杂元素。

有时候，在图表中适当地添加文字内容，有助于用户更加快速地理解信息，所以，不要惧怕在图表中使用文字。

多花些时间改进图表的细节，避免设计过度复杂的图表，永远不要为了添加而添加，应该认真思考，带着明确的目标，设计图表的时候多做减法，体现设计者的用心和对用户的尊重，这会耗费设计者的一些时间，但这么做是值得的，因为能让用户对图表有更多的耐心，进而能够增加成功传达信息的概率。



## 作者简介

我是林骥，从 2008 年开始从事「数据化分析」，坚持用心做原创，期待你的关注。



![ ](https://tva1.sinaimg.cn/large/007S8ZIlgy1ged8i5z35lj30pv0b0gnh.jpg)



## 推荐阅读

[数据可视化 | 用柱形图进行对比分析](https://mp.weixin.qq.com/s/G78MNwpJAPt9V4KZLwjg5w)

[数据可视化 | 用子弹图进行对比分析](https://mp.weixin.qq.com/s/UGugJfL-cAa3ryP15VrmjQ)

