Note:最终的流程以及翻译人员本周五确定，所以以下内容可能修改。翻译工作最早周三后才开始

Windows用户可以使用markdownpad软件打开本文件，Linux用户（你都用linux肯定会google:-）搜一下有什么markdown eidtor 就可以。其实任何文本编辑器都可以编辑查看markdown文件，有编辑器只是可以让你看到显示效果

# 书的下载地址

请查看群共享，DataScienceBookV3 一书

# 工具

## 翻译采用Markdown语言

1. Markdown是一名超级简单的书写语言，你可以在1小时内学会它，详细内容[点这里](http://wowubuntu.com/markdown/)  
[简书上的介绍](http://jianshu.io/p/q81RER)

2. 你可以采用任何文本编辑器编辑markdown文件，如果你使用Windows，你可以用[markdownpad](http://www.markdownpad.com/)，也可以用[Sublime](sublimetext.com)。Sublime也支持Linux，或者你可以用Vim，Emacs。

3. 书中的R的相关内容是可采用Rstudio的[R markdown](http://rmarkdown.rstudio.com/)来完成

所以准确说，我们是采用R Markdown来翻译。因此你需要安装的软件有：

1. R 这个可以在 [CRAN](cran.r-project.org)下载
2. [R studio](http://www.rstudio.com/)
3. 在R Studio安装后，启动R Studio，新建 R markdown文件就可以开始你的翻译了。之后你点击Knitr HTML就可以看到对应的html输出

## 协作方式采用Github

我们协作翻译采用Git继续版本管理，如果你不了解Git可以先看看下面的参考链接：

[Github快速入门手册](http://blog.csdn.net/michaelfeng1024/article/details/7047619)  
[Git简易指南](http://www.bootcss.com/p/git-guide/)  
[另一个Git与Github的使用](http://www.diguage.com/archives/42.html)  
[更详细的github使用](http://www.worldhello.net/gotgithub/)  
[阮一峰的git介绍](http://www.ruanyifeng.com/blog/2012/07/git.html)

## QQ

平时的一些讨论，我们使用QQ群


# 协作模式

## Git的使用


1. 首先fork我们的项目
2. 把fork过去的项目也就是你的项目clone到你的本地
3. 在命令行运行 git branch develop 来创建一个新分支
4. 运行 git checkout develop 来切换到新分支
5. 运行 git remote add upstream https://github.com/johnstart/data-science.git 把我们的库添加为远端库
6. 运行 git fetch upstream gh-pages 拉取我们的库的更新到本地
7. 运行 git rebase upstream/gh-pages 将我们的更新合并到你的分支


这是一个初始化流程，只需要做一遍就行，之后请一直在develop分支进行翻译。

如果翻译过程中我的库有了更新，请重复6、7步。翻译完成之后，首先push到你的库，然后登录GitHub，在你的库的首页可以看到一个 pull request 按钮，点击它，填写一些说明信息，然后提交即可。

## 任务安排

翻译采取自愿模式进行，请大家更新本地git repo后，检查task.md文件，查找当前无人认领的翻译任务。

### 任务认领

我们的任务采取认领方式，每个任务2人认领，2人认领完任务后，下来自己协调该任务的翻译工作:

1. fetch最新的task.md文件，查找无人认领任务
2. 选择任务，将自己的qq群里面名字写作该任务后面，例如  
chp1 负责人 [机器人](qq) [Ripe](qq)
3. 将你本地task.md修改pull到我们的repo中，并在QQ中发一下消息，让大家知道，因为有时可能你的pull request，我们没有及时更新到最终的翻译repo
4. 如果当前任务都已经认领完毕，你可以联系任何一个已经认领任务的负责人，请求参加Review

### 任务跟踪

Note： 2014.7.2~2014.7.12 翻译前10章，请勿认领其它章节。

1. 任务认领后2天，如果没有任何更新，管理员将联系任务人，如果因为时间原因无法投入，我们将把任务回收
2. 每周所有翻译人员最少开会一次，讨论翻译问题，协调问题。目前暂时定在周6或周日晚8点（待定）

# 翻译注意事项

## 文件命名

文件名统一命名为英文方式，不能使用中文名。比如第一章命名 chp1-

## 图片

图片目录统一放在img 目录下，因此如果需要引用图片时采用这样的格式`![图片](./img/图片.png)`

## 标题

章的标题用`#`，下面小节标题用`##`，依次类推