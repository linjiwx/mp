#  520，致我爱的人

## 01

你好，我是林骥。

今天是 5 月 20 日，我用 Python 做了一个视频，送给我爱的人。

（在公众号中有视频演示）

## 02

下面是我制作爱心视频的过程。

首先，导入所需的库，并设置中文字体和定义颜色等。

```python
# 导入所需的库
import numpy as np
import pandas as pd
import matplotlib as mpl
import matplotlib.pyplot as plt
from matplotlib.animation import FuncAnimation
import re
import jieba
import wordcloud
from imageio import imread

%matplotlib inline

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

其次，生成画图用的数据。

```python
# 生成心形曲线的数据
x_coords = np.linspace(-3, 3, 500)
y_coords = np.linspace(-3, 3, 500)
points = []
for y in y_coords:
    for x in x_coords:
        if(x**2+y**2-1)**3 <= x**2*y**3:
            points.append({ "x": x, "y": y})
heart_x = list(map( lambda point: point[ "x"], points))
heart_y = list(map( lambda point: point[ "y"], points))

# 设置动画图片的张数
frames = 100
# 切分列表
points = [int(x) for x in np.linspace(0, len(heart_x), frames)]
```

接下来，开始用「面向对象」的方法进行画图。

利用上面生成的数据，制作爱心动画和爱心图片。

用上面生成的数据，制作爱心动画和爱心图片。

```python
# 使用「面向对象」的方法画图
fig, ax = plt.subplots(figsize=(6, 6))

# 隐藏边框
ax.spines['top'].set_visible(False)
ax.spines['right'].set_visible(False)
ax.spines['bottom'].set_visible(False)
ax.spines['left'].set_visible(False)

# 隐藏坐标轴刻度
ax.set_xticks([])
ax.set_yticks([])

# 设置坐标轴范围
ax.set_xlim(-1.2, 1.2)
ax.set_ylim(-1.15, 1.25)

# 记录进度
txt = ax.text(0, 1.35, '', fontsize=26, color=colors['深灰色'], ha='center')

# 动画函数
def animate(i):
    # 设置起止点
    start = points[i-1]
    end = points[i]
    
    # 画散点图
    ax.scatter(heart_x[start:end], heart_y[start:end], s=5, c='r')
    
    # 文字动态变化
    txt.set_text(str(i+1))

# 用函数的方式绘制动画，interval 表示间隔毫秒数
anim = FuncAnimation(fig, animate, frames=frames, interval=100)

# 保存为 mp4 的文件格式
anim.save('爱心.mp4')

# 保存图片
plt.savefig('爱心.png')
```

然后，利用爱心图片，加上一封情书，做成一张词云图。

```python
# 词云图的文本
text = '''
时光仿佛一杯静水，依然深刻，依然可以深流，而一份心情，却与风月无关，水逝惊鸿去。

站在时光的路口，回望曾经走过的美丽和温柔。

许多人，许多事，许多曾经花发枝满的渴求与憧憬，依然在岁月的长河中缓缓流过，又默默回溯。

世事纷繁，时光终是无言，所谓的执念也许只是虚妄，所谓的抵达也不过是终点。

生命不止，红尘无尽。仅以一程换一种懂得，仅以一程换一场经历，如此，而已。

谁又是谁生命中的看客和过客？推开一扇叫岁月的门，许多年华，终于被渐次搁浅。

而你，永远是斜格子里的光影，游走在梦与现实的边缘。

若是时光锁住的葱茏，曳动冷冷的素月清秋，那么，谁取你一瓢，醉饮红尘外？

此生，若你安好，便是晴天！
'''

# 分词
word_list = jieba.cut(text, cut_all=True)
word = ' '.join(word_list)

# 读取图片
pic = imread('爱心.png')

# 绘制词云图
wc = wordcloud.WordCloud(mask=pic, font_path='simhei.ttf', background_color='white').generate(word)

# 隐藏坐标轴
plt.axis('off')

# 生成词云图
plt.imshow(wc)

# 保存词云图
wc.to_file('词云图.png')

# 显示图片
plt.show()
```

![ ](https://tva1.sinaimg.cn/large/007S8ZIlgy1gexfxvka3lj30c00c00uq.jpg)

最后，把视频和图片导入视频剪辑软件，配上音乐，一段表达爱意的视频就做好了，你学会了吗？

## 03

谨以此文，送给我爱的人。

把冰冷的技术，与温暖的情感相融合，是我努力的一个方向。

作为数据分析师，我认为也应该以理性和感性的视角，去理解业务，洞察人性。



##  作者简介

林骥，从 2008 年开始从事「数据化分析」，坚持用心做原创，期待你的关注。



![ ](https://tva1.sinaimg.cn/large/007S8ZIlgy1gefb1hj9krj30pv0b00u8.jpg)

