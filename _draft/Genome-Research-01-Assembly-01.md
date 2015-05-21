---
layout: post
title: '基因组研究系列（一）：基因组组装（上）'
categories:
    - 生物信息
    - 基因组学
tags:
    - 基因组组装
    - de Bruijn Graph
    - Lander-Waterman Model
---

在evernote的“测序与组装相关”的笔记本下。

<a id='introduce' name='introduce'> </a>

### 前言
----

之所以写这个，一方面是因为我打算写一个关于人类基因组学研究的一系列文章，算作是对过去这几年的一个小结，另一方面，现在能真正能够详尽讲述基因组学研究的中文材料非常少，因而，我想挑战一下自己看看能否尽可能的将它们写出来。鉴于这一领域的庞大以及我自己的认识有限，只将其局限在人类基因组研究的范围内，即便只是如此，我也没办法面面俱到，只能是想到哪写到哪了，我也不敢说这会是最前沿的，但应该可以是当前人类基因组学研究的主流方法和成果，若有疏谬还请大家斧正！

### 概述
----
测序我在去年已经写过了，这里就不再重复，直接进入下一步的基因组组装。

基因组组装是基因组学研究中的一个非常重要，同时也非常复杂的问题，它的复杂性主要源自于不同物种自身基因组序列特征的复杂性，比如有些植物和海洋生物他们有着巨复杂的基因组，复杂到以目前的技术手段还解决不了，像这方面的例子有[...]()。另外一个重要的因素是基因组组装所消耗的计算资源相当巨大，按照目前做的比较好的一些组装软件，如SOAPdenovo，AllPathLG，它们消耗的计算机内存资源约为物种基因组大小的100倍到150倍。我本来只想用一个篇幅的文章来概述一下基因组组装技术，但是写着写着就发现这个过程实在复杂。所以，最终只能砍成3篇了。

* （上）[]()
* （中）[]()
* （下）[]()

OK，以下进入正题。

### 什么是基因组组装，为什么要进行基因组组装
----

但在开始之前我有必要先说明一下什么是基因组组装（也可称为基因组序列构建）以及为什么要进行基因组组装这两个问题。

基因组组装就是将通过测序仪测得的物种DNA序列片段正确组合成属于该物种的一个基因组。这样说可能有点抽象，举个具体点的例子，就是把汽车厂生产出来的一堆零件组合成一辆汽车。这里测序仪就是汽车厂，测得的DNA片段就是各个的零件，这个汽车组合的过程就是组装。

你或许会疑惑，难道测序仪测出来的每条完整DNA序列不应该直接就是一个物种完完整整的DNA序列了吗，怎么还会需要组装呢？第一，对于真核生物而言绝大部分的DNA都存在于细胞核之中，要检测它就必须击破细胞，将藏在核中的DNA抽取出来，这是一个破坏的过程，再加上实际操作中的其他物理损伤，基本上就已无法保证DNA的完整性了（对原核生物来讲也类似）；第二，来自于测序技术本身的限制，当前还没有一种测序技术能够真正做到将物种的DNA**一次性！连续！完整！**地测出来，都需要先打断成一定长度的小片段才行。这两点是为什么需要基因组组装的根本原因。组装的必要性和的目自是不必多说，而且对于还没有参考序列的物种来说更是重中之重。不管是人还是其他物种，所有重要的遗传信息都在它的DNA上，获得其尽可能完整的DNA序列，是你进行所有下游注释，比较，进化，健康相关，临床应用，药物反应等等各类分析与研究的重要前提。一言以蔽之，基因组组装是深入理解一个物种的技术前提。

俗话说：“巧妇难为无米之炊”，组装之前需先测序。基因组测序是基因组学研究的一个核心手段，这几年来，确切来说是自2007年以来短测序技术的发展速度非常快——关于测序技术的原理和发展历程我在去年的文章[三代基因组测序技术原理简介](/2013/08/02/An-Introduction-of-NGS-Sequence.html)中已作了比较详细的描述，这里就不再赘述。总的说来，基因组测序技术已从传统的Sanger法发展到现在成熟的第二代边合成边测序，并且也已步入第三代的单分子测序技术领域。

