---
layout: post
title:  "免费搭建一个基于Jekyll的博客站点"
tags:
  - 博客
  - jekyll
  - 中文
date:   2021-03-17 23:12:36 +1100
categories: jekyll update
---

今天终于建起了一个看起来还挺好用的Tech blog，开心。
参考的视频是Youtube王爵老师的视频
[“Jekyll博客系列”](https://www.youtube.com/playlist?list=PLK2w-tGRdrj7vzX7Y-GqKPb2QPrHCYZY1)

在follow教程安装Jekyll的过程中也遇到了一些问题，比如更新ruby的版本，安装rvm等等，把今天制作Tech blog的开发过程记录一下。

### 安装Jekyll
```
brew install ruby
sudo gem install jekyll
sudo gem install jekyll bundler
```

### 关于Ruby的版本问题：
如果装过ruby了可以用`ruby -v`来检查ruby的当前版本。
在install jekyll的时候发现当前ruby是2.3.7但需要ruby更新的版本，所以尝试着根据这篇教程[“How to update from ruby 2.3 to 2.6”](https://help.learn.co/en/articles/2789231-how-to-upgrade-from-ruby-2-3-to-2-6)把ruby更新成了2.6.1的版本。

今天最让我头大的问题是安装rvm(ruby version manager)，根据RVM安装说明（[Installing RVM](https://rvm.io/rvm/install)）首先需要“install GPG keys used to verify installation package”，但使用安装说明给出的代码 `pgp --keyserver hkp://pool.sks-keyservers.net ...`可能会出现这样的error:`gpg: keyserver receive failed: No route to host`

解决方法如下：
```
gpg --keyserver hkp://ipv4.pool.sks-keyservers.net --recv-keys 409B6B1796C275462A1703113804BB82D39DC0E3 7D2BAF1CF37B13E2069D6956105BD0E739499BDB
```