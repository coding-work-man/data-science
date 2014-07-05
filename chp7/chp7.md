罐子中的样本
==========

抽样分布是理解统计推断的关键概念。为了理解随机性在抽样时所产生的影响，很多讲解抽样分布的方法都提到了从罐子中抽取弹球或糖果这样的经典例子。通过前面提到的美国人口的例子，我们可以清楚的看到尽管总体的分布不是正态的，但其样本均值的分布却服从正态分布。

想象你拥有一个装满了红色和蓝色糖果的罐子。罐子中放入的红色糖果和蓝色糖果各有100颗，但这些糖果在放入罐中时是混合在一起的。如果你从罐中随机抽取8颗糖果，你会得到什么颜色的糖果？如果抽取如想象中般顺利的话（虽然通常都没那么走运），你将会抽到4颗红糖果和4颗蓝糖果。红的和蓝的各占一半，这个比例和罐中的红蓝比例是一样的。当然了，你很难抽到这个结果，是不？除了4红4蓝的组合，你也可能会抽到3颗红糖果和5颗蓝糖果，或者其他各种你能想到的组合。事实上，尽管概率很小，但你还是有可能抽中8颗红色的。虽然有多种可能，但仅就一次抽取结果来看，我们是无法预测会抽到红和蓝的哪种组合的。而这种不确定性，就是能够以无法预知的方式来影响你抽取结果的随机性的力量。

我们现在提出一个有趣的想法，这虽然对预测单次抽取结果没什么帮助，但却能很好的展示多次抽取的*长期趋势*。从罐子中抽取8颗糖果，数一数其中有多少红色的，然后将所有的糖果都放回罐中。我们并不需要数蓝色糖果的数量，这是因为 `8 - 红色糖果的数量 = 蓝色糖果的数量`。将罐中的糖果晃动一下，这样能混合的更好一些，然后再抽取8颗糖果，同时数一下红色糖果的数量。将这种操作重复多次，我们可以对每次记录的结果用下面的表格来表示：
<center>
<table>
   <tr>
      <td>抽取次数</td>
      <td>红色糖果的数量</td>
   </tr>
   <tr>
      <td><center>1</td>
      <td><center>5</td>

   </tr>
   <tr>
      <td><center>2</td>
      <td><center>3</td>
   </tr>
   <tr>
      <td><center>3</td>
      <td><center>6</td>
   </tr>
   <tr>
      <td><center>4</td>
      <td><center>2</td>
   </tr>
</table>
</center>

注意到表格左边的列就是对抽取次数的计数而已。表格右边的列是我们感兴趣的内容，它列出了每次抽样中的红色糖果数量。在本例中，红色糖果的数量真是天南海北。在第4次抽样时，我们只抽到了两颗红色糖果，但是在第3次抽样时，我们却抽到了足足6颗红色糖果。但是本例最有趣的地方就是，如果你将这4次抽取的红色糖果数量进行*算术平均*，你会得到平均每次抽到*正好4颗红色糖果*的结果，而这正是我们期望的红蓝各半的罐子中应该抽出的结果。以上只是一个人为制造的例子，而我们在平时抽取时，很难在仅仅4次抽取之后就遇到这么好的平均的结果。但是如果你将随机抽取这一操作，进行了4000次，你将会肯定得到一个接近 **平均每次抽到4颗红色糖果** 的完美结果。

从总体中不断的抽取子集的过程称为“抽样”，而经过大量的抽样之后，我们会最终得到抽样分布。注意到，在刚才这句话中，我们用到了“总体”这个词汇，用来代表我们从什么地方做的抽样，这也是它的统计含义。在前面章节的例子中，我们的数据集包含美国各个州的人口数量，这个“人口”与“总体”同样使用英文单次 population 可以说是一个巧合。下面，我们使用 R 来帮助我们从美国各州人口数据集中抽取大量样本。

R 中有一个很便捷的函数 sample()，可以用来从数据集中简便的抽取样本。我们可以用它来从我们的数据集中抽取样本：




```r
set.seed(31415)
sample(USstatePops$V1, size = 16, replace = TRUE)
```

```
##  [1]  1852994  4339367  3574097  2967297  2915918  4533372  1360301
##  [8]  5773552  6724540  6346105  3574097  3751351  1052567 37253956
## [15]   814180  3046355
```