基因组测序成本目前正在快速转入“白菜价”，如今年初illumina就宣称测序成本已经进入1000刀（尽管它的计算方式完全是一个理想情形的计算），通量也越来越高，我想到基因组测序成为常态的那一天已经不会太久了。但序列读长Sanger法的800bp左右，降到到现在第二代高通量的100bp，150bp读长的序列（以illumina为代表），考虑到目前第三代测序技术错误率偏高，应用也仍不普遍，这里暂不讨论与第三代测序技术相关的组装问题。

就基因组组装而言序列读长是一个非常重要的因素，但每一次测序技术革新中序列读长的变化都会显著影响基因组组装技术的发展方向。传统的Sanger测序法中，由于测序的序列读长比较长，可以通过测序片段之间的重叠来确定测序片段的位置，最终组装出基因组，称之为overlap-layout-consensus的组装方法，也是“人类基因组计划”中主要使用的方法。但当前的第二代高通量测序读长只有Sanger法的八分之一到五分之一，原来的overlap-layout-consensus方法就不再适用于这些短序列测序数据的组装【原因是什么没说明白？】，而是基于*de* Bruijn Graph的图论方法来解决这些短测序读长的基因组组装。或许你会觉得用了不同的测序方法就得换一个不同的组装技术真是一个麻烦的事情，难道就不能有一个稳定点的标准吗！？有也没有！说有是因为万变不离其宗，归根到底它们都是一个数学问题，在数学的理论层面上它是不变的，说没有，是因为实际的应用过程中资源是有限的（例如？），所以接下来我将先分别从基因组组装的数学模型和计算机算法实现这两大方面进行详细的讲解，限于篇幅，这一篇先讲解基因组组装的数学模型。

（组装容易吗？为什么难，计算资源，内存，基因组的序列复杂情况）

### 基因组组装的数学模型
----

Lander-Waterman模型是基因组组装的一个基本数学模型，这个模型从数学的角度对基因组组装进行了分析，对组装的结果进行预测，从中我们可以对影响组装结果的各个因素进行分析。

但在开始讲解Lander-Waterman模型之前，我们需要对测序的术语有一个基本的认识。在全基因组测序中，需要先利用超声波将**一定个数**（注意并非只是一个！）的原始**全长**基因组随机打断成许多个在一定长度范围内的小DNA片段，这些小DNA片段相当于是原来基因组上的一段段克隆，称为克隆片段，更流行的说法是称之为插入片段，之所以叫做插入片段，是因为在测序之前需要在这些克隆片段两端加上**测序接头**，这看起来就宛如是一段序列加插在两段接头序列之间，因此得名。这些小插入片段的集合称为**测序文库**，而用来构建这一文库的原始全长基因组的数目称为基因组**覆盖深度**。测序之后，我们会得到一条条更短，但长度却完全一致的小序列，称为read，read的长度就是上文提到的**读长**，二代测序的读长都很短，**大多数**只有100bp，也即是100个碱基。由于我们测序的时候覆盖深度一般都是很大的（>30层，用于组装的话会更大，一般要在80层左右），所以每条read基本上都会有其他的read与它存在一部分（甚至完全）的重叠，根据这些read之间的重叠就能进一步形成较长的片段，称为contig序列（重叠群），其实就是由多个read合在一起变长了的序列而已，这样正是基因组组装的目的——尽可能获得最长的contig序列。

基因组DNA序列组装的一个基本思想就是通过这些read序列之间的重叠信息排列出各个read的先后顺序，然后把它们串起来，最终还原成原始的基因组序列。对应的数学问题，其实就是求解一系列字符串的最大公共子序列的问题，但你千万不要以为实现起来会如算法理论中所提到的那般简单，这在后面的篇幅中我还会讲到。Lander-Waterman模型给出了组装问题的理论数学模型。

