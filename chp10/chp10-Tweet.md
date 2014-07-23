# 第10章 推特，推特

![tweet](./img/chp10-1.png)

我们已在数据科学的道路上取得了很大进步：学习了基本的R技能，接触了R-studio，了解了管理第三方包的相关知识，有了创建函数，使用基本描述性统计以及采样分布与统计推论的的经验。本章，我们将使用推特这一社交媒体服务来获取并处理实时数据。

本章之前我们使用的都是一些玩具数据：虚构的的家庭数据和美国50个州以及哥伦比亚特区的人口普查数据。然而此时我们已经实践了一系列处理真实数据的技能。数据无处不在，成千上万，许多可以自由取用，包括心理学实验到电影演员等一系列有趣的话题。然而从数据的及时性来讲，没有什么能比推特社交服务更好了。你可能从实际使用经验中了解了推特是一种允许用户向全世界的人广播自己的思想的微博服务（小于140字），之后用户的粉丝（follower，即其他订阅了发送者消息的人）会阅读到该信息。推特的开发者的天才之举是决定让这些消息发布-即推特，通过twitter.com的网站对公众开放，同时支持API调用。

有了API，R对Twitter的扩展就是很自然的了。Jeff Gentry，一个财务服务行业的数据专业人士开发了twitteR（不确定如何发音，不过"twit-are"应该很相近了）这个三方包。twitteR包提供了相当简单的接口通过推特服务下载推特列表。利用twitteR的接口，可以搜索某一主题的相关推特。每条推特包含了作者发的推特文本以及许多其它有用信息，例如推特的发送时间。将这一切整合在一起，为我们提供了一种有趣的方式来观察人们对各种主题的实时思考。

使用twitteR的另一重要好处是，我们将使用大量本书前面学习的技能。

## 令牌：使用OAuth

在我们加入Rstudio创建代码前，我们还有一个重要步骤需要在推特网站完成。

2013年，推特完成了新版本应用编程接口（API）的迁移。新的API要求特定授权机-在通过软件应用搜索推特是向推特网站证明你就是声明是的那个人。推特网站的易购采用了行业标准OAuth来完成这一过程。OAuth提供了获取"secret"和"key"信息的方法，没有它将使与推特网站交互即使不是不可能也是极为困难。一下是具体步骤：

1. 如果你没有帐号，请在Twitter.com网站创建一个帐号
2. 以你的个人账户登录推特网站的开发页面 (https://dev.twitter.com)
3. 点击"My Applications"。它在网站的具体位置可能会随着时间不同而不同，不过你可以在屏幕右上角你的个人Profile图像处的下拉列表去看看。
4. 点击"Create a New Application."，回答空白处问题，填上你的主页。这是必须填写的内容，因此你需要填写响应指向的页面。相反“Callback URL”可以为空白。之后点击提交。
5. 选中下图设置中的checkbox，这样你的应用才能登陆推特网站。
![develop](./img/chp10-2.png)
6. 接着你将看到一个大量数据的屏幕，记得将它们全部保存下来。而你真正需要的部分则是"Consumer key"和"Consumer Secret"，这两部分都是字符串和数字。你的应用在R里面运行时将需要这些字符串。字符串这么长是由于它们被加密的原因。
7. 记录下来Request Token URL 和Authorize URL，大部分情况下这些不会变化，但是可能随着时间推移出现改动。
8. 点击Settings，确定设置了"Read, Write and
Access direct messages"

你可能也注意到了dev.twitter.com网站的Home->My applications窗口还有其他与OAuth相关的任务tab。通过其中"OAuth tool"你可以找回Consumer key和Consumer secret。本章会将如何使用Consumer key和Consumer secret，不过先我们需要设置好twitteR包。

## 使用twitteR

开始使用twitteR之前需要启动Rstudio，首先创建一个Rstudio "项目"，Rstudio中的项目可以把一个数据分析任务的数据集，你的变量，函数等各种信息整合在一起。对专业的R和Rstudio人生而言，为每一个项目分析任务建立一个项目是极其重要的：这样使得不同数据集，变量不会相互影响。在Rstudio中点击"Project"菜单，然后选择“New Project”，通常你会有三个不同的新项目选择，全新的“干净”项目，从一个已经有文件的目录建立项目，或者来自版本控制项目的项目。（本书后面会介绍版本控制，这对涉及多人协作的项目是极其有用的）。现在选择"New Directory"来开始一个全新项目，你可以给你的项目去任意名称，不过因为这个项目使用twitteR包，你可能会将项目命名为“twitter”。这时也还可以选择Rstudio将在电脑的哪个目录下创建你的项目。

Rstudio将启动一个干净的控制窗口以及没有任何我们在前面章节创建的变量的全新“工作空间”。使用twitteR，我们还需要加载“bitops”，“RCurl”，“RJSONIO”包，之后“twitteR”才能加载。现在让我们写一个函数来完成这一工作，而无需手工来执行没一步。首先该函数以包名为输入，它会检测该包是否已经“安装”，如果没有安装，我们的函数将进行安装，之后使用require函数将该包加载。我们给函数取名“EnsurePackge”，因为它确保我们需要的包可用。如果你不记得该怎么做了，你应该点击“File”菜单，之后点击“New”建立一个新的R脚本文件。之后输入或复制下列代码到文件中：


```r
EnsurePackage<-function(x)
{
	x <- as.character(x)
	if (!require(x,character.only=TRUE))
	{
		install.packages(pkgs=x,
		repos="http://cran.r-project.org")
		require(x,character.only=TRUE)
	}
}
```

在Windows中，你需要保证你在新的R包存储的目录中有写权限。你可以在Windows文件浏览器中点击该目录，选择“属性->安全“，之后选择你的用户名和用户组，点击编辑，赋予所有权限。如果仍有问题，你可以查看CRAN上的Windows常见问题（FAQ）[http://cran.r-project.org/bin/windows/base/rw-FAQ.html](http://cran.r-project.org/bin/windows/base/rw-FAQ.html)

上面代码的第4行的require()函数与前面章节学习的library()函数功能相同，不过require函数在参数中传递的包不存在时还同时返回“FALSE”。同样是第4行代码中，这里还用到了"if"语句这个新概念。计算机科学家将它称为条件表达式。条件表达式会检查括号中的语句执行是否TRUE或FALSE。如果为TRUE，程序会执行大括号内的脚本（4～8行代码）。如果为FALSE，大括号中的语句将被跳过。第3行代码中"x"是将传入"require()"函数的包名，"character.only=TRUE"告诉"require()"函数，传入的x将是字符串。