(译者注：原著中并未使用set.seed()来设置随机的种子，这会导致读者的可重复性研究非常困难。在本章中我们统一将种子设置为31415。读者也如此设置，便可实现和我们相同的抽样结果。)在本例中，注意到我们调用的sample()函数含有三个参数。第一个参数是数据源。
对于第二个和第三个参数，为了让 R 能够正确识别，我们采用了“命名参数”的方
式来调用，而不是单靠这些参数在参数列中的位置。参数 `size = 16` 可以让 R 
从总体中抽取16个州的人口作为样本。参数 `replace = TRUE` 则规定了一种统计
学家经常用来简化数学证明的抽样方法。对我们来说，采取放回抽样还是不放回抽
样通常没有实际的影响，所以我们直接使用统计学家的常用方法。

相较于给抽取的各色糖果计数，我们在处理类似于各州人口的这类数值时，对其算数平均数，或者称为均值，更感兴趣。于是我们可以让 R 使用mean()来计算所抽取的样本均值：


```r
set.seed(31415)
mean(sample(USstatePops$V1, size = 16, replace = TRUE))
```

```
## [1] 5617503
```


从上面的mean()函数可以看出，我们又遇到了函数的嵌套调用了。我们在mean()函
数的输出中并未看到从51个州中随机抽取的16个州人口数值，而是看到了mean()函
数直接用这16个数来为我们计算出来的均值。如果你的记忆力很好，或者往前面的
章节翻一下，你会看到51个数值的均值是 6053834. 于是你会发现我们从这16个州
的样本获得的均值真的是离总体的均值相差的有点远。我们是不是应该感到焦虑？
当然不是！我们知道，当抽取样本时，无论是抽取糖果，还是抽取各个州，我们的
样本均值永远都不会和总体均值正好一样。其实我们并非对某一次抽样的结果感兴
趣，而是要专注于长期抽样的结果。于是，我们现在要让 R 来帮我们做重复抽样了，不是抽取1次、4次，而是400次甚至4000次。和其他的编程语言一样，R 有很多方法可以重复某个操作。其中最简单的一个方法非replicate()函数莫属。让我们先尝试一下重复4次：


```r
set.seed(31415)
replicate(4, mean(sample(USstatePops$V1, size = 16, replace = TRUE)), simplify = TRUE)
```

```
## [1] 5617503 3945129 6510150 4407374
```


简单的不可思议吧！我们仍然使用了刚才用过的命令，即用来计算16个州的均值的
mean()函数。只不过这一次，我们将mean()函数作为一个参数放到replicate()函数
里面，于是我们才可以一次又一次的求均值。其中的 `simplify = TRUE` 参数是让 
R 将函数运行的结果以一个均值构成的向量形式返回，而这正是我们想要的形式。
由于我们只将mean()函数运行了4次，所以我们不会看到满屏的数字。从刚才的尝试
来看，我们可以很轻易地将求均值的过程重复400次。你可以自己试一下，看看结果
。至于在本书中，我们要将replicate()函数整个塞进另一个mean()函数，于是我们
可以得到400个样本均值的平均数。我们来看一下：


```r
set.seed(31415)
mean(replicate(400, mean(sample(USstatePops$V1, size = 16, replace = TRUE)), 
    simplify = TRUE))
```

```
## [1] 5983180
```


在上述命令中，最外层的mean()函数用粗体表示，以凸显其地位与之前命令的不同。将这个多层嵌套的命令的效果，用语言来描述一下就是：a)从51个州的总体中抽取400个样本容量为16的样本; b)对每一个样本计算其样本均值，并将均值保存到一个列表中; c)当计算完这400个均值后，计算包含有400个均值的列表的均值。从刚才的计算结果可以看到400个样本的均值是5983180. 虽然这个值和总体均值仍然不相等，但是已经接近了。我们距离总体均值的真值大概70000多，大概是1.16%(更精确的数值为70654/6053834 = 1.167%)。你可能也注意到了，即使你有一个速度较快的电脑，运行刚才的命令也得花一些时间。因为刚才的命令的确包含了很多操作！让我们更进一步，看看我们是否能更加接近总体均值：


```r
set.seed(31415)
mean(replicate(4000, mean(sample(USstatePops$V1, size = 16, replace = TRUE)), 
    simplify = TRUE))
```

```
## [1] 6052941
```


现在我们更加接近真值了！我们得到的均值距离总体均值的真值只差不到千分之一！