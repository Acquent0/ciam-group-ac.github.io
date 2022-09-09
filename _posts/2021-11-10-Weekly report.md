---
title: Weekly Report
date: 2022-08-30 
comments: true
categories:
- cascade
tags:
- plan
description: to note the everyweek work.
---

#  Weekly Report

8.29-9.4

8.29-8.30在网上查找资料学习Attention、Transformers、CVRP问题的基本概念、原理，记录笔记。

8.31尝试在服务器运行Attention代码

- 配置服务器，一开始有点搞不懂，目前有点头绪，开始配置环境。希望可以运行
- 晚饭后服务器配置完毕，算是遇到了一些小坑：
  - tensorboard库安装需要tensorflow作为前置，之前一直以为tensorflow和pytorch是两个相对的大型库，现在看起来并不完全正确；
  - 服务器的配置在师姐的帮助下比较顺利；
  - Attention的代码刚拿到手上时不知道怎么看，询问了师兄师姐后，有了一些头绪，大致是先从readme或其他说明文件入手，然后再细看run.py或main.py，有需要再看model文件。
  - 对于读论文：先看abstract，然后看introduction，了解内容后，知道论文的分类和方向，要解决的问题，以及采用的大致方法，再从插图入手看方法。最后看implementation==为啥来着？==

9.1忙于安排课表等一些杂事，代码没怎么看

9.2继续看Attention代码，之前在服务器上跑的结果也出来了，但因为epoch设置的太大，且当时不知道可以后台跑代码，因此迭代次数只有五十多次，效果也不行：

![image-20220902160800296](https://gitee.com/Acquent0/image-cloud/raw/master/img/image-20220902160800296.png)

![image-20220902203313245](https://gitee.com/Acquent0/image-cloud/raw/master/img/image-20220902203313245.png)

![image-20220902203320906](https://gitee.com/Acquent0/image-cloud/raw/master/img/image-20220902203320906.png)

![image-20220902203328658](https://gitee.com/Acquent0/image-cloud/raw/master/img/image-20220902203328658.png)

跑的结果还没收敛。

==什么是端到端==

Q：

![image-20220902213712867](https://gitee.com/Acquent0/image-cloud/raw/master/img/image-20220902213712867.png)

对这个参数的内容不是很懂

看了一晚上代码 大概能看懂它的运行结构和参数设置，但它的核心model代码似乎写的比较

