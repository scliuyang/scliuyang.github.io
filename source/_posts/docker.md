title: 初识docker
date: 2016-12-21 16:32:00
categories: scliuyang
tags:
- docker

---
docker经过这么几年的发展，到如今更是火热到即使你没吃过猪也见过猪跑的地步,本节会给大家介绍docker的一些含义，让大家了解docker

<!--more-->

# 什么是Docker

Docker是一个开放源代码软件项目，让应用程序布署在软件容器下的工作可以自动化进行，借此在Linux操作系统上，提供一个额外的软件抽象层，以及操作系统层虚拟化的自动管理机制。Docker利用Linux核心中的资源分离机制，例如cgroups，以及Linux核心命名空间（name space），来建立独立的软件容器（containers）。这可以在单一Linux实体下运作，避免启动一个虚拟机器造成的额外负担。

——摘自维基百科

# Docker到底牛逼在哪里

docker的中文含义`码头工人`，这个翻译初看会觉得很怪异,其实官方取名这个名字有包含隐喻的意思在内.

我们再看看docker的logo，一头像船的鲸鱼上面装载着许许多多的箱子，结合`码头工人`的意思，我们其实可以想到`集装箱`这个概念，`集装箱`是全球物流系统中一个非常重要的发明，他带来了物流的规范化，统一化，极大的节约了人力物理和时间成本。
<img src="http://i1.piimg.com/4851/5356edebd8c82056.png" style="width:200px;"></img>

## 集装箱的作用

  我们可以想想在集装箱这个定义出来之前的货物运输：货物一箱箱的搬上货车送到附近的火车站，然后一箱箱卸下，再一箱箱的搬上火车运送到附近的码头...
  上述的过程中不难发现大量的人力和时间成本都花费在一箱箱的搬运上面，在运输速度一定的情况下，装卸就成为了物流的瓶颈。
  在`集装箱`出现后这个问题得到了极大的改善。集装箱重要在它提供了一种通用的封装货物的标准规格（尺寸，外形符合统一标准），这样就产生了一个巨大的优点：在物流运输中只需要在运输前一次封装，集装箱就可以放上火车，卡车，拉到码头，直接放在货船上；卸船之后直接再放上火车，卡车，运送到目的地。而且由于集装箱符合统一标准，整个流程非常容易机械化，这引发了以集装箱为中心的整个全球物流的标准化进程，从而节省了大量的时间资源和人力资源，成本迅速下降，促进了全球资源的流动与重新配置。

## Docker与集装箱

docker就像码头的工人一样，把应用打包为一个个封装好的标准集装箱，就是大家口中经常镜像文件。那docker为业内带来了什么呢？

### docker诞生以前，运维的蛮荒时代

  以前我们搭建一个网站，你可能会装PHP,Mysql，Apache等等一堆软件，好花费大半天的时间这个网站能正常运行起来了。

  一段时间后我们需要更高的PHP版本来搭建一个其他类型的网站，版本冲突了咋整？一番google后，费了九牛二虎之力两个网站终于能共同运行了。
老板后来告诉你由于网站太火爆了我们需要换一台服务器迁移过去，于是你又不得不巴拉巴拉的折腾一番将所有的环境重新配置一遍。

  老板小手一拍，我们要优化我们的服务，在全国各地都建立节点提供服务，你掰一掰的你手指头发现有数不清的环境需要搭建，你的内心是崩溃的。

### docker出现后，我们步入了工业时代

就和`集装箱`一样，docker为我们带来的是标准，具体一点说就是镜像。在docker当中镜像其实就是你把一大包工具打包成一个集装箱交于docker运行，镜像与镜像之间互不影响(集装箱与集装箱之间也是互不影响运输)。

在docker中，镜像是无法直接运行的，我猜想这并不是技术上的原因，而是工程设计上的原因。因为一般来说，一个软件的某个具体版本只会打包成一个镜像。如果镜像可以配置，运行的话，在使用过程中很可能会对镜像造成破坏。

那怎么样避免这个问题呢，就是再加一层，也就是相当于用了分身术，只要本尊没问题，分身怎么扑街都不会真正的跪掉。多加的这一层分身，就叫容器（container），这个名字也挺形象，它就像个盒子一样，你的应用在里面运行，而且多了一层安全机制。你想使用服务或把你的应用跑起来的话，只需要使用镜像新创建一个容器就可以了（也是一条命令搞定），而镜像还放在那里不动，没办法，金贵嘛。

### docker究竟做了什么？
docker正是在部署过程中，将上面那些重复的部分，由docker自动化完成。只需要在第一次部署时，构建完可用的docker镜像。然后在以后使用的过程中，短短的几行命令，就可以直接拉取镜像，根据这个镜像创建出一个容器，把服务跑起来了。所需要的仅仅是安装了docker的服务器，一个Dockerfile文件，以及比较流畅的网络而已。真可谓`一次构建，到处部署`。
- 需要nginx,直接pull nginx镜像完事
- 迁移服务器？直接下载一个非常小的Dockerfile，安装一个docker环境即可，简单的不能在简单
- 多个版本共存？新建一个镜像，爱用哪个版本用哪个，容器的隔离性让我们就是这么任性

到这个地方，你可能已经发现了，docker镜像成为了一种像集装箱那样的标准货件。它不像传统的软件交付方式那样，只把代码以及说明文档之类的给你就完了，而是直接给你一个标准docker货件，它可能是Dockerfile，或者直接就是镜像，这个标准件不仅包括了代码本身，还包括了代码运行的OS等各种整体环境。

于是，谁想用我的服务，直接拉取镜像，实例化一个容器就可以了，能直接提供你所要的服务，不再像之前那样有繁复的安装过程————这些都有人给你做过了。

# 与传统虚拟机对比

有人可能会说，这些不是虚拟机都做到了么，还要docker干嘛？

传统虚拟机就像一个老爷爷，走路晃晃悠悠，还需要分配资源，给予照顾才行。反观docker则像是一个健步如飞的年轻小伙子，干啥事都是一个字`快`.

- 容器不需要进行硬件虚拟以及运行完整操作系统等额外开销，Docker 对系统资源的利用率更高。无论是应用执行速度、内存损耗或者文件存储速度，都要比传统虚拟机技术更高效。因此，相比虚拟机技术，一个相同配置的主机，往往可以运行更多数量的应用。
- 传统的虚拟机技术启动应用服务往往需要数分钟，而 Docker 容器应用，由于直接运行于宿主内核，无需启动完整的操作系统，因此可以做到秒级、甚至毫秒级的启动时间。大大的节约了开发、测试、部署的时间。
- Docker 使用的分层存储以及镜像的技术，使得应用重复部分的复用更为容易，也使得应用的维护更新更加简单，基于基础镜像进一步扩展镜像也变得非常简单。此外，Docker 团队同各个开源项目团队一起维护了一大批高质量的官方镜像，既可以直接在生产环境使用，又可以作为基础进一步定制，大大的降低了应用服务的镜像制作成本。

下图是docker与传统虚拟机性能对比，图片来自 [Docker — 从入门到实践](https://yeasy.gitbooks.io/docker_practice/content/)
<img src="http://p1.bqimg.com/4851/12e0188ee1b32356.png"></img>

# 下一节预告

下一节会给大家来带一些docker常用的使用命令介绍