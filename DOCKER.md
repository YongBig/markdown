---
title: DOCKER 
tags: docker,容器,容器云
grammar_cjkRuby: true
---

# 概要

**掌握程度**

 - 掌握Docker核心概念
-  熟悉Docker工作原理
-  独立使用Docker部署应用程序
-  接入CI/CD，实现环境标准化


**目录**

 1. [概述](#summary)
	 - [什么是DOCKER](#what_docekr)
 	 - [设计目标](#design_goal)
	 - [基本组成](#basic_composition)
	 - [容器 VS 虚拟机](#docker_vs_virtual)
	 - [应用场景](#application_scenario)
	
	
	
#### <span id="summary">一 、 概述</span>

##### <span id="what_docekr">什么是DOCKER？</span>


> [官方文档](https://www.docker.com/resources/what-container) 
>
>  - 使用最广泛的开源容器引擎
>  - 一种操作系统级的虚拟化技术
>  - 依赖于Linux内核特性：Namespace（资源隔离）和Cgroups（资源限制）
>   - 一个简单的应用程序打包工具

 ##### <span id="design_goal">设计目标？</span>

.> [官方文档](https://www.docker.com/resources/what-container) 
>
>  ![enter description here](./images/设计目标.png)

 ##### <span id="docker_vs_virtual">基本组成</span>

>  ![enter description here](./images/docker基本组成.png)
>  - Docker Client：客户端
> - Ddocker Daemon：守护进程
> - Docker Images：镜像
> - Docker Container：容器
> - Docker Registry：镜像仓库
> ![enter description here](./images/docker_CS.jpg)

 ##### <span id="basic_composition">容器 VS 虚拟机</span>

>  ![enter description here](./images/docker基本组成.png)
>  - Docker Client：客户端
> - Ddocker Daemon：守护进程
> - Docker Images：镜像
> - Docker Container：容器
> - Docker Registry：镜像仓库
> ![enter description here](./images/docker_CS.jpg)

