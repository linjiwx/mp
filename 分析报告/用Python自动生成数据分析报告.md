你好，我是林骥。

很多时候，我们需要做一些重复性的工作，比如说，每个月制作类似的数据分析报告，整个框架是基本固定的，此时，我们可以采用 Python 来自动生成数据分析报告，**把更多的时间和精力用在分析上面**，而不是调整报告的格式。

python-pptx 是一个能够自动创建和更新 PPT 文件的 Python 库，可以用来自动生成数据分析报告。

下面，我以自己的个人数据为例，用 python-pptx 制作一个简略版的数据分析报告，供你参考。

![ ](https://tva1.sinaimg.cn/large/007S8ZIlgy1gjfdhacltjj313u0medgw.jpg)

![ ](https://tva1.sinaimg.cn/large/007S8ZIlgy1gjfdj8rw4fj313u0memzd.jpg)

![ ](https://tva1.sinaimg.cn/large/007S8ZIlgy1gjfelc0g9cj313u0meq65.jpg)

![ ](https://tva1.sinaimg.cn/large/007S8ZIlgy1gjfdkkuu8dj313u0metd3.jpg)

![ ](https://tva1.sinaimg.cn/large/007S8ZIlgy1gjh5l3g92kj313u0me762.jpg)

![ ](https://tva1.sinaimg.cn/large/007S8ZIlgy1gjfdlws7k0j313u0medic.jpg)





下面是具体制作的步骤和方法。

首先，如果你还没有安装 python-pptx，那么请在命令行输入：

```python
pip install python-pptx
```

其次，利用 matplotlib 等绘图工具，生成数据分析报告中用到的图表，统一保存到 pic 文件夹中。

然后，建立一个 PPT 模板文件，预先定义好母版，设置相应的布局版式等，把文件命名为「模板.pptx」。

接下来，在 Jupyter Lab 环境中运行以下代码：

```python
# 导入库
from pptx import Presentation
from pptx.util import Inches, Pt
from pptx.dml.color import RGBColor

# 模板下载 https://github.com/linjiwx/mp
prs = Presentation('模板.pptx')

# 添加幻灯片首页
slide_layout0 = prs.slide_layouts[0]
slide = prs.slides.add_slide(slide_layout0)

# 设置标题和副标题文本
title = slide.shapes.title
subtitle = slide.placeholders[10]
title.text = '2020年9月林骥的数据分析报告'
subtitle.text = '2020-10-08'

# 添加幻灯片，正文模块，根据实际需求选择布局版式

# *************1. 主要分析结论*****************
slide_layout1 = prs.slide_layouts[1]
slide1 = prs.slides.add_slide(slide_layout1)

# 添加标题
title = slide1.placeholders[10]
title.text = "1. 主要分析结论"

# 添加正文内容
content = slide1.placeholders[11]
ft = content.text_frame
ft.clear()
p = ft.paragraphs[0]
run = p.add_run()
run.text = '（1) 年初制定的运动目标是平均每天走'

# 重点强调的内容
run = p.add_run()
run.text = '10000步'
font = run.font
font.name = 'Arial'
font.size = Pt(26)
font.color.rgb = RGBColor(0, 88, 159)

# 继续添加其他内容
run = p.add_run()
run.text = '，9月份的目标完成率为'

# 重点强调的内容
run = p.add_run()
run.text = '108.8%'
font = run.font
font.name = 'Arial'
font.size = Pt(26)
font.color.rgb = RGBColor(0, 88, 159)

# 继续添加其他内容
run = p.add_run()
run.text = '''，超额完成任务目标；

（2) 学习的各项指标均有所提升，其中笔记方面的提升最为明显，9月底的笔记评级变成'''

# 重点强调的内容
run = p.add_run()
run.text = 'A+'
font = run.font
font.name = 'Arial'
font.size = Pt(26)
font.color.rgb = RGBColor(0, 88, 159)

# 继续添加其他内容
run = p.add_run()
run.text = '。'

# ***************2. 目标完成情况******************
# 添加幻灯片
slide_layout2 = prs.slide_layouts[3]
slide2 = prs.slides.add_slide(slide_layout2)
# 添加正文模块标题
title= slide2.placeholders[10]
title.text = "2. 目标完成情况"
# 插入图片 https://github.com/linjiwx/mp
img_path='./pic/2. 目标完成情况.jpg'
picture_placeholder = slide2.placeholders[11]
placeholder_picture = picture_placeholder.insert_picture(img_path)
# 添加描述内容
content= slide2.placeholders[12]
content.text = ' '

# ***************3. 关键指标变化******************
# 添加幻灯片
slide_layout3 = prs.slide_layouts[6]
slide3 = prs.slides.add_slide(slide_layout3)
# 添加正文模块标题
title= slide3.placeholders[10]
title.text = "3. 关键指标变化"
# 插入图片对象，主图
img_path='./pic/3. 关键指标变化.jpg'
picture_placeholder = slide3.placeholders[11]
placeholder_picture = picture_placeholder.insert_picture(img_path)
# 添加描述内容
content= slide3.placeholders[12]
content.text = '''与年初相比，
各项指标均有所提升，
其中笔记的提升最多，
9月底的笔记评级变成A+。
'''

# ***************4. 变化原因分析******************
# 添加幻灯片
slide_layout4 = prs.slide_layouts[1]
slide4 = prs.slides.add_slide(slide_layout4)

# 添加正文模块标题
title= slide4.placeholders[10]
title.text = "4. 变化原因分析"

# 添加描述内容
content= slide4.placeholders[11]
content.text = '''
（1) 为了错开上班早高峰的时间，我早上通常在7点钟之前就到了公司，增加了很多学习和写读书笔记的时间；

（2) 在OKR方法的指引下，我年初制定了精细阅读26本书和原创写作60篇文章的目标，用输出倒逼输入。
'''

# *************5. 建议改善措施*****************
slide_layout5 = prs.slide_layouts[1]
slide5 = prs.slides.add_slide(slide_layout5)

# 添加正文模块标题
title= slide5.placeholders[10]
title.text = "5. 建议改善措施"

# 添加内容
content= slide5.placeholders[11]
content.text = '''
（1) 建议继续坚持运动和学习，提升自己的健康水平和能力水平，以饱满的状态投入工作，不断提高工作效率，创造出远大于回报的价值；

（2) 建议加强知识分享，教会别人，比自己动手操作要难得多，但是，分享的过程会让自己收获更多，这是一件值得投入的事。
'''

# ***************6. 封底******************
# 添加幻灯片
slide_layout2 = prs.slide_layouts[3]
slide2 = prs.slides.add_slide(slide_layout2)
# 添加正文模块标题
title= slide2.placeholders[10]
title.text = '6. 感谢您的关注'
# 插入图片对象，主图
img_path='./pic/林骥.png'
picture_placeholder = slide2.placeholders[11]
placeholder_picture = picture_placeholder.insert_picture(img_path)
# 添加描述内容
content= slide2.placeholders[12]
content.text = '用数据化解难题，让分析更加有效。'

prs.save('2020年9月林骥的数据分析报告.pptx')

print("报告已生成，请打开PPT文件查看。")
```

打开自动生成的 PPT 文件，就可以看到完整的数据分析报告结果。

长按下方的二维码，关注林骥的公众号，更多干货早知道。

![ ](https://mmbiz.qpic.cn/mmbiz_png/giaycic3UNwo0IvXVY910XS9h5qCC6kuVt2ZPOUWUib2SrDxeYP8iawPXDOIDzPb0dUgtXtOj30gB0QqnxAM6iaEehw/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

