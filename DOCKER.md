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

 ##### <span id="design_goal">设计目标</span>

> [官方文档](https://www.docker.com/resources/what-container) 
>
>  ![enter description here](./images/设计目标.png)

 ##### <span id="basic_composition">基本组成</span>

>  ![enter description here](./images/docker基本组成.png)
>  - Docker Client：客户端
> - Ddocker Daemon：守护进程
> - Docker Images：镜像
> - Docker Container：容器
> - Docker Registry：镜像仓库
> ![enter description here](./images/docker_CS.jpg)

 ##### <span id="docker_vs_virtual">容器 VS 虚拟机</span>

> [官方文档](https://www.docker.com/resources/what-container) 
> ![enter description here](./images/容器VS虚拟机.png)

|     |  Container    |   VM   |
| --- | --- | --- |
|   启动速度  | 秒级    |  分钟级    |
|    运行性能  | 接近原生    |  5%左右损失   |
|   磁盘占用  |  MB   |   GB	  |
|  隔离性   |  进程级别   |  系统级（更彻底）   |
|  操作系统   |  只支持Linux   | 几乎所有    |
|  封装程度   |  只打包项目代码和依赖关系，共享宿主机内核   | 完整的操作系统    |

 ##### <span id="application_scenario">应用场景</span>

 
 > 1. 应用程序打包和发布
 > 2. 应用程序隔离
 > 3. 持续集成
 > 4. 部署微服务
 > 5. 快速搭建测试环境
 > 6. 提供PaaS产品

