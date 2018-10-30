---
layout: post
title: Docker 概览
tags: tool docker
date: 2018-10-28 10:13:00
postPatterns: 'circuitBoard'
---

Docker 是一个开发,运输和运行应用程序的开源平台. Docker 可以帮你将应用程序从物理硬件(比如电脑)中分离开来, 以快速传送软件. 使用docker, 你可以像管理应用程序一样管理你的物理硬件. 利用 docker 快速运输,测试和部署代码方法的优势, 你可以极大的缩减从编码到产品测试所需时间.

## Docker 平台

Docker 提供一种被称作容器的松隔离环境, 可以在容器中打包和运行应用程序. 其隔离和安全性可以允许你在同一台主机上同时运行多个容器. 容器是轻量级的, 因为他们不需要额外的管理程序负载就可以直接在主机的内核中运行. 这就意味着用在一台硬件设备中可以运行比虚拟机运行更多的容器. 你设置可以在虚拟机中运行docker容器.

Docker 为容器提供工装和一个平台以管理其生命周期.

* 使用容器开发你的应用程序和其所支持的组件
* 容器成为你的应用程序的发布和测试的单元
* 当你准备好时, 将应用程序部署到产品环境中, 如容器或精心策划的服务. 不管你的产品环境是一个本地数据中心, 一个云提供商还是两者混合, 都是一样的.

## Docker 引擎

Docker 引擎是一个客户端-服务器应用程序, 主要有两个组件:

* 一个服务器是一种一直运行的程序, 称为守护进程 (`dockerd` 命令)
* 一个REST API, 其指定端口, 应用程序可以用这些端口跟守护进程进行通信.
* 命令行接口(CLI)客户端(`docker` 命令)