任何一个数学模型的开始都需要预先定义相关假设，以下给出**Lander-Waterman模型的假设**：  
1. G，以bp（base pair，即碱基数）为单位，基因组的长度；   
2. L，以bp（base pair，即碱基数）为单位，每条read的长度，该值是恒定的，如上文的100bp；  
3. N，这些**随机分布**的read的总数目；  
4. α=N/G，显然，这表示基因组上<strong><span style="color: #ff0000;">某个碱基</span></strong>是一个read的**起始碱基**（也即第一个碱基）的概率；  
5. T，以bp（base pair，即碱基数）为单位，表示两个read序列之间相互重叠的长度；  
6. θ=T/L，表示一个read中重叠部分所占的比例，$$σ=1-θ$$表示这个read非重叠部分的比例；  
7. c=LN/G，为测序的覆盖深度，相当于测序数据的总量能够将整个基因组覆盖c层。  

这里需要再多说一句，关于以上第2点认为L是恒定的，对于当前illumina以及大部分的第二代测序仪来讲是正确的，但是对于Sanger法和第三代测序技术测到的不同read，L也可以是不同的，但是我们为了分析的方便，全都假定为恒定值。

OK！接下来我们就利用以上的7个假设开始推导基因组组装的数学过程吧。

现在，我们来考虑contig序列的搭建过程。α表示基因组上的某个碱基是一个read序列起始碱基的概率，假设某个contig从这个read序列的第一个碱基开始，如果在接下来的Lσ长度中能够遇到另一个read的起始碱基，那么则说明这两个read序列之间是有重叠的，contig可以向前一个碱基一个碱基的延伸到另一个read上；反之，如果在接下来的Lσ长度中不能够遇到另一个read的起始碱基，则说明该read将不会与其他read存在重叠关系，contig的延伸就只能终止在该read上，并形成一个独立完整的contig。那么我们该如何用数学的语言来描述这样的一个contig延伸终止的过程呢？看到这里，我们其实很容易可以想到这是一个服从几何分布的过程，contig的延伸如果要在这个read上结束，那么只要这条read在接下来的Lσ个碱基中没有一个是其他read的起始碱基就行，而这个事件的概率是$${(1-\alpha)}^{L\sigma}$$，所以contig在这个read终止的概率是$$\alpha{(1-\alpha)}^{L\sigma}$$。而由于一个contig可以开始于基因组上的任何位置，因此最后得到的总contig个数为：

$$
G\times \alpha{(1-\alpha)}^{L\sigma} 
$$

我们知道$$\alpha=\frac{N}{G}$$，而L可以表示为$$L=\frac{Gc}{N}$$，L表示read的读长，虽然它是一个固定的长度，但是不要错误的以为它会是一个常量！要知道不同的测序技术，读长是不一样的。

$$
= G\times \frac{N}{G}{(1-\frac{N}{G})}^{L\sigma} 
$$



<p>
以上。希望也能对您有所帮助，<strong><span style="color: #ff0000;">但本系列只可转载和分享，谢绝一切将本系列文章做成文档等格式上传至百度，360，金山等各类文库或各类云盘的行为，还望自重，谢谢合作！</span></strong>
</p>

### 基因组组装的算法

### 基因组组装的一般流程
（0）如何调查基因组的大小(如何利用kmer估算基因组的大小)
（1）数据过滤
（2）kmer 频率分析
	1）什么是kmer，为什么kmer只能是奇数
（3）k-mer纠错的算法
（4）组装的kmer是什么
（4）pregraph
（5）contig
（6）scaffolding
（7）Gap fill
### （短序列）组装的困难之处
#### 什么是Tips，Bubble，他们是如何形成的
#### Repeat序列组装的难点，以及为什么他们是难点，如何处理。 
### 如何评估组装结果
### 组装软件比较
### 组装的应用
### 最后

*参考文献*

> 1. [Genomic mapping by fingerprinting random clones: a mathematical analysis](http://www.ncbi.nlm.nih.gov/pubmed/3294162)

{% highlight python %}
print "Hello world!"
{% endhighlight %}
