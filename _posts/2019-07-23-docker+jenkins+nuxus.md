---
layout:     post
title:      docker+jenkins+nexus
subtitle:   打包到镜像到仓库
date:       2019-07-23
author:     BY LSD
header-img: img/post-bg-swift.jpg
catalog: true
tags:
    - docker
    - jenkins
    - nexus
---
##### 安装Docker Compose
使用Dockerfile模板文件，很方便去定义一个单独容器。但当多个容器需要相互依赖或者配合的时候，之前的已经不能够满足
需求。这个时候就需要将这些容器放在一起，组成一个服务，再将一个个服务变成一个项目，这个时候就需要compose进行编
排，满足需求。
具体安装过程可以参考[Docker三剑客之Compose](https://blog.51cto.com/bovin/2286718),比较详细，这里就不一一阐述了。

##### 安装jenkins
jenkins是一款非常优秀的java开源项目，可以持续集成，持续交付，持续部署。jenkins的插件丰富，1000多个，可以满足众多需求，构建伟大，无所不能。文档可以参考官方[Jenkins](https://jenkins.io/zh/doc/),非常详细。
这里推荐一个五分钟安装jenkins教程，简单，没有之一。[简书五分钟完成Linux服务器上jenkins的环境搭建](https://www.jianshu.com/p/42f1e4323dfc)

##### 安装Sonatype Nexus
nexus是私服经常选择的仓库，对于团队开发，特别是局域网开发，体现的作用更大。
简介和安装，参考[Maven私服-Sonatype Nexus](https://www.jianshu.com/p/bae0ef064749),
[Maven和 Sonatype Nexus私服的安装、配置及使用入门](https://blog.csdn.net/wuxiaobingandbob/article/details/79396239)

##### 具体流程
jenkins项目的创建
gitlab项目配置
shell执行脚本

compose编排镜像