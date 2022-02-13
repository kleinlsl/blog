---
title: Google Colab 免费GPU算力
date: 2020-04-20 12:14:16

---
﻿@[TOC](https://blog.csdn.net/qq_41971768/article/details/105639794)

# 1、前言

> 最近在做一些需要GPU算力的项目，自己的电脑的算力不够用。找到了 [Google Colab（Colaboratory）](https://research.google.com/colaboratory/unregistered.html) ，作为Google推出的免费的云端GPU服务。
>
> 官方对其的说明是：
>
> > Colaboratory 是一个研究项目，可免费使用。

> <!-- more -->

# 2、Google Colab特征
- Colaboratory 是一个 Google 研究项目，旨在帮助传播机器学习培训和研究成果。它是一个 Jupyter 笔记本环境，不需要进行任何设置(自带大部分python库)就可以使用，并且完全在云端运行。
- Colaboratory 笔记本存储在 Google 云端硬盘中，并且可以共享，就如同您使用 Google 文档或表格一样。Colaboratory 可免费使用。
- 利用Colaboratory ，可以方便的使用Keras、TensorFlow、PyTorch等框架进行深度学习应用的开发。



# 3、使用

> 注意：使用google服务可能需要梯子

## 3.1在谷歌云盘上创建文件夹

当登录账号进 入[谷歌云盘](https://drive.google.com/drive/my-drive)时 ，系统会给予15G免费空间大小。由于Colab需要依靠谷歌云盘，故需要在云盘上新建一个文件夹。

选择新建文件夹，文件夹名称可自定义。

## 3.2创建Colaboratory

进入创建好的文件夹，点开我的云盘-更多。
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200420172221824.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQxOTcxNzY4,size_16,color_FFFFFF,t_70)

如果在更多栏里没有发现Colaboratory，选择关联更多应用，搜索Colaboratory，选择关联。

## 3.3创建完成

创建完成后，会自动生成一个jupyter笔记本

# 4、设置GPU运行

- 选择 修改-笔记本设置
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200420172504794.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQxOTcxNzY4,size_16,color_FFFFFF,t_70)
- 将硬件加速器设置为GPU即可

# 5、运行.py文件

## 5.1安装必要库

输入相应代码，并执行（crtl+F9）

```
!apt-get install -y -qq software-properties-common python-software-properties module-init-tools
!add-apt-repository -y ppa:alessandro-strada/ppa 2>&1 > /dev/null
!apt-get update -qq 2>&1 > /dev/null
!apt-get -y install -qq google-drive-ocamlfuse fuse
from google.colab import auth
auth.authenticate_user()
from oauth2client.client import GoogleCredentials
creds = GoogleCredentials.get_application_default()
import getpass
!google-drive-ocamlfuse -headless -id={creds.client_id} -secret={creds.client_secret} < /dev/null 2>&1 | grep URL
vcode = getpass.getpass()
!echo {vcode} | google-drive-ocamlfuse -headless -id={creds.client_id} -secret={creds.client_secret}

```



> 运行后，先点开相应的链接，选择自己的谷歌账号，并允许，最后会得到相应的代码，输入相应的框中即可
> 
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200420172555986.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQxOTcxNzY4,size_16,color_FFFFFF,t_70)
## 5.2 挂载云端硬盘

同上，输入下面命令，执行即可

~~~
!mkdir -p drive
!google-drive-ocamlfuse drive  -o nonempty
~~~



## 5.3 安装Keras
同理，输入命令

~~~ 
!pip install -q keras
~~~



# 6、训练自己的代码

### 6.1上传项目代码到Google 云盘

### 6.2 将云盘代码拷贝至分配的虚拟目录

~~~
!cp -r dir1 dir2  #将目录dir1 拷贝至 目录dir2下
~~~

### 6.3 使用python命令运行py脚本

~~~
!python main.py
~~~

### 6.4 注意事项

- Google 训练自己的代码，需要修改代码中有关路径的部分为绝对路径
- Google训练过程中不可关闭网页，不然会断开连接导致训练中断

## 参考文献

[https://medium.com/deep-learning-turkey/google-colab-free-gpu-tutorial-e113627b9f5d](https://medium.com/deep-learning-turkey/google-colab-free-gpu-tutorial-e113627b9f5d)

[Google Colab 免费GPU服务器使用教程](https://blog.csdn.net/cocoaqin/article/details/79184540?depth_1-utm_source=distribute.wap_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-1&utm_source=distribute.wap_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-1#51%E5%AE%89%E8%A3%85%E5%BF%85%E8%A6%81%E5%BA%93)
