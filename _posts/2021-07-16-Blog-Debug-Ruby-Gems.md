---
title: Gemfile.lock可以被gitignore吗？
layout: post
subtitle: null
date: '2021-07-16 21:30:00'
author: Lanzhou
header-img: img/post-bg-unix-linux.jpg
tags:
- blog
- debug
- ruby
---

之所以想到标题的问题是因为……

## 本地运行Jekyll博客时遇到了新的Error message

面对周末墨尔本的lockdown非常绝望，而众所周知抵御绝望最好的方法就是（~~打游戏~~）努力学习！
所以我打开电脑准备整理近期学到的内容并更新之前发布的博文。

### 今天遇到的Bug：
发现今天使用的电脑中的本地Jekyll博客repo并不包含最新内容，所以从远端`git pull`，确定更新了本地repo之后用`bundle exec jekyll serve`运行博客网站……然后就看到了如下Error message:
```
.../ruby/bin/bundle:23:in `load': cannot load such file -- .../ruby/gems/3.0.0/gems/bundler-2.2.15/exe/bundle (LoadError)
        from .../ruby/bin/bundle:23:in `<main>'
```
运行`bundle install`也会出现相同的error。

### 尝试的方法：
根据一篇stackoverflow上的建议尝试运行了`gem update --system`，结果没什么用。

### 解决方法：
根据stackoverflow上的另一篇回答，回答中指出“我本地的 Gemfile.lock 文件以某种方式决定了bundle命令的运行方式，所以需要移出Gemfile.lock重新运行`bundle install`”，由此发现可能是因为自己本地文件夹中的`Gemfile.lock`文件中Bundler版本号不符合本地Bundler版本。用`bundler -v`查询了本机Bundler版本号：`Bundler version 2.2.24`。

而`Gemfile.lock`中的内容是：
```
BUNDLED WITH
   2.2.15
```
在删除了本地Gemfile.lock后，重新运行`bundle install`，就成功创建了新的`Gemfile.lock`文件，其中Bundler版本号也改成了2.2.24，再次运行`bundle exec jekyll serve`就可以本地访问博客没有再出现问题了。



---

## 是否可以把Gemfile.lock文件加入.gitignore？

如何避免未来再次发生类似的问题呢？由此我产生了这样的疑问，是否Gemfile.lock文件根本就不应该出现在remote Github repo中？

一个非常简明扼要的回答：取决于你在做什么，如果你在做一个ruby gem，那么**不能**把Gemfile.lock放进项目中，而如果你在做一个Rails app，那么应当把Gemfile.lock保留在项目中。在Yehuda Katz的博文中（见参考资料），他解释了这一点：“Gems 依赖于一个名字和一个版本范围, 并且不关心dependencies具体从何而来。 Rails apps 的发布更为可控, 并需要保证所有环境(dev, ci, production)使用完全一致的代码。”

以[colorize](https://rubygems.org/gems/colorize)这个ruby gem为例，在我们使用它的时候，我们会在gemfile中添加`gem 'colorize', '~> 0.8.1'`，而这个gem command并不会使这个gem的dependencies精确依据Gemfile.lock文件中的版本。（就算可以也没人想要这样做，因为用户--主要是开发者们在使用gem的时候并无需使用你开发gem时使用的那些dependencies的相同版本）

如果你在开发一个app，需要考虑到发布、维护的整个过程，那么你应当保留Gemfile.lock，因为在不同环境都需要使用bundler tool，所以dependencies的版本精确度就很重要，它保证了你在各个环境都在运行相同的app。

---
## Gemfile中的“~>”符号是什么意思？

“~>”符号表示的是版本约束，在之前的例子中`gem 'colorize', '~> 0.8.1'`，“~> 0.8.1”表示的是：
```
gem "cucumber", ">=0.8.5", "<0.9.0"
```
代表这里的三位数字中最后一位数字“1”可以递增，直到达到最大版本，而这个最大版本将小于0.9.0，因为前面的两位数字“0”和“8”都不能递增。

这种用法表现的场合可能是你觉得0.9版本会是一个重大修改，而0.8.x版本可能都只是bug修正。

如果仅仅使用“>=0.8.1”那么只要版本比0.8.1等于或比它更新都可以接受，超过0.9也可以，不会有上限。

（如果文中有错误欢迎指正，如果有建议和疑惑也欢迎写在评论中。）

**后记：**

发现debug之后允许自己花一点时间“掉进兔子洞（go down a rabbit hole）”是很好的学习方法，在试图解决问题的过程中，一个问题会把人引向另一个问题……工作的时候没什么时间去深挖问题背后的问题，自己学习的时候觉得花些时间也很值得。这也让我想起去年三月份刚开始转行学习ruby时发布过的一个ruby gem作业：[DinnerChoice](https://rubygems.org/gems/DinnerChoice)，发现果然还保留着Gemfile.lock……现在已经快过去一年半了，回头看自己写的代码发现了很多可以改进的地方，可以新增加的功能，以后有时间可以继续琢磨改进。记得一位导师也曾经说过，根据学到的内容不停地改进同一个项目是非常好的学习方法。

---
### 参考资料：
   - [Bundle gem load error in Ruby - Paul Meinshausen](https://stackoverflow.com/a/61627799/12346593)
   - [Should Gemfile.lock be included in .gitignore?](https://stackoverflow.com/questions/4151495/should-gemfile-lock-be-included-in-gitignore/4151540#4151540)
   - [Clarifying the Roles of the .gemspec and Gemfile](https://yehudakatz.com/2010/12/16/clarifying-the-roles-of-the-gemspec-and-gemfile/)
   - [在Gemfile中指定rubygem时，〜>和> =之间有什么区别？](https://www.it-swarm.cn/zh/ruby/%E5%9C%A8gemfile%E4%B8%AD%E6%8C%87%E5%AE%9Arubygem%E6%97%B6%EF%BC%8C%E3%80%9Cgt%E5%92%8Cgt-%E4%B9%8B%E9%97%B4%E6%9C%89%E4%BB%80%E4%B9%88%E5%8C%BA%E5%88%AB%EF%BC%9F/970519493/)


