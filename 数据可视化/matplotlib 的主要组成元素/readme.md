#  matplotlib 的主要组成元素

![ ](https://mmbiz.qpic.cn/mmbiz_png/giaycic3UNwo1d0OsmJPa1qlBa9pxzZSLQ09bwiaAJLkU4C0KoI5AOBcfo91MFkTtIbJuT9OsNw7sj0IFbDXYDLQA/0?wx_fmt=png)

## 01

你好，我是林骥。

matplotlib 是 Python 中的一个绘图库，具有可定制性强、图表资源丰富等特点，既能创建静态图形，也能创建动态交互的数据可视化效果；既能创建 2D 图形，也能创建 3D 图形；既能创建常见的简单图形，也能创建统计学专业的复杂图形。matplotlib 绘制出来的图形质量很高，可以达到出版级别，而且功能非常强大，几乎能创建任何你能想象的可视化图形。

在 Python 中，还有很多其他非常优秀的绘图库，比如 Seaborn、Pyecharts、Bokeh、Plotly、ggplot 等等，它们各有各的优势，但是我个人认为，应该先把 matplotlib 学精学透，达到深入理解和熟练运用的程度。因为 matplotlib 属于比较底层的绘图库，熟练掌握 matplotlib 之后，再去学习其他绘图库的时候，通常也能够触类旁通。

使用 matplotlib 进行数据可视化，就好像是自己点餐，可能相对会比较麻烦一点，但最后上桌的菜，全是适合自己口味的；而有些绘图库就像是餐厅里面的套餐，可能相对比较简单，但套餐里往往会有些菜不是那么合自己口味的。

## 02

下面这张图是用 matplotlib 画出来的，你可以前往 <https://github.com/linjiwx/mp> 获取完整代码。

![ ](https://tva1.sinaimg.cn/large/00831rSTgy1gdnp8h3us7j30fs0fudip.jpg)

从图中可以看出，matplotlib 的主要组成元素包括：

（1） figure 属于最底层的画布，其中可以包含多个 axes；

（2） axes 是画布上的图形区域，是 axis 的复数，表示其中可以包含多个 axis；

（3） axis 是图形区域中的坐标轴，其中上下左右可以包含 tick；

（4） tick 是坐标轴中的标记，包含坐标轴的标记线和标签文本；

（5） title 是图表的标题，通常起到画龙点睛的作用；

（6） label 是图表中的标签，例如 xlable 代表 X 轴的标签；

（7） legend 是说明符号的图例；

（8） grid 是图表中的网格线；

（9） span 是图表中突出显示的区域；

（10） spine 是图表中的边界线；

（11） text 是图表中自定义的文本；

（12） annotate 是图表中注解信息的箭头；

（13） line 是图表中的线条；

（14） plot 主要用于绘制折线图；

（15） scatter 主要用于绘制散点图。

还有一些其他的元素，以后我们再结合示例进行介绍。

## 03

尽管使用类似于 plt.plot() 的函数，可以简化 matplotlib 画图的代码，但是「面向对象」的方法更加灵活、更加强大，林骥推荐坚持使用后者，也就是通过调用 figure 和 axes 的方式，因为个人感觉这样更有掌控感。建议尽量不要同时使用两种不同的方法，以免造成混乱。

数据可视化的目标，是更加有效地传递信息，从而更好地解决问题。可视化的技术在不断地进步，但是人性却不会发生什么大的变化。通过数据可视化和解决问题的分析思维，去理解和洞察数据背后的人性，最终为人服务，这是数据分析的底层心法。

我们可以把自己交付的工作成果，当成自己亲手设计打造的产品，将数据分析、心理学、产品设计和营销结合起来，不断进行优化迭代，以用户为中心，在艺术性与易用性、可靠性与安全性、成本与功能、时间与效果之间，寻求平衡与和谐，为用户提供一种更好的服务体验。

通过自我修炼，不断精进，帮助他人，实现共赢，这是一条充满阳光的修行之路。

> 关于作者：林骥，从 2008 年开始从事数据分析工作，坚持用心做原创，用数据化解难题，让分析更加有效，@数据化分析。

![ ](https://mmbiz.qpic.cn/mmbiz_png/giaycic3UNwo1coPZTxWDIUPwI0DujmBqXwKwCsDnRQT8XzrfXICgcyJPz0icxf4DjUB1Gy8cSn23ia07PfFYy2vIw/0?wx_fmt=png)

### 推荐阅读

[用 Python 画梦想矩阵](https://mp.weixin.qq.com/s/zVv3hs7inrVTDRwGcc-InA)

[Python编程实践（2）：数据可视化](https://mp.weixin.qq.com/s/dmJGMvFAroxJkTk5NwynSw)

[收藏 | 数据可视化的方法、工具和应用](https://mp.weixin.qq.com/s/DZIkVgvh7YOuOwLALb15vw)
