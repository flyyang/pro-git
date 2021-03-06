# 起步

本章介绍 Git 的起步知识。我们从版本控制工具的一些历史背景开始，然后到如何把 Git
在你的本地系统运行起来，最后配置好一个可以使用的 Git 环境。在本章的末尾，你将会
理解：为什么 Git 会存在， 为什么你需要使用它，以及为什么你需要尽力去掌握它。

## 关于版本控制

什么是“版本控制”，为什么你需要关注它？版本控制是一个可以追踪一个或多个文件变化
，并且可以随时切换到某一个变化版本的系统。以本书为例，书中的源码是被版本控制的，
但版本控制实际上可以是计算机上任何类型的文件。

如果你是一个图形或者 web 设计师，想要保留每一个版本的图片或布局的变化（通常你都
想这么做），一个好用的版本控制系统（VCS）是一个明智的选择。它允许你讲文件恢复到
之前的某一个状态，也允许你讲整个项目恢复到某个状态，可以比较不同版本的变化，查
看谁最后修改了某个东西可能是造成问题的罪魁祸首，谁，在何时引入了问题，等等。使
用版本控制系统，通常意味着，如果你把文件搞坏了，或者丢失了，你可以轻松的恢复出
来。值得一提的是，达成这么多目标，通常很简单。

## 本地版本控制系统

大部分人所谓的版本控制通常是拷贝文件到其他目录（带上日期标记是更聪明的做法）。
因为简单易行，这种做法非常常见，但也容易出错。比如，经常忘记自己在哪个文件夹
下，一不小心写错了文件，或者拷贝错了文件。

为了解决这些问题，程序员开发了本地版本控制系统。版本控制系统通常拥有一个简单的
数据库，追踪所有的文件变化。

图1-1. 本地版本控制。

其中一个比较出名的系统叫做 RCS，目前仍在许多操作系统中使用。甚至在非常流行的Mac
OSX 操作系统上，如果你安装开发人员工具的话， 一个 rcs 命令会跟随安装。RCS 的原
理是在硬盘上保存一个特殊格式的 patch 集（即，文件之间的不同部分），通过打 patch
的形式来切换不同版本的文件。

## 中心版本控制系统

接下来还有一个重要的问题，如何与其他系统中的人进行合作。为了解决这个问题，人们
开发出了中心版本控系统（CVCSs）。这些系统，如 CVS, Subversion, 以及Perforce, 都
是由同一个服务端包含所有的版本文件，多个客户端从服务端签出文件。中心版本控制系
统成为版本控制的标准，持续了很多年。

中心版本控制系统包含许多优点，尤其与本地版本控制系统相比较。举例来说，每一个人都
在某些程度上知道同一项目中其他人在做什么。管理者容做权限控制。管理相对容易，只
在中心系统中管理，比分别在不同的客户端处理数据库要简单很多。

## 分布式版本控制系统

分布式管理系统（DVCSs）接踵而至。 DVCS(如 Git, Mercurial, Bazaar 或者 Darcs),
客户端不仅仅只签出最新文件快照 —— 它们会镜像整个项目。因此，如果任何一个用来
提交代码的 Server挂掉，可以直接用客户端的项目直接覆盖服务端来恢复。实际上，客户
端的每一个签出都是所有数据的全量备份。

另外，所有的这些系统都很方便的支持多个远程项目协作，因此在同一个项目下，你可以
和不同组的人同时工作。允许多种类型的工作流，在中心版本控制系统中是不可能的，如
继承模型。

## Git 简史

和许多伟大的事情一样，Git 来源于

Linux 内核是一个非常大的开源软件项目。Linux 内核维护的的大部分时间
（1991-2002），修改通常以patch 或者文件存档的形式传递。从2002年开始，Linux 内核
项目开始使用一个带有专利的分布式版本控制系统 BitKeeper。

2005年， Liunx 内核研发社区和开发 BitKeeper 的公司分道扬镳，BitKeeper 不再免费。
这导致 Linux  社区（尤其是 Linus Torvalds, Linux 创建者）开发急需开发一款自己的
版本控制工具。基于使用 BitKeeper 过程中的经验教训，新系统设立的目标为：

* 快速
* 设计简单
* 重点支持非线性开发（上千个并行分支）
* 完全的分布式
* 高效（速度，大小）处理大项目，如 Linux 内核

自从2005年创立开始， Git 发展成熟为一个易于使用但又保持原有的质量的分布式版本控
制系统。它快速，处理大项目高效，以及为非线性开发准备的及令人难以置信的分支系统。

## Git 基础

所以，如何简单的概述 Git？这部分很重要，因为如果你理解了什么是 Git, 以及
Git 大体上是如何工作的，那么高效利用 Git 对你来说会简单不少。随着你不断的学习
Git, 不断的尝试清除从其他版本控制系统中学的知识， 如 Subversion 和 Perforce,
 会帮助你排除一些微妙的疑虑。Git 存储以及对待信息的方式和其他管理系统不同，尽管
用户界面非常接近，了解这些不同会帮助你消除使用过程中的疑虑。

## 快照，不是 diff

Git 和其他 VCS（Subversion 等） 的主要不同在于 Git 如何看待数据。从概念上来说，
大部分系统存储文件变动的信息。 这些系统（CVS, Subversion, Perforce, Bazaar 等）
看待信息


