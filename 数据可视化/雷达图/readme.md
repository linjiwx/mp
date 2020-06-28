#  用雷达图进行对比分析

## 01

你好，我是林骥。

雷达图的背景一圈一圈地像雷达，用多边形来展现数据的大小，我认为比较适合用于有多种不同维度的情形，是发现差距的一种好工具。

比如说，「得到 APP」上的学分构成包括 5 个不同维度，我根据自己的学分构成及其变化，制作了一张雷达图。

![ ](https://tva1.sinaimg.cn/large/007S8ZIlgy1gfzorvkm3mj30po0swtfh.jpg)

其中「持续性」与学习的天数相关，「学习量」与听课或看书的数量相关，「笔记」与笔记的数量和互动相关，「知识分享」与分享转发的次数相关，「好奇心」与搜索的次数和广度相关。

从图中可以看出，在 2020 年的年初，我在笔记方面还比较薄弱，经过努力，我做笔记的数量明显增加了。

借助雷达图，我们可以直观地看到差距，进而通过分析，更好地进行改善。

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
filepath='./data/林骥的学分构成.xlsx'

# 读取 Excel文件
df = pd.read_excel(filepath)

# 提取画图所需的数据
data0 = df.iloc[0, 2:].values
data1 = df.iloc[1, 2:].values

#提取标签
label = np.array(df.iloc[1, 2:].index)

# 根据分数添加评级的标签
for i, d in enumerate(data1):
    if d > 4:
        grade = 'A^+'
    elif d == 4:
        grade = 'A'
    elif d > 3:
        grade = 'B^+'
    elif d == 3:
        grade = 'B'
    else:
        grade = 'B^-'
    label[i] += '\n' + r'$\bf{' + grade + '}$'
    

# data 有几个数据，就把整圆 360° 分成几份
angle = np.linspace(0, 2*np.pi, len(data0), endpoint=False)

# 增加第一个 angle 到所有 angle 里，以实现闭合
angles = np.concatenate((angle, [angle[0]]))

# 倒转顺序，以让雷达图顺时针显示
angles = angles[::-1]

#增加第一个 data 到所有的 data 里，以实现闭合
data0 = np.concatenate((data0, [data0[0]]))
data1 = np.concatenate((data1, [data1[0]]))
```

接下来，开始用「面向对象」的方法进行画图。

```python
# 使用「面向对象」的方法画图，定义图片的大小
fig, ax=plt.subplots(figsize=(8, 8), subplot_kw=dict(polar=True))

# 设置背景颜色
fig.set_facecolor('w')
ax.set_facecolor('w')

# 设置标题
ax.set_title('\n林骥的学分构成及其变化\n\n', fontsize=26, loc='left', color=c['深灰色']) 

# 设置网格标签
ax.set_thetagrids(angles*180/np.pi, labels=label)

# 画雷达图，用顺时针显示
ax.plot(angles, data0, 'o-', label=df.iloc[0, 0].strftime('%Y-%m-%d'))
ax.plot(angles, data1, 'o-', label=df.iloc[1, 0].strftime('%Y-%m-%d'))

# 设置极坐标 0° 的位置
ax.set_theta_zero_location('N') 

# 设置显示的极径范围
ax.set_rlim(0, 5)

# 填充颜色
ax.fill(angles, data0, facecolor=c['浅蓝色'], alpha=0.6)
ax.fill(angles, data1, facecolor=c['浅橙色'], alpha=0.6)

# 设置极径标签，放在第一象限的中间位置
ax.set_rlabel_position(360-360/len(data0)/2)

# 设置图例显示的位置
l = ax.legend(ncol=2, loc='lower center', frameon=False, borderaxespad=-3, fontsize=13)
for text in l.get_texts():
    text.set_color(c['深灰色'])
#     text.set_size(13)

# 去掉最外围的黑圈
ax.spines['polar'].set_visible(False) 

# 设置坐标标签字体大小和颜色
ax.tick_params(labelsize=16, colors=c['深灰色'])

plt.show()
```

你可以前往 https://github.com/linjiwx/mp 下载画图用的数据和完整代码。

## 03

雷达在展现多个维度的得分或性能方面，效果不错，在财务分析和标杆管理中有着广泛的应用。

另外，在一些游戏中，也有用雷达图来展现人物的能力。

但是，雷达图也有一些自身的缺点，包括：

（1）如果在一个雷达图中展现超过 2 组数据，会让图表难以阅读。

（2）变量的个数不宜过多，否则密密麻麻的线条可能让人抓不到重点。

（3）从表达数据的精确度来看，极坐标中的角度，不如直角坐标中的位置。

同样的数据，不同人得出的观点可能不一样，图表的选择可能也不一样，我们通常需要考虑以下几个因素：

（1）分析提炼的信息；

（2）所属数据的类型；

（3）想要表达的观点；

（4）想要强调的信息。

很多人作图有一种误区，就是喜欢运用所谓的技巧和创新，做出让人难以看懂的复杂图表，这与图表的目的背道而驰，是我们应该避免的。

