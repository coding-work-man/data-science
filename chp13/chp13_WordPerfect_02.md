在上一步中，我们用“tm”包将cleanText变量强制归入特定“Class”， 即语料库；并且将结果保存为一个新的数据对象”tweetCorpus“。这是我们第一次遇到一个”Class“。”Class“一词来源于计算机科学中的”面向对象编程“（OOP)。虽然R和面向对象编程语言（如Java）有很多不同，它仍然有很多基本功能来实现面向对象的编程。这里，我们介绍一些关于“Class”的基本知识。第一，“Class”只是一个关于数据结构的概念；第二，“Class”通过基本的数据形式来建立复杂的数据结构，例如数值型数据。例如，如果建立一个新的分类“Dashboard（数字仪表盘）”，我们可以引入“Miles Per Hour（小时里程数）“，”RPM“，以及“Fuel Level”来显示油位。由此，我们发现，R软件用户可以直接建立“类”。本例中，“tm”包的作者Ingo Feinerer，创建了一个新的门类，称之为语料库，成为了文本挖掘函数的核心数据结构（Feinerer，维也纳大学计算机科学教授，任职于数据库和人工智能研究组）。最后，本段讨论最重要的是，“类”不仅包含数据结构的概念，它还包含了函数的操作指南。换言之，“Class”是一个带有操作指引的数据对象，从简单操作，如添加、提取对象，到诸如画图之类的复杂操作过程。

在“tm”包的例子中，语料库定义了文本挖掘工作者所关心最基础的函数，一个包含了很多文件的语料集。我们一旦将文本储存到语料库中，“tm”包中自带的函数便可以使用了。上述中的最后三个命令展示了tm_map()函数的用法，该函数是tm包中非常有用的特性。在每一个用到tm_map()函数的例子中，我们将tweetCorpus作为自变量，另一个命令给语料数据变形。这里，我们进行了三次变换，首先将所有字母变成小写字母，然后去除标点符号，最后提取出所谓的“停用词”。

我们需要对停用词进行必要的解释。通过开发早期搜索引擎获得电子数据库的研究人员发现这样一个事实：一些停用词受到算法变化的影响。譬如“the”，“a”，“at”之类在文本中词频较高的词汇，对于文档的区分度就比较差了。而文档中词频较低的专有名词，动词，形容词对语料库中的文档区分度则比较高。因此，研究人员决定把常用且较短的词语除去。“停用词”一词可以追溯到上世纪六十年代，用来表示计算机操作系统由于数据处理任务毫无意义而终止。为了简化去除停用词的法则，tm包囊括了很多语言这类词的列表。在上页中的最后一个命令，我们去掉了所有常用的停用词。

这样，我们将语料处理成为了一个没有大写字母，标点符号，和停用词的词语集合。现在我们可以通过创建“文档矩阵” 这些语料进行一些统计分析了。下面tm包中的命令就新建了一个矩阵：

```r
tweetTDM <- TermDocumentMatrix(tweetCorpus)
```

```
## Error: 没有"TermDocumentMatrix"这个函数
```

```r
tweetTDM
```

```
## Error: 找不到对象'tweetTDM'
```

一个文档矩阵，有时也被称为文件-条目矩阵，具有矩形数据结构，以条目s为行，以文件s为列（在别的地方若需要反过来也可以）。条目可以是一个单个单词，比如“biology”，也可以是一个复合词汇，比如“data analysis”。通过统计学的方法来观察哪些词汇通常组成词组，然后判断词汇的形式（是否是一个词组），或者也可以通过词典的方式来解决这个问题。Tm包支持词典的方式，但是我们在这个例子中并没有使用。所以，如果一个条目，比如“data”在第一个文件中出现一次，在第二个文件中出现两次，在第三个文件中没有出现，那么条目数据的栏就会包含1，2，0。

统计量会告诉我们什么时候需要在命令行中输入tweetTDM显示结果。条目文件Matrix()函数从100个tweets中提取了375个条目s。结果矩阵有很多0；在矩阵中的37500个单元中，只有610个包括非0条目，剩下的36890个包含0。单元中有0表示特定的文件没有对应的特定条目。根据对输入tweets的最大单词长度，最大条目长度是21个词。最后，在尾行，以“Weighting”开头的表示这类数据存放在文档矩阵中。在这个案例中，我们使用了默认最简单的选项，只是以条目的形式简单地记录了次数的数量，它会出现在文集中的文件中。可以使用下面的检查函数查看文档矩阵：

```r
inspect(tweetTDM)
```

```
## Error: 没有"inspect"这个函数
```


做好输出大量数据的准备。记住矩阵摘要中的条目“sparse”了吗？Sparse指大量包含0的单元——表明特点的条目并没有出现在给定的文件中。大多数的文档矩阵都是有许多0的。由于36890/37500 = 0.98，所以这个是98%稀疏的。大部分情况下，我们为了展示或者可视化的需求，需要精选或者填补文档矩阵。Tm工具包提供了多种方法来填补0条目，但是在这个例子中，我们将这个艰巨的任务交给词云工具包。

