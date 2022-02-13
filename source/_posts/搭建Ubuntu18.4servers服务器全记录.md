---
title: Ubuntu18.04 Server搭建和使用记录                    #文章页面上的显示名称，一般是中文
date: 2020-10-3 19:14:16 #文章生成时间，一般不改，当然也可以任意修改
top: true
---



> 记录自己搭建Ubuntu18.04 Server的全部过程，有不足之处请见谅！！！

## 1、前置工作

本文做出以下假设：

* 您已熟悉Linux操作系统
* 您拥有(或能够独立创建)一台Ubuntu Server

本文采用Vmware 虚拟机创建Ubuntu 18.04 Server

* 镜像获取：[Ubuntu 18.04.5 LTS (Bionic Beaver)](http://cdimage.ubuntu.com/ubuntu/releases/18.04/release/?_ga=2.128866185.407240186.1601692120-588330145.1601692120)
* Vmware：[VMware15.5](https://www.vmware.com/cn/products/workstation-pro/workstation-pro-evaluation.html)
* SecureCRT：[SecureCRT](https://www.vandyke.com/download/securecrt/6.7/index.html)

搭建Ubuntu18.04 Server 教程：[VMware 安装 Ubuntu Server 18.04.5 LTS](https://blog.csdn.net/qq_41971768/article/details/108908496)

> <!--more-->

## 2、Server准备工作

* 设置root密码：`sudo passwd root`

* 更改镜像源：

  * 备份源配置文件： ` sudo cp /etc/apt/sources.list /etc/apt/sources.list.bak `

  * 在源配置文件前添加阿里云镜像源配置 : ` sudo vim /etc/apt/sources.list `

    ```shell
    deb http://mirrors.aliyun.com/ubuntu/ bionic main restricted universe multiverse
    deb-src http://mirrors.aliyun.com/ubuntu/ bionic main restricted universe multiverse
     
    deb http://mirrors.aliyun.com/ubuntu/ bionic-security main restricted universe multiverse
    deb-src http://mirrors.aliyun.com/ubuntu/ bionic-security main restricted universe multiverse
     
    deb http://mirrors.aliyun.com/ubuntu/ bionic-updates main restricted universe multiverse
    deb-src http://mirrors.aliyun.com/ubuntu/ bionic-updates main restricted universe multiverse
     
    deb http://mirrors.aliyun.com/ubuntu/ bionic-proposed main restricted universe multiverse
    deb-src http://mirrors.aliyun.com/ubuntu/ bionic-proposed main restricted universe multiverse
     
    deb http://mirrors.aliyun.com/ubuntu/ bionic-backports main restricted universe multiverse
    deb-src http://mirrors.aliyun.com/ubuntu/ bionic-backports main restricted universe multiverse
    ```

    

  * 执行更新命令 : `sudo apt-get clean`  、`sudo apt-get update`

  * 查看Java、Python、MySql、Tomcat、nginx、SqlServer环境

  * Java: `java -version`
  
  * Python: `python 或 python3`
  
  * MySql：`mysql -version`
  
  * Redis： `redis-server -v`  
  
  * Tomcat：{待补充}
  
  * Docker：`docker -v`
  
  * nginx：{待补充}
  
  * SqlServer：{待补充}
  
    

### 2.0 安装文件上传工具

```
安装命令：apt-get install lrzsz -y

使用命令：rz # 上传

```

### 2.1 安装Mysql环境

* 安装Mysql：

  ```shell
  apt-get install mysql-server
  apt-get install mysql-client
  ```

* 检测安装是否成功：`mysql` 

* 修改bind-address（允许远程主机访问）： `vim /etc/mysql/mysql.conf.d/mysqld.cnf`

  ```shell
  
  [mysqld_safe]
  socket          = /var/run/mysqld/mysqld.sock
  nice            = 0
  
  [mysqld]
  #
  # * Basic Settings
  #
  user            = mysql
  pid-file        = /var/run/mysqld/mysqld.pid
  socket          = /var/run/mysqld/mysqld.sock
  port            = 3306
  basedir         = /usr
  datadir         = /var/lib/mysql
  tmpdir          = /tmp
  lc-messages-dir = /usr/share/mysql
  skip-external-locking
  #
  # Instead of skip-networking the default is now to listen only on
  # localhost which is more compatible and is not less secure.
  # bind-address          = 127.0.0.1
  # 注释掉绑定的ip地址
  ```

  > mysql -u root -p123456    默认安装的话密码是任何字符都可以，也就是没有密码。

* 修改默认编码格式：`vim  /etc/mysql/mysql.conf.d/mysqld.cnf`

  * 更改前编码：

  ```shell
  mysql> STATUS
  --------------
  mysql  Ver 14.14 Distrib 5.7.31, for Linux (x86_64) using  EditLine wrapper
  
  Connection id:          6
  Current database:
  Current user:           root@localhost
  SSL:                    Not in use
  Current pager:          stdout
  Using outfile:          ''
  Using delimiter:        ;
  Server version:         5.7.31-0ubuntu0.18.04.1 (Ubuntu)
  Protocol version:       10
  Connection:             Localhost via UNIX socket
  Server characterset:    latin1     # 
  Db     characterset:    latin1	   # 默认编码，需要修改为utf-8
  Client characterset:    utf8
  Conn.  characterset:    utf8
  UNIX socket:            /var/run/mysqld/mysqld.sock
  Uptime:                 26 min 3 sec
  
  Threads: 1  Questions: 15  Slow queries: 0  Opens: 105  Flush tables: 1  Open tables: 98  Queries per second avg: 0.009
  --------------
  
  mysql> 
  ```

  	* 更改：`root@ubuntu:vim  /etc/mysql/mysql.conf.d/mysqld.cnf`

  ```shell
  [mysqld]
  #
  # * Basic Settings
  #
  user            = mysql
  pid-file        = /var/run/mysqld/mysqld.pid
  socket          = /var/run/mysqld/mysqld.sock
  port            = 3306
  basedir         = /usr
  datadir         = /var/lib/mysql
  tmpdir          = /tmp
  lc-messages-dir = /usr/share/mysql
  skip-external-locking
  character-set-server=utf8    # 添加该句
  #
  # Instead of skip-networking the default is now to listen only on
  # localhost which is more compatible and is not less secure.
  # bind-address          = 127.0.0.1
  ```

  * 重启服务，更改后编码：

  ```
  mysql> STATUS
  --------------
  mysql  Ver 14.14 Distrib 5.7.31, for Linux (x86_64) using  EditLine wrapper
  
  Connection id:          2
  Current database:
  Current user:           root@localhost
  SSL:                    Not in use
  Current pager:          stdout
  Using outfile:          ''
  Using delimiter:        ;
  Server version:         5.7.31-0ubuntu0.18.04.1 (Ubuntu)
  Protocol version:       10
  Connection:             Localhost via UNIX socket
  Server characterset:    utf8
  Db     characterset:    utf8
  Client characterset:    utf8
  Conn.  characterset:    utf8
  UNIX socket:            /var/run/mysqld/mysqld.sock
  Uptime:                 5 sec
  
  Threads: 1  Questions: 5  Slow queries: 0  Opens: 105  Flush tables: 1  Open tables: 98  Queries per second avg: 1.000
  --------------
  
  mysql> 
  ```

* 授权远程登录：

  ```mysql
  mysql> GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' IDENTIFIED BY '123456' WITH GRANT OPTION;
  Query OK, 0 rows affected, 1 warning (0.00 sec)
  
  ```

  ```
  mysql> FLUSH PRIVILEGES;
  Query OK, 0 rows affected (0.00 sec)
  ```

* 进行远程连接: 使用Navicat进行连接，如果不能连接；请百度开放Ubuntu的3306端口。

* 修改MySql用户密码：[{待补充}](https://blog.csdn.net/qq_42410605/article/details/96496394)

### 2.2 安装Java环境

* 下载JDK

```shell
https://www.oracle.com/technetwork/java/javase/downloads/index.html
```

* 安装JDK

  ```shell
  #上传jdk到/export/software路径下去，井解压
  
  tar -zxvf jdk-8u161-linux-x64.tar.gz -C /export/servers/
  
  mv jdk1.8.0_161 jdk
  ```

* 配置JDK环境变量

  ```shell
  vi /etc/profile
  
  添加以下内容：
  
  export JAVA_HOME=/export/servers/jdk
  
  export PATH=$PATH:$JAVA_HOME/bin
  
  export CLASSPATH=.:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar
  
  修改完成之后记得source /etc/profle生效
  
  source /etc/profile
  ```

* JDK环境验证

  ```shell
  java -version
  ```

  

### 2.3 配置python环境

* 检测python3、pip3：`python3`、`pip3 install numpy`

* 安装python3、pip3：`apt-get install python3`、`apt-get install python3-pip`

* 收集和安装依赖：



```shell
pip： 

pip freeze > requirements.txt
pip install -r requirements.txt

conda：

conda list -e > requirements.txt
conda install --yes --file requirements.txt

```



* 安装conda环境：[官网](https://www.anaconda.com/products/individual)

  * 选择合适的Anaconda版本地址：https://repo.anaconda.com/archive/Anaconda3-2020.07-Linux-x86_64.sh

  * 下载安装包：

    ```shell
    cd /export/software/
    wget https://repo.continuum.io/archive/Anaconda3-4.4.0-Linux-x86_64.sh
    bash Anaconda3-4.4.0-Linux-x86_64.sh
    ```


> 一路回车；

  * 修改环境变量：

    ```shell
    vi ~/.bashrc
    ```

    >  在bashrc文件的最后添加：export PATH="/home/用户名/anaconda3/bin:$PATH"。（vi编辑器中按i进入编辑模式） 
    >
    > 如：`export PATH="/root/anaconda3/bin:$PATH"`

  * 更新 `.bashrc` 使得环境变量生效： ` source ~/.bashrc ` 

  * 更改镜像源：

    ```shell
    修改了清华源，
    
    conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/msys2/
    conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/conda-forge/
    conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/free/
    conda config --set show_channel_urls yes
    ```

    

  * 升级conda：

    ```
    conda update conda
    conda update anaconda
    conda update python
    ```

  * [conda管理python环境](https://zhuanlan.zhihu.com/p/22678445)：
  
    * 环境管理：

    *  conda常用命令

      - 查看当前系统下的环境
    
      ```text
      conda info -e
      ```
    
    - 创建新的环境
    
    ```text
      # 指定python版本为2.7，注意至少需要指定python版本或者要安装的包# 后一种情况下，自动安装最新python版本
      conda create -n env_name python=2.7
      # 同时安装必要的包
      conda create -n env_name numpy matplotlib python=2.7
    ```
  
    - 环境切换
    
    ```text
      # 切换到新环境# linux/Mac下需要使用source activate env_name
      activate env_name
      #退出环境，也可以使用`activate root`切回root环境
    deactivate env_name
    ```

      - 移除环境
  
      ```text
    conda remove -n env_name --all
      ```

      *  给某个特定环境安装package 
  
      ```text
      conda install -n env_name pandas
      ```

      - 查看已经安装的package

      ```text
      conda list
      # 指定查看某环境下安装的package
    conda list -n env_name
      ```

      - 查找包
  
      ```text
      conda search pyqtgraph
      ```
  
    - 更新包
    
      ```text
      conda update numpy
      conda update anaconda
      ```
    
      - 卸载包
    
      ```text
      conda remove numpy
      ```

### 2.4 Redis环境配置:[here](https://www.cnblogs.com/pinuocao/p/12402225.html)

* 安装redis：

  ```shell
  $ sudo apt-get install redis-server
  ```

  * 或者：

  ```shell
  $ wget http://download.redis.io/releases/redis-5.0.5.tar.gz
  解压：tar -zxf redis-5.0.5.tar.gz
  进入redis目录：cd redis-5.0.5
  编译：make
  ```

* 配置：参照[here](https://www.cnblogs.com/pinuocao/p/12402225.html)

### 2.5 Tomcat ：[官网](http://tomcat.apache.org/)

* 下载Tomcat 9.0.38:

  ```shell
  wget https://mirror.bit.edu.cn/apache/tomcat/tomcat-9/v9.0.38/bin/apache-tomcat-9.0.38.tar.gz
  ```

* 解压到指定目录：` tar -zxvf apache-tomcat-9.0.38.tar.gz -C /export/servers/`

* 重命名文件名：`mv apache-tomcat-9.0.38/ tomcat9`

* 更改启动脚本`startup.sh`:

  ```shell
  #set java environment
  export JAVA_HOME=/export/servers/jdk
  export JRE_HOME=${JAVA_HOME}/jre
  export CLASSPATH=.:%{JAVA_HOME}/lib:%{JRE_HOME}/lib
  export PATH=${JAVA_HOME}/bin:$PATH
  
  #tomcat
  export TOMCAT_HOME=/export/servers/tomcat9
  ```

* 启动Tomcat： `./startup.sh `

### 2.6 Docker 环境安装

安装参考：[菜鸟教程](https://www.runoob.com/docker/ubuntu-docker-install.html)

* 查看Linux的IP地址

  ```shell
  ip addr
  ```

* 使用客户端连接Linux

#### 2.6.1 **在Linux上安装docker**

步骤：

* 检查内核版本，必须是3.10及以上：`uname -r`
* 安装docker： `yum install docker`  或  [参考菜鸟教程](https://www.runoob.com/docker/ubuntu-docker-install.html)  、[使用阿里云镜像](https://blog.csdn.net/sun_hui_/article/details/100581161)

* 启动docker：`systemctl start docker`
* 查看docker版本： `docker -v`
* 开机启动docker: `systemctl enable docker`
* 停止docker： `systemctl stop docker`

#### 2.6.2 **Docker常用命令&操作**

##### 2.6.2.1 **镜像操作**



| 操作 | 命令                                         | 说明                                                    |
| ---- | -------------------------------------------- | ------------------------------------------------------- |
| 检索 | docker search 关键字 eg：docker search redis | 我们经常要去docker hub上检索镜像的详细信息，如镜像的tag |
| 拉取 | docker pull 镜像名:tag                       | :tag是可选的，tag表示标签                               |
| 列表 | docker images                                | 查看所有的本地镜像                                      |
| 删除 | docker rmi images-id                         | 删除指定的本地镜像                                      |

https://hub.docker.com/

##### 2.6.2.2 容器操作

软件镜像（QQ安装程序)----运行镜像----产生一个容器(正在运行的软件，运行的QQ) ;

步骤：

* 搜索镜像：`docker search tomcat`
* 拉取镜像：`docker pull tomcat`
* 根据镜像启动容器： `docker run --name mytomcat -d tomcat:latest`
* 查看运行中的容器：`docker ps`
* 停止运行中的容器：`docker stop 容器的id`
* 查看所有的容器： `docker ps -a`
* 启动容器： `docker start 容器id`
* 删除一个容器：`docker rm 容器id`
* 启动一个做了端口映射的tomcat： `docker run -d -p 8888:8080 tomcat`
  * -d:后台运行
  * -p：将主机端口映射到容器的一个端口   主机端口:容器内部端口
* 关闭Linux的防火墙：
  * 查看防火墙状态：`service firewalld status`
  * 关闭防火墙：`service firewalld stop`
* 查看容器的日志： `docker logs container-name/container-id`
* 更多命令参看：[https: //docs.docker .com/engine/reference/commandline/docker/](https: //docs.docker .com/engine/reference/commandline/docker/)
  * 可以参考每一个镜像的文档

##### 2.6.2.3 安装MySql示例

```shell
docker pull mysql
```

* 错误的启动

```shell
docker run --name mysql01 -d mysql
```

* 正确的启动

```shell
docker run --name mysql101 -e MYSQL_ROOT_PASSWORD=123456 -d mysql
```

* 做了端口映射

```shell
docker run -p3306:3306 --name mysql102 -e MYSQL_ROOT_PASSWORD=123456 -d mysql
```

* 几个其他的高级操作

```shell
docker run --name mysql03 -v /conf/mysql:/etc/mysq1/conf.d -e MSQL_ROOT_PASSMORD=my-secret-pw-d mysql:tag

# 把主机的/conf/mysql文件夹挂载到 mysqldocker容器的/etc/mysq1/conf.d文件夹里面改mysql的配置文件就只需要把mysql配置文件放在自定义的文件夹下(/conf/mysql)

docker run --name some-mysql -e MYSQL_ROOT_PASSWORD=my-secret-pw -d mysql:tag --character-set-server=utf8mb4 --collation-server=utf8mb4_unicode_ci
# 指定mysql的一些配置参数

```

##### 2.6.2.4 安装zookeeper

> CAP原则又称CAP定理，指的是在一个分布式系统中，一致性（Consistency）、可用性（Availability）、分区容错性（Partition tolerance）。CAP 原则指的是，这三个要素最多只能同时实现两点，不可能三者兼顾。

* zookeeper 是CP 强一致性的

>  访问[hub.docker.com](https://hub.docker.com/) 主要是获得安装软件的信息及文档 

* 安装步骤：

```shell
klein@klein:~$ docker -v   # 查看docker版本
klein@klein:~$ docker search zookeeper   # 搜索镜像
klein@klein:~$ sudo docker pull zookeeper   # 拉取镜像
klein@klein:~$ docker images  # 查看已安装镜像
root@klein:/home/klein# docker start zookeeper #启动zookeeper
root@klein:/home/klein# docker ps -a    # 查看启动镜像进程
```

* 进入zookeeper shell终端

```shell
(base) root@klein:/home/klein# docker exec -it zk0 bash
```

* 启动zookeeper 服务

```shell
root@73dd67ae6722:/apache-zookeeper-3.6.2-bin# cd bin/
root@73dd67ae6722:/apache-zookeeper-3.6.2-bin/bin# ./zkServer.sh start
ZooKeeper JMX enabled by default
Using config: /conf/zoo.cfg
Starting zookeeper ... already running as process 120.
```

* 启动客户端： 

```shell
root@73dd67ae6722:/apache-zookeeper-3.6.2-bin/bin# ./zkCli.sh 
```

* 在客户端操作

```shell
[zk: localhost:2181(CONNECTED) 0] ls /
[services, zookeeper]
[zk: localhost:2181(CONNECTED) 1] ls
ls [-s] [-w] [-R] path
[zk: localhost:2181(CONNECTED) 2] ls /services 
[cloud-provider-payment]
[zk: localhost:2181(CONNECTED) 3] ls /services/cloud-provider-payment 
[305ca35d-55fc-4b21-a8c8-74582694c48c]
[zk: localhost:2181(CONNECTED) 4] ls /services/cloud-provider-payment/305ca35d-55fc-4b21-a8c8-74582694c48c 
[]
[zk: localhost:2181(CONNECTED) 5] get /services/cloud-provider-payment/305ca35d-55fc-4b21-a8c8-74582694c48c 
{"name":"cloud-provider-payment","id":"305ca35d-55fc-4b21-a8c8-74582694c48c","address":"DESKTOP-AUK8T9H","port":8004,"sslPort":null,"payload":{"@class":"org.springframework.cloud.zookeeper.discovery.ZookeeperInstance","id":"application-1","name":"cloud-provider-payment","metadata":{}},"registrationTimeUTC":1606791533908,"serviceType":"DYNAMIC","uriSpec":{"parts":[{"value":"scheme","variable":true},{"value":"://","variable":false},{"value":"address","variable":true},{"value":":","variable":false},{"value":"port","variable":true}]}}
[zk: localhost:2181(CONNECTED) 6] 
```

* json解析后的内容

```json
{
  "name": "cloud-provider-payment",
  "id": "305ca35d-55fc-4b21-a8c8-74582694c48c",
  "address": "DESKTOP-AUK8T9H",
  "port": 8004,
  "sslPort": null,
  "payload": {
    "@class": "org.springframework.cloud.zookeeper.discovery.ZookeeperInstance",
    "id": "application-1",
    "name": "cloud-provider-payment",
    "metadata": {}
  },
  "registrationTimeUTC": 1606791533908,
  "serviceType": "DYNAMIC",
  "uriSpec": {
    "parts": [
      {
        "value": "scheme",
        "variable": true
      },
      {
        "value": "://",
        "variable": false
      },
      {
        "value": "address",
        "variable": true
      },
      {
        "value": ":",
        "variable": false
      },
      {
        "value": "port",
        "variable": true
      }
    ]
  }
}
```



## 3、Linux系统目录结构

| **目录** | **说明**                                          |
| -------- | ------------------------------------------------- |
| /        | 虚拟目录的根目录，通常不会在这里存储文件          |
| /bin     | 二进制目录，存放用户级的GNU工具                   |
| /boot    | 启动目录，存放启动文件                            |
| /dev     | 设备目录，系统在这里创建设备节点                  |
| /etc     | 系统配置文件目录                                  |
| /home    | 主目录，系统在这里创建用户目录                    |
| /lib     | 库目录，存放系统和应用程序的库文件                |
| /media   | 媒体目录，可移动媒体设备的常用挂载点              |
| /mnt     | 挂载目录，另一个可移动媒体设备的常用挂载点        |
| /opt     | 可选目录，常用于存放第三方软件包和数据文件        |
| /proc    | 进程目录，存放现有硬件及当前进程的相关信息        |
| /root    | root用户的主目录                                  |
| /sbin    | 系统二进制目录，存放许多gnu管理员级工具           |
| /run     | 运行目录，存放系统运作时的运行时数据              |
| /srv     | 服务目录，存放本地服务的相关文件                  |
| /sys     | 系统目录，存放系统硬件信息的相关文件              |
| /tmp     | 临时目录，可以在该目录中创建删除临时工作文件      |
| /usr     | 用户二进制目录，大量用户级的gnu工具和数据文件存储 |
| /var     | 可变目录，用以存放经常变化的文件，比如日志文件    |

* 个人目录

  ```shell
  mkdir -p /export/data      # 存放一些数据文件
  
  mkdir -p /export/software  # 存放安装包
  
  mkdir -p /export/servers   # 软件安装目录：jdk、tomcat等
  
  cd /home/username/         # 进入用户个人空间
  ```

  