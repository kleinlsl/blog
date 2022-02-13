---
title: 使用百度 Ai Studio GPU算力
date: 2020-04-20 22:14:16

---
@[TOC](https://blog.csdn.net/qq_41971768/article/details/105645906)

# 前言

> 继上次找到Google Colab 免费GPU之后，又发现了[百度AI studio](https://aistudio.baidu.com/aistudio/usercenter)云平台的GPU算力。
>
> 百度的是相当优惠：每日使用gpu就可以获得12小时的算力卡，连续五天还可以额外获得48小时算力卡。
>
> 搭载的是 飞桨PaddlePaddle 框架，目前并不支持tensorflow-gpu。但是自己可以通过以下配置来使用tensorflow-gpu.

> <!-- more -->

## 1、选择版本

> 其实AI Studio我们可以理解为一台免费的云服务器，这样我们就可以在里面配置相应的环境，就和自己本机配置tensorflow环境基本一致。



>  安装之前注意自己的python版本，tensorflow版本，cuda版本，cudnn版本，一定要相互匹配 
>
> 可以参考以下版本：

~~~
python 3.6
tensorflow 1.12.0
cuda 9.0
cudnn 7.4.1
Linux 16.04 (ai studio系统版本)
~~~

## 2、下载配置文件

~~~
1 在ai studio或者notebook下载cuda
!wget https://developer.nvidia.com/compute/cuda/9.0/Prod/local_installers/cuda_9.0.176_384.81_linux-run
 
2 新建一个目录~/cuda-9.0/
 
3 将下载的cuda安装到上述新建的目录中
!sh cuda_9.0.176_linux-run --silent --toolkit --toolkitpath=$HOME/cuda-9.0
 
4 下载cudnn，注意这个需要去官网注册账号，事先下载到自己的电脑然后，
  注意版本要对，然后通过新建数据集上传到ai studio的data/目录下，名字尽量短，下载之后是一个tgz格式   的文件，我把名字改为cudnn-9.0.tgz
 
5 解压4步下载的文件到根目录,解压之后的cudnn文件名默认为cuda，
!tar -zxvf /home/aistudio/data/data25688/cudnn-9.0.tgz 
 
6 解压把cudnn的指定文件copy到cuda安装文件对应的目录中注意目录要对，这一步只需要做一次就可以
!cp cuda/include/cudnn.h cuda-9.0/include/
!cp cuda/lib64/libcudnn* cuda-9.0/lib64/
 
至此准备工作完成，这些工作只需要一次就可以，接下来进行环境配置阶段
~~~

## 3、环境配置

- 新建虚拟环境

  ~~~
  1 在当前目录新建一个文件命名为envm,运行一下脚本，注意文件名即可
  !echo 'export PATH=$HOME/cuda-9.0/bin:$PATH\nexport LD_LIBRARY_PATH=$LD_LIBRARY_PATH:$HOME/cuda-9.0/lib64' > ~/envm
  (修正)：此处使用以上路径会导致无法使用 ls 命令，应在envm文件更改为以下内容：
  export PATH=$HOME/cuda-9.0/bin${PATH:+:${PATH}} 
  export LD_LIBRARY_PATH=$HOME/cuda-9.0/lib64${LD_LIBRARY_PATH:+:${LD_LIBRARY_PATH}}
   
  2 在终端新建虚拟环境,这里选择与tensorflow版本匹配的python版本
  依次,这个需要每次都重新做，我还不知道怎么把这个放到一个shell脚本中，所以只能分开了
  conda create -n env_name python=3.6
  安装过程选择yes输入y
  source activate env_name
  ~~~

  

- 激活环境

  ~~~
   
  2 新建脚本命名为chmod_cuda90.sh,加入以下脚本，注意你自己的目录
  #!/bin/bash
  chmod a+r ~/cuda-9.0/include/cudnn.h
  chmod a+r ~/cuda-9.0/lib64/libcudnn*
  source ~/envm
  pip install -i https://pypi.tuna.tsinghua.edu.cn/simple tensorflow-gpu==1.12.0
   
  3 在终端进入自己的虚拟环境运行上述脚本
  source chmod_cuda90.sh就可以使用gpu进行加速了
  ~~~

  

## 4、使用tensorflow-gpu

-  每次重启环境只需要运行以下脚本 

  ~~~
  conda create -n env_name python=3.6
  source activate env_name
  source chmod_cuda90.sh
  ~~~

  

# 参考资料

[百度AI studio配置tensorflow环境]( https://blog.csdn.net/firesolider/article/details/105023062?depth_1-utm_source=distribute.wap_relevant.none-task-blog-BlogCommendFromBaidu-8&utm_source=distribute.wap_relevant.none-task-blog-BlogCommendFromBaidu-8 )
[白嫖百度AIstudio免费GPU](https://blog.csdn.net/qq_36666756/article/details/105252530#comments)