首先，我们需要安装并库化“wordcloud”工具包。正如其他工具包在R-Studio中使用工具包接口或者EnsurePackage()函数一样，这个我们在前面的章节已经介绍过。词云工具包是由自由统计学家Ian Fellows编写的，他还为R开发了“Deducer”用户接口。Deducer提供了图形接口，允许熟悉SPSS或SAS菜单系统的用户能够不必通过命令行来使用R。

加载词云工具包之后，我们需要做一些准备，使得数据就绪，提交给云生成函数。这个函数需要两个向量作为输入变量，第一个是条目的一列，第二个是条目出现的频率。条目列和频率必须以频率优先的方式排序。为了完成这个工作，我们需要将矩阵变成无格式数据矩阵，这样才能以频率排序。下面的第一个命令可以完成这个：

```r
tdMatrix <- as.matrix(tweetTDM)
```

```
## Error: 找不到对象'tweetTDM'
```

```r
sortedMatrix <- sort(rowSums(tdMatrix), decreasing = TRUE)
```

```
## Error: 找不到对象'tdMatrix'
```

```r
cloudFrame <- data.frame(word = names(sortedMatrix), freq = sortedMatrix)
```

```
## Error: 找不到对象'sortedMatrix'
```

```r
wordcloud(cloudFrame$word, cloudFrame$freq)
```

```
## Error: 没有"wordcloud"这个函数
```


在上面的第二行，我们完成的是：计算每一行的总数，给出了所有tweets/文件一个条目的总频率。我们还将结果以高频优先排序。结果是一个命令序列：每个条目有一个频率而且每个条目的名称是频率对应的名字。

第二行到最后一行程序，我们将从之前的命令从命名列中提取的名字绑定在一起，形成带频率的数据帧。这个数据帧，也就是“cloudFrame”，包括同名列中的确切信息。“序列化矩阵”，但是cloudFrame在不同数据栏中有多种名称。这样最后一个命令就简单了，就是调用wordcloud()函数。Wordcloud()函数有许多可选参数来丰富词云，例如调整它的形状，条目出现频率等。但是，这里为了使问题简单明了，我们全部用默认的参数。我们讨论了特定数据集在wordcloud()函数下的检索词表和频率表，得到下面的图片。

<center>![Alt text](./img/chap13_02.jpg)</center>

如果回顾我们从Twitter获取数据的过程，”solar（太阳）“是频率最高的词汇也就不难理解了（即使我们去掉了所有的标签）。词频第二高的是”energy（能量）“，随后，有很多相关的词语，比如”independence（自主的）“，”green（绿色的）“，”wind（风能）“，以及”metering（测量）“。

## 章节难点

基于以前我们建立的函数，建立一个可以查询词条的函数，来进行Twitter查询，清洗文本数据，简历文档矩阵，并且将词频输入wordcloud()函数中。这个主要是”turnkey“包可以实现的功能，即通过Twitter查询建立一个词云，正如本章最初提到的Jason Davies的观点一样。

### 本章资源

[http://cran.r-project.org/web/packages/wordcloud/wordcloud.pdf](http://cran.r-project.org/web/packages/wordcloud/wordcloud.pdf)

[http://www.dbai.tuwien.ac.at/staff/feinerer/](http://www.dbai.tuwien.ac.at/staff/feinerer/)

[http://en.wikipedia.org/wiki/Document-term_matrix](http://en.wikipedia.org/wiki/Document-term_matrix)

[http://en.wikipedia.org/wiki/Stop_words](http://en.wikipedia.org/wiki/Stop_words)

[http://en.wikipedia.org/wiki/Text_mining](http://en.wikipedia.org/wiki/Text_mining)

[http://stat.ethz.ch/R-manual/R-devel/library/base/html/colSums.html](http://stat.ethz.ch/R-manual/R-devel/library/base/html/colSums.html)

[http://www.jasondavies.com/wordcloud/](http://www.jasondavies.com/wordcloud/)

### 本章代码


```r
# CleanTweets() - Takes the junk out of a vector of tweet texts
CleanTweets <- function(tweets) {
    # Remove rudundant spaces
    tweets <- str_replace_all(tweets, " ", " ")
    # Get rid of URLs
    tweets <- str_replace_all(tweets, "http//t.co/[a-z,A-Z,0-9]{8}", "")
    # Take out retweet header, there is only one
    tweets <- str_replace(tweets, "RT @[a-z,A-Z]*: ", "")
    # Get rid of hashtags
    tweets <- str_replace_all(tweets, "#[a-z,A-Z]*", "")
    # Get rid of references to other screennames
    tweets <- str_replace_all(tweets, "@[a-z,A-Z]*", "")
    return(tweets)
}
```