![](https://docs.docker.com/engine/images/engine-components-flow.png)

CLI 通过脚本或直接的CLI命令, 使用 REST API 对守护进程进行控制或交互. 很多其他的 Docker 应用程序使用底层API和CLI.

守护进程创建和管理 Docker 对象, 例如 镜像, 容器, 网络和存储.

> **注意**: Docker 符合开源 Apache 2.0 许可.

更多内容, 见下边的 Docker 架构.

## Docker 可以用来做什么

**快速连贯的发布应用程序**

Docker 允许开发者在一个能够提供给你应用程序和服务的本地容器组成的标准的环境中进行开发, 这样极大的精简了开发的生命周期. 在容器中很容易进行后续集成和后续发布等工作流程.

想象一下这样的场景:

* 开发者在本地的容器中编写代码并且要跟其他同样使用 Docker 容器的同事协同工作.
* 他们使用 Docker 将他们的应用程序推送到一个测试环境中去, 进行自动和手动测试.
* 当开发者发现bug时, 他们可以在开发环境中修复后再重新部署到测试环境中去测试和验证.
* 当测试完成时, 给用户打补丁就跟将更新的镜像推送到生产环境中去一样简单.

**响应式的部署和扩展**

Docker 的基于容器的平台具有非常高的便携性. Docker 容器可以在开发者的本地电脑上工作, 在数据中心的物理或虚拟机器上, 在云服务提供者,或在一个集成的环境中.

Docker 的便携性和轻量级特性使它更容易的根据商务需求接近湿湿的管理工作负载, 扩展或销毁应用和服务.

**在同一个硬件上运行更多的工作**

Docker 具有轻量级和快速的特性. 与基于管理的虚拟机相比,它是一种可行的, 经济有效的替代. 这样你可以使用你的电脑的更多能力来完成工作目标. 在高密度的, 使用更少的资源做中小型的部署的环境中表现出色.

## Docker 架构

Docker 使用客户端服务器架构. Docker 客户端跟 Docker 守护进程进行通信, 守护进程承担了繁重的工作, 运行, 发布你的 Docker 容器. Docker 客户端和守护进行可以在同一个主机上运行, 或者客户端也可以远程连接守护进程. Docker 客户端和服务器使用 `RESET API`, 在 UNIX 套接字或一个网络接口上进行通信.

![](https://docs.docker.com/engine/images/architecture.svg)

### Docker 守护进程

Docker 守护进程 (`dockerd`) 监听 Docker API 请求, 并且管理 Docker 对象, 例如镜像, 容器, 网络和磁盘. 一个守护进程也可以跟其他的守护进程进行通信来管理 Docker 服务. 

### Docker 客户端

Docker 客户端 (`docker`) 是 Docker 用户跟 Docker 进行交互的主要途径. 当你使用命令, 例如`docker run`时, 客户端会将这些命令发送到`dockerd`去实施. Docker 命令使用 Docker API. Docker 客户端可以跟多个守护进程进行通信.

### Docker 登记处

Docker 登记处用来存放 Docker 镜像. Docker Hub 和 Docker Cloud 是任何人都可以使用的公用的登记处, Docker 被配置成默认从 Docker Hub 上寻找镜像. 你也可以运行私有的登记处. 如果你使用 Docker 数据中心(DDC), 可以使用 Docker 信赖登记处(DTR). 

当你使用 `docker pull` 或者 `docker run` 命令时, 会从你配置的登记处里拉需要的镜像. 当你使用 `docker push` 时, 你的镜像会被推到你配置的登记处.

你可以使用 [Docker 商店](http://store.docker.com/) 购买, 销售, 或免费发布镜像. 例如, 你可以从软件提供者哪里购买一个包含一个应用程序或服务的镜像, 并使用这个镜像将这个应用程序部署到你的测试, 评测和生产环境中去. 你可以通过拉这个镜像的新版本来升级这个应用, 重新部署容器.

### Docker 对象

当你使用 Docker 时, 你就创建并使用了镜像, 容器, 网络, 磁盘, 插件等对象. 本段针对这些对象做简要概述.

#### 镜像

镜像是一个具有指令的, 用来创建容器的只读模板. 通常, 一个镜像基于其他的镜像, 增加一些额外的自定义资源. 例如, 你可以搭建一个基于 `Ubuntu` 的镜像, 但是在里边安装了 `Apache 网络服务器` 和你自己的应用及其配置文件.

你可以创建自己的镜像或者只使用那些他人发布在登记处的镜像. 要创建你自己的镜像, 你要创建一个 `Dockerfile`文件, 这个文件可以使用简单的语法定义创建和运行这个镜像的步骤. Dockerfile 中的每一句指令都在镜像中创建一个层. 当你修改 Dockerfile, 重新建立这个镜像, 只有那些被修改的层会被重新建立. 这就是跟其他的虚拟化技术相比, 镜像这么轻量, 小巧, 和快速的原因.

#### 容器

容器是对象的一个可运行的实例化. 你可以使用 Docker API 或 CLI 创建, 开始, 结束, 移动或删除容器, 可以将一个容器连接多个网路, 给他添加存储, 甚至可以根据他当前的状态创建一个新镜像. 

默认情况下, 一个容器跟其他容器和他所在的主机很好的隔离开的. 你可以控制一个容器的网络, 存储, 或其他底层子系统于其他容器或主机的隔离程度.

当你创建或启动容器时, 容器是由他的镜像和一系列的配置选项来定义的. 当你删除它时, 他当前状态中没有存储到永久存储设备上的所有修改都会被删除.

**`docker run` 命令**

下边命令启动一个 `ubuntu` 容器, 交互式依附到本地的命令行会话中, 运行 `/bin/bash`.

```
$ docker run -i -t ubuntu /bin/bash
```

当你运行这个指令时, 会发生如下的过程 (假设你使用默认的登记处):

1. 如果你本地没有 `ubuntu` 镜像, Docker 会从你配置的登记处下载, 就像你手动运行过 `docker pull ubuntu`.
2. Docker 创建你个新的容器, 就像你手动运行过 `docker container create`.
3. Docker 为这个容器分配一个可读写的文件系统作为最后一层. 这样允许一个运行的容器可以创建或修改他本地文件系统中的文件和目录.
4. Docker 创建一个网络接口, 如果你没有指定网络选项, 它将这个容器连接到默认的网络中. 这包含为这个容器分配一个IP地址. 默认情况下, 容器可以利用主机的网络连接去连接外网.
5. Docker 启动这个容器并执行 `/bin/bash`. 因为这个容器是交互式运行并且依附到你的终端上(因为 `-i` 和 `-t` 选项), 你可以使用键盘给容器输入, 同时他的输出也会显示在终端上.
6. 当你输入 `exit` 命令结束 `/bin/bash` 时, 这个容器会停止运行, 但是没有被删除. 你可以再次启动或删除它.

#### 服务

服务可以帮你将你的容器扩展到多个 Docker 守护进程上, 这些守护进程构成了一个由多个管理者和工作者的集群. 这个集群中的每一个成员都是一个守护进程, 并且他们都使用 Docker API 进行通讯. 你可以使用服务定义想要的状态, 例如在某一个时刻必须可用的服务副本的数量. 默认情况下, 服务在多个工作者节点上是负载均衡的. 对用户而言, 服务就像一个应用程序一样. 在 Docker 1.12或更高版本中, Docker 引擎支持集群模式.

## 底层技术

Docker 是使用 [Go](https://golang.org/) 语言编写的, 并且利用了很多 Linux 内核的优点来实现其功能.

###　名字空间

Docker 使用一种被成为名字空间的技术来提供隔离的工作区，这就是容器. 当你运行一个容器, Docker 就会为这个容器创建一系列的名字空间.

这些名字空间提供一个隔离层. 容器的每一个点都运行在独立的名字空间中, 并且只能由本名字空间访问.

Docker 引擎使用名字空间, 在Linux中, 如下:

* **`pid` 名字空间**: 进程隔离(PID: Process ID)
* **`net` 名字空间**: 管理网络接口(NET: Networking)
* **`ipc` 名字空间**: 管理进程间通信资源访问(IPC: InterProcess Communication)
* **`mnt` 名字空间**: 管理文件系统挂载点(MNT: Mount)
* **`uts` 名字空间**: 隔离内核和版本识别码(UTS: Unix Timesharing System)

### 控制组

Linux 上的 Docker 引擎还依赖另外一种称为*控制组*(`cgroup`)的技术. 控制组将应用限制到指定的资源上. 控制组允许 Docker 引擎给容器分享可用的硬件资源, 非强制的执行限制和约束. 例如, 你可以限制分配给指定容器的内存.

### 联合文件系统

联合文件系统(UnionFS), 通过创建层来操作, 使之轻量化和快速化. Docker 引擎使用联合文件系统为容器提供建造模块. Docker 引擎可以使用多个联合文件系统变体, 包括 AUFS, btrfs, vfs和 DeviceMapper.

### 容器格式

Docker 引擎将名字空间, 控制组和联合文件系统打包到一起成为容器格式. 默认的容器格式是`libcontainer`. 未来, Docker 可能通过集成BSD Jails 或 Solaris Zones 等技术支持其他的容器格式.