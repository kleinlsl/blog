---
title: Intellij  连接远程服务器代码实时同步                    #文章页面上的显示名称，一般是中文
date: 2020-10-5 8:14:16 #文章生成时间，一般不改，当然也可以任意修改
tags: [Idea,Ubuntu,Vmware]
copyright: true
comments: false
description: Intellij  连接远程服务器代码实时同步
top: 
photos: 
	- "/images/Title-photos/IMG_20200917_160921.jpg"
---



## 前言

使用IDEA 或 Pycharm等工具实现，连接远程服务器并实时同步代码。

> 前提条件
>
> * Intellij 产品需为专业版
> * 远程主机需能够远程登录



> 本文环境
>
> * 主机：windows 10 home
> * 远程服务器：Ubuntu 18.4 servers
> * 虚拟机：Vmware 15.5 pro
> * 其他软件：SecureCRT、Pycharm 2019.2(Professional Edition)



<!--more-->

## 配置步骤

1、打开SecureCRT确保可以连接至服务器,如图:

![](/images/blog-images/images-Intellij/测试SecureCRT.png)

2、打开Pycharm 进行配置

*  Pycharm打开一下ssh session测试

  > 选择：tools-->Start SSH session
  >
  > ![](/images/blog-images/images-Intellij/pycharm-ssh.png)
  >
  > 能看到在isdea的下方开启了一个终端 
  >
  > ![](/images/blog-images/images-Intellij/ssh-成功.png)

* 打开项目，选择：tools->deployment->brower remote host ，如图：

  ![](/images/blog-images/images-Intellij/选择示意图.png)

* 新建连接，如图：

  ![](/images/blog-images/images-Intellij/打开配置示意图.png)

* 填写服务器信息，如下图：

  ![](/images/blog-images/images-Intellij/配置详情.png)

* 一定要配置Mappings,如下图：

  ![](/images/blog-images/images-Intellij/addMappings.png)

* 更新服务器文件：

  ![](/images/blog-images/images-Intellij/更新服务器文件.png)

* 配置自动更新:

  ![](/images/blog-images/images-Intellij/配置自动更新.png)

## 配置效果展示

![](/images/blog-images/images-Intellij/效果展示.png)

## 远程运行服务器代码

> 打开：file-> Settings -> project Interpreters -> SSH Interpreter

![](/images/blog-images/images-Intellij/2020102801.png)

![](/images/blog-images/images-Intellij/2020102802.png)

![](/images/blog-images/images-Intellij/2020102803.png)

![](/images/blog-images/images-Intellij/2020102804.png)

![](/images/blog-images/images-Intellij/2020102805.png)

![](/images/blog-images/images-Intellij/2020102806.png)