---
title: 用theme美化，用plugin强化Jekyll blog
layout: post
subtitle: null
date: '2021-03-20 23:54:00'
author: Lanzhou
header-img: img/post-bg-unix-linux.jpg
tags:
- 博客
- jekyll
- 中文
---

今天用别人做好的主题将博客从原来的“极简风”修改成了现在的样子，愉快。

参考的是这位博主主题：[Hux blog](http://huangxuan.me/)<br>
Github repo：[Huxpro/huxpro.github.io](https://github.com/huxpro/huxpro.github.io)<br>
模板的Github repo：[Huxpro/huxblog-boilerplate](https://github.com/Huxpro/huxblog-boilerplate)<br>
（我是直接使用模板的，感觉比较方便）

更高兴的是通过trouble shooting解决了deploy到Github Pages出现的问题，在主题作者的Template repo中提交了一个新的issue和修改这个问题的pull request。

今天学到的事情：

- [Securing your GitHub Pages site with HTTPS](https://docs.github.com/en/github/working-with-github-pages/securing-your-github-pages-site-with-https)
    Trouble shooting的时候学到的事情。
    > Enforce HTTPS --Required for your site because you are using the default domain (lanzhou-j.github.io)

- 学会使用Jekyll plugins，比如jekyll-seo-tag，jekyll-admin，jekyll-feed等等。
    <br>参考的是这个教程：[Jekyll 博客系列 - 03 使用插件让博客更强大](https://www.youtube.com/watch?v=_zf8oeI3Ov8&list=PLK2w-tGRdrj7vzX7Y-GqKPb2QPrHCYZY1&index=3)

- 学会给别人的github repo提交New Issue和提供修改的Pull Request。