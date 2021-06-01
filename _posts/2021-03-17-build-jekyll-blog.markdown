---
layout: post
title:  "Github Pages + Jekyll搭建极简博客"
subtitle:
date:   2021-03-17 23:12:36 +1100
author:     "Lanzhou"
header-img: "img/post-bg-unix-linux.jpg"
tags:
  - blog
  - jekyll
  - 中文
  - project
# categories: blog
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
在install jekyll的时候发现当前ruby是2.3.7但需要ruby更新的版本。

首先需要安装rvm(ruby version manager)，根据RVM安装说明（[Installing RVM](https://rvm.io/rvm/install)）首先需要“install GPG keys used to verify installation package”，但使用安装说明给出的代码 `pgp --keyserver hkp://pool.sks-keyservers.net ...`可能会出现这样的error:`gpg: keyserver receive failed: No route to host`

解决方法如下：
```
gpg --keyserver hkp://ipv4.pool.sks-keyservers.net --recv-keys 409B6B1796C275462A1703113804BB82D39DC0E3 7D2BAF1CF37B13E2069D6956105BD0E739499BDB

curl -L https://get.rvm.io | bash -s stable
```

然后就可以继续根据这篇教程[“How to update from ruby 2.3 to 2.6”](https://help.learn.co/en/articles/2789231-how-to-upgrade-from-ruby-2-3-to-2-6)把ruby更新成2.6.1的版本啦。

现在应该可以成功安装Jekyll了，恭喜！✿✿ヽ(°▽°)ノ✿
（安装RVM的过程卡了我好久……）

### 本地使用Jekyll
这一部分也是根据王爵老师的教程。[“Jekyll 博客系列 - 01 快速入门”](https://www.youtube.com/watch?v=Zt_QzSbyDcw&list=PLK2w-tGRdrj7vzX7Y-GqKPb2QPrHCYZY1&index=1)
在想存放`myblog`的文件夹中
```
jekyll new myblog
cd myblog/
sudo bundle install
```
本地运行博客网站：
```
bundle exec jekyll serve
```
然后根据log去Server address访问就好：
```Server address: http://127.0.0.1:4000/```

可以修改`_config.yml`文件和`_posts`里的内容，添加新的博文。
可以添加一个新的文件夹`_drafts`用于放草稿。
添加模板等等更多内容可以看教程，网上很多。

### 使用Github Pages来发布博客
在Github上创建一个新的repo，名字是`XXX.github.io`就可以了，XXX是github用户名。
然后进入`myblog`文件夹:
```
git init
git add .
git commit -m "<commit message>"
git remote add origin git@github.com:<github-username>/<github-username>.github.io.git
git push
```

然后就可以在github-username.github.io看到自己的博客页面了！
✿✿ヽ(°▽°)ノ✿

发布博客参考教程：
- [“Jekyll 博客系列 - 02 配置域名访问”](https://www.youtube.com/watch?v=7lFxEsF5Rw0&list=PLK2w-tGRdrj7vzX7Y-GqKPb2QPrHCYZY1&index=2)
- [“三分钟在GitHub上搭建个人博客”](https://zhuanlan.zhihu.com/p/28321740)

成功发布博客之后会看到第一篇post：

*Title: Welcome to Jekyll!*

You’ll find this post in your `_posts` directory. Go ahead and edit it and re-build the site to see your changes. You can rebuild the site in many different ways, but the most common way is to run `jekyll serve`, which launches a web server and auto-regenerates your site when a file is updated.

Jekyll requires blog post files to be named according to the following format:

`YEAR-MONTH-DAY-title.MARKUP`

Where `YEAR` is a four-digit number, `MONTH` and `DAY` are both two-digit numbers, and `MARKUP` is the file extension representing the format used in the file. After that, include the necessary front matter. Take a look at the source for this post to get an idea about how it works.

Jekyll also offers powerful support for code snippets:

{% highlight ruby %}
def print_hi(name)
  puts "Hi, #{name}"
end
print_hi('Tom')
#=> prints 'Hi, Tom' to STDOUT.
{% endhighlight %}

Check out the [Jekyll docs][jekyll-docs] for more info on how to get the most out of Jekyll. File all bugs/feature requests at [Jekyll’s GitHub repo][jekyll-gh]. If you have questions, you can ask them on [Jekyll Talk][jekyll-talk].


[jekyll-docs]: https://jekyllrb.com/docs/home
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-talk]: https://talk.jekyllrb.com/