---
title: Attention学习
date: 2022-08-30
description: 
comments: true
mathjax: true
categories:
- Learning-notes
tags:
- Attention
---

# CVRP问题

## 概念

Capacitated Vehicle Routing Problem：有能力约束的车辆路径调度问题

VRP：多车

CVRP：装载限制，权重

## Solution

目前CVRP问题算法有许多解决方案：

- 粒子群算法(PSO)：一种在n维搜索空间中，对用m个粒子用搜索策略进行位置更新来寻找空间中适应度最佳值的算法。

  每个粒子的位置代表优化问题的一个解。

- 爬山法
- Ptr-Net
- Attention

## Transformer

transformer指的是一个encoder和decoder类型的模型算法，attention指的是一个机制。

pointer network不同的是它的输出可以比输入多。

### 结构

![Transformer结构](https://img-blog.csdnimg.cn/ed0339c8517448d09062466e05cacbec.png#pic_center)

主要由Encoders和Decoders两部分组成，其中在论文中对Encoder和Decoder两者有6个子结构(都相同,但不共享权值)，最顶端的Encoder输出会传递给每一个Decoder。

每个Encoder输入先经过一个soft-attention层，帮助Encoder在编码单词时查看输入序列中的其他单词，输出传入全链接的全馈神经网络，每个encoder的前馈神经网络参数个数相同，但作用独立。

每个Decoder也有这种层级结构，但这之间有一个Attention层，帮助Decoder专注输入句子中对应的单词。

![在这里插入图片描述](https://img-blog.csdnimg.cn/c7fdb60bf1e44adabdf480db366efd52.png#pic_center)

### 基本单元

首先将单词变成词向量，这涉及到Word Embedding

#### Word Embdding

是NLP语言模型中对单词处理的一种方式，把单词或短语映射到一个n位的数值化向量。

- One-Hot encoding：在语料库中对单词进行0-1编码

- word to vector：

  ![img](https://img-blog.csdn.net/20180813205059927?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MjQyMTAwMQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

对所有encoder来说，可以按下图理解：

![在这里插入图片描述](https://img-blog.csdnimg.cn/4ee3068f85474ce4a6948c1f355c21db.png#pic_center)

列表的大小和词向量维度的大小是可设置的超参数，通常是数据集中最长句子的长度。

在前反馈层可以用并行化提升速率

#### Self-Attention

**transformer的重点**，于Soft Attention相比，它是Encoder内部或Decoder内部之间所发生的注意力机制，是Google AI研究所提出的预训练语言表征模型——BERT(Bidirectional Encoder Representations from Transformers)主要组成部分之一。

包括LSTM、GRU在内的RNN模型需按照固定方向顺序计算，为有效缩短远距离依赖特征之间的距离，在S-A中各隐藏层状态可通过特定计算步骤被直接联系，因此它们**不必按照固定方向顺序串联**，从而使得S-A更容易捕捉时间序列数据中互相依赖的特征。

——==此部分未完==

## Attention

### 相关论文：Attention, Learn to Solve Routing Problems!

Transformer所采用的主要算法模型即Attention**(multi-headed self-attention、masked multi-headed self-attention以及Encoder-Decoder Attention)**，所以，了解注意力机制的工作原理至关重要。

### Encoder-Decoder框架

![在这里插入图片描述](https://img-blog.csdnimg.cn/d18d2b2fdaaa4794a0f15dd7d6e4a591.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzIyMTcwOTY3,size_16,color_FFFFFF,t_70#pic_center)

### Seq2Seq模型

![在这里插入图片描述](https://img-blog.csdnimg.cn/aeea8c12365648a9a9ac294e168732ea.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzIyMTcwOTY3,size_16,color_FFFFFF,t_70#pic_center)

输入序列和输出序列的长度是可变的

### 两者之间的关系

- S2S不特指具体方法，班组输入序列和输出序列的目的都可以统称为S2S模型
- S2S使用的具体方法基本都属于ED模型的范畴

- 通常文本处理和语音识别的Encoder部分通常采用RNN模型，图像处理Encoder一般采用CNN模型

### 为什么需要Attention

因为ED之间只有一个向量c来传递信息，且c长度固定，因此输入信息太长时会丢失信息。(==为撒？==)

因此Attention主要为了解决信息过长、信息丢失问题。

Attention使得Encoder不再将整个输入序列编码成固定长度的中间向量c，而是编码成一个向量的序列，目标句子中每个单词都需学会对应源句中单词的注意力**分配概率信息**。这意味在生成每个单词时，原先都是相通的中间语义表示C会被替换成根据当前生成单词而不断变化的C<sub>i</sub>

![在这里插入图片描述](https://img-blog.csdnimg.cn/5e74bc2ab837405d8c5066d6adff2101.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzIyMTcwOTY3,size_16,color_FFFFFF,t_70#pic_center)

### Attention机制原理

#### soft-Attention

计算过程：

![在这里插入图片描述](https://img-blog.csdnimg.cn/d7552954d6314a36b57799fe8522cd9d.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzIyMTcwOTY3,size_16,color_FFFFFF,t_70#pic_center)

计算步骤：

Encoder：通过Word Embedding将汉字转化成词向量，通过模型训练得到[h<sup>1</sup>,h<sup>2</sup>,h<sup>3</sup>,h<sup>4</sup>],一般采用RNN模型(LSTM、GRU居多)

Decoder：开始解码，先用固定的start token，z<sup>0</sup>和每个h<sup>i</sup>计算(一般采用神经网络)，然后通过softmax函数得到加权c<sup>0</sup>，z<sup>0</sup>可以随机初始化，但一般采用Encoder的最后一个输出。

用c<sup>0</sup>和z<sup>0</sup>作为解码RNN输入，得到z<sup>1</sup>并预测第一个词是machine。

再继续预测就是用z<sup>1</sup>求Attention，上一个Decoder输出y<sub>i-1</sub>也会当成输入之一。

z<sup>i</sup> = f(y<sub>i-1</sub>,z<sup>i-1</sup>,c<sup>i-1</sup>)

y<sub>i</sub> = g(z<sup>i</sup>)

索引i从0开始。

#### Self- Attention

传统的Attention计算QK之间的依赖关系和相关性，而它分别计算QK自身的依赖关系。

![在这里插入图片描述](https://img-blog.csdnimg.cn/fd7eef9935ae402c9f4a0ac8aa68e944.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzIyMTcwOTY3,size_16,color_FFFFFF,t_70#pic_center)

![在这里插入图片描述](https://img-blog.csdnimg.cn/fc60bb53feb94f09b0b32c657ed03ccf.png)

计算步骤：

![在这里插入图片描述](https://img-blog.csdnimg.cn/ffe515b329af4cb1818ed9c6e69bc02d.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzIyMTcwOTY3,size_16,color_FFFFFF,t_70#pic_center)

当我们处理Thinking这个词时，我们需要计算句子中所有词与它的Attention Score，这就像将当前词作为搜索的Query，去和句子中所有词（包含该词本身）的key去匹配，看看相关度有多高。

我们用q1代表Thinking对应的query vector，k1和k2分别代表Thinking以及Machines对应的key vector，则计算Thinking的attention score的时候我们需要计算q1与k1,k2的点乘，同理，我们计算Machines的attention score的时候需要计q2与k1,k2的点乘。

如上图中所示我们分别得到q1与k1,k2的点乘积，然后我们进行尺度缩放与softmax归一化。显然，当前单词与其自身的attention score一般最大，其他单词根据与当前单词重要程度有相应的score。然后我们在用这些attention score与value vector相乘，得到加权的向量。

如果将输入的所有向量合并为矩阵形式，则所有query, key, value向量也可以合并为矩阵形式表示。
![在这里插入图片描述](https://img-blog.csdnimg.cn/1031dea8791d44c783fc52420632ec5b.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzIyMTcwOTY3,size_16,color_FFFFFF,t_70#pic_center)

也就是说， x<sub>1</sub>与 W <sup>Q</sup> 权重矩阵相乘得到 q<sub>1</sub> , 就是与这个单词相关的查询向量。最终使得输入序列的每个单词的创建一个查询向量、一个键向量和一个值向量。
![在这里插入图片描述](https://img-blog.csdnimg.cn/9247c46f768c450aabddaa46e961e19a.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzIyMTcwOTY3,size_16,color_FFFFFF,t_70#pic_center)

化简为矩阵形式：

![在这里插入图片描述](https://img-blog.csdnimg.cn/ed807963154046aabfe68154387c086a.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzIyMTcwOTY3,size_16,color_FFFFFF,t_70#pic-center)

### Attention本质思想

![](https://img-blog.csdnimg.cn/a3e801248e664f0189bdbcc6caff48a5.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzIyMTcwOTY3,size_16,color_FFFFFF,t_70#pic_center)

我们可以这样来看待Attention机制：将Source中的构成元素想象成是由一系列的<Key,Value>数据对构成，此时给定Target中的某个元素Query，通过计算Query和各个Key的相似性或者相关性，再利用softmax函数，得到每个Key对应Value的权重系数，然后对Value进行加权求和，即得到了最终的Attention数值。所以本质上Attention机制是对Source中元素的Value值进行加权求和，而Query和Key用来计算对应Value的权重系数。

### Soft-A和Self-A区别

![image-20220830131222370](https://gitee.com/Acquent0/image-cloud/raw/master/img/image-20220830131222370.png)

### Attention分类

- Soft/Hard Attention
  - soft attention：传统attention，可被嵌入到模型中去进行训练并传播梯度
  - hard attention：不计算所有输出，依据概率对encoder的输出采样，在反向传播时需采用蒙特卡洛进行梯度估计。主要采用了强化学习(RL)的思想，相当于在soft attention的情况下，对这个概率进行采样，即是hard attention的一个样本，输出为one-hot向量，并聚焦于某一点。MC sampling是RL常用的一种方法（如Policy gradient的实现中依赖MC方法来估计梯度)

- Global/Local Attention
  - global attention：传统attention，对所有encoder输出进行计算
  - local attention：介于soft和hard之间，会预测一个位置并选取一个窗口进行计算

- Self Attention

  传统attention是计算Q和K之间的依赖关系，而self attention则分别计算Q和K自身的依赖关系。

### Attention优缺点

优点：

- 一步到位获取全局与局部的联系，不会像RNN网络那样对长期依赖的捕捉会收到序列长度的限制。

- 由固定的中间语义表示C换成了根据当前输出单词并调整成加入注意力模型的变化的 C<sub>i</sub> ，有效解决“信息过长，信息丢失”的问题

- 每步的结果不依赖于上一步，可以做成并行的模式

- 相比CNN与RNN，参数少，模型复杂度低。(根据attention实现方式不同，复杂度不一）

缺点：

Attention的缺点就是没法捕捉位置信息，即没法学习序列中的顺序关系，也就是缺少位置信息，比方说有十个输入词向量，我会将10个输入均等看待，哪个先哪个后，Attention不关心，解决办法就是通过加入位置信息，如通过位置向量来改善，在每一个输入里面再加一个位置向量，告诉模型哪个第一个，哪个第二个等，具体可以参考最近大火的BERT模型。