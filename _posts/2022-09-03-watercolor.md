---
title: 学习水彩画一年的诚实记录
layout: post
subtitle: null
date: '2022-09-03 20:30:00'
author: Lanzhou
header-img: img/post-bg-unix-linux.jpg
tags:
- study
- 中文
- 碎碎念
---

- 尺寸：水彩画尺寸越大，画起来越痛苦。
- 纸：风景水彩画最好选择合适的纸，比如300g/m2 宝虹水彩纸、阿诗水彩纸.
- 笔：我好爱红胖子！勾线笔是必须的；毛笔可以用来画树的纹理，总是想再多买一只。
- 颜料：选择合适的颜料也很重要，我喜欢盒装水彩，比如马蒂尼36色。新手可以多买一些颜色。（一定要做色卡，绘画时对照色卡找颜色非常方便）对于新手来说调色会比较花时间，往往调好颜色水彩纸就已经干透了，再上色就无法与背景颜色融合。
- 胶带：普通和纸胶带就可以，淘宝上的水彩胶带也很好用。
- 调色盘，白色的，好清洁的，调色面积大一些的就可以。
- 水罐：需要一个稍微大一些的水罐，瓶盖那么大的不可以。
- 留白液：可以有，但用得很少。
- 喷壶：必须有，用得很多，新手必备。
- 控水：学了很久才明白控水，早点掌握早点摆脱水迹的烦恼。
- 素描：不学是不行的。黑白灰阴影关系还是很重要，老师会用纯色黑白灰或者棕色深棕色来打小色稿。
- 透视：不学是不行的。风景画，尤其是城市建筑街景都会用到。

今年的水彩作业：

![watercolor-1](/img/in-post/watercolor-1.JPG)
![watercolor-2](/img/in-post/watercolor-2.JPG)

---
## 水彩学习资料:
- [Watercolor by Shibasaki](https://www.youtube.com/c/WatercolorbyShibasaki) 日本老爷爷教水彩
- [水彩咖啡厅 - Eric](https://www.youtube.com/c/%E6%B0%B4%E5%BD%A9%E5%92%96%E5%95%A1%E5%BB%B3Eric/videos?view=0&sort=p&flow=grid)
- [Makoccino](https://www.youtube.com/c/Makoccino/videos) 很适合水彩新手入门的小教程

---

### Troubleshooting:

很长时间没有更新blog的结果是 -- 这篇博文花了很久才成功发布。主要原因如下：
1. 发现新的commit没法开始一个新的Github Actions workflow，最后发现是因为几个月前我把自己的Github.io github repo设置为private了……（可以在Settings > Code and automation > Pages看到提醒）
2. 其他文章都可以正常更新，但这篇发布后在网站上根本看不见……检查了Github Actions的 “pages build and deployment > build > Build with Jekyll” 发现这行信息:
    ```
    Skipping: _posts/2022-06-02-watercolor.md has a future date
    ```
    原因是文章date值默认是按照UTC时间来设置（https://stackoverflow.com/questions/30625044/jekyll-post-not-generated）

人生跨入新的阶段，之后争取多多更新blogs！