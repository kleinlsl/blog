---
title: IDEA和maven配置多仓库的解决方案   #文章页面上的显示名称，一般是中文
date: 2020-10-15 10:14:16 #文章生成时间，一般不改，当然也可以任意修改
tags: [Maven,IDEA]
categories: [Maven]
copyright: true
comments: false
description: 通过maven的配置文件优先级和IDEA的settings实现多仓库
top: true
photos: 
	- "/images/forrestgump.png"
---

> maven的配置文件优先级： **pom.xml> user settings > global settings** ;
>
> 通过maven的配置文件优先级和IDEA的settings(设置)实现多仓库;

<!--more-->

# 1、前言

> 公司内部开发项目时一般会有一个公司的Maven私服，这个私服只能在公司内网中使用；如果，我们不再公司的情况下可能需要下载一些其他 jar 包。基于这种情况，我们可以配置两个本地仓库：公司使用（{basedir}/.m2/repository/） 和 自己使用（{basedir}/.aliyun/repository/）。在切换maven配置文件settings.xml时，一定要清楚优先级关系。

# 2、配置文件优先级

* maven的配置文件优先级： **pom.xml> user settings > global settings** ;
  * user settings：  user.home/.m2/settings.xml 
  * global settings：${M2_HOME}/conf/settings.xml
  * 用户配置优先于全局配置，存在覆盖配置问题

# 3、解决方案

## 3.1 settings.xml文件示例：

```xml
  <!-- localRepository # 本地仓库位置
   | The path to the local repository maven will use to store artifacts.
   |
   | Default: ${user.home}/.m2/repository
  <localRepository>/path/to/local/repo</localRepository>
  -->
<localRepository>D:/Users/24458/ideaCaches/.aliyun/repository</localRepository>
```

```xml
  <mirrors>
    <!-- mirror # 镜像
     | Specifies a repository mirror site to use instead of a given repository. The repository that
     | this mirror serves has an ID that matches the mirrorOf element of this mirror. IDs are used
     | for inheritance and direct lookup purposes, and must be unique across the set of mirrors.
     |
    <mirror>
      <id>mirrorId</id>
      <mirrorOf>repositoryId</mirrorOf>
      <name>Human Readable Name for this Mirror.</name>
      <url>http://my.repository.com/repo/path</url>
    </mirror>
     -->
    <mirror>
      <id>aliyun-public</id>
      <mirrorOf>*</mirrorOf>
      <name>aliyun public</name>
      <url>https://maven.aliyun.com/repository/public</url>
    </mirror>

    <mirror>
      <id>aliyun-central</id>
      <mirrorOf>*</mirrorOf>
      <name>aliyun central</name>
      <url>https://maven.aliyun.com/repository/central</url>
    </mirror>

    <mirror>
      <id>aliyun-spring</id>
      <mirrorOf>*</mirrorOf>
      <name>aliyun spring</name>
      <url>https://maven.aliyun.com/repository/spring</url>
    </mirror>

    <mirror>
      <id>aliyun-spring-plugin</id>
      <mirrorOf>*</mirrorOf>
      <name>aliyun spring-plugin</name>
      <url>https://maven.aliyun.com/repository/spring-plugin</url>
    </mirror>
  </mirrors>

```

```xml
 <!--配置jdk版本-->
  <profiles>
    <profile>
      <id>jdk-1.8</id>
      <activation>
        <activeByDefault>true</activeByDefault>
        <jdk>1.8</jdk>
      </activation>
      <properties>
        <maven.compiler.source>1.8</maven.compiler.source>
        <maven.compiler.target>1.8</maven.compiler.target>
        <maven.compiler.compilerVersion>1.8</maven.compiler.compilerVersion>
      </properties>
    </profile>
  </profiles>
```

## 3.2 配置不同仓库的配置文件

> 主要配置本地仓库位置和镜像

* settings.xml  :  公司配置

  ```
  <localRepository>D:/Users/24458/ideaCaches/.m2/repository</localRepository>
  ```

* settings.me.xml  :  自己的配置

  ```xml
  <localRepository>D:/Users/24458/ideaCaches/.aliyun/repository</localRepository>
  ```

  

## 3.3 IDEA选择要使用的配置文件

> 打开： file --> settings --> Maven:

![](/images/blog-images/IDEA和maven配置多仓库的解决方案/IDEA-settings.png)

## 3.4 查看当前所用settings.xml内容

![1602730526685](/images/blog-images/IDEA和maven配置多仓库的解决方案/effective-settings.png)

![](/images/blog-images/IDEA和maven配置多仓库的解决方案/effective-settings-01.png)