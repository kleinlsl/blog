---
title: hexo+github搭建个人博客                    #文章页面上的显示名称，一般是中文
date: 2020-01-15 19:14:16 #文章生成时间，一般不改，当然也可以任意修改
tags: [Hexo,Node.js,git]
categories: [Hexo]
copyright: true
comments: false
description: 记录使用 hexo+GitHub搭建个人博客
top: true
photos: 
	- "/images/forrestgump.png"
---



> 记录使用 hexo+GitHub搭建个人博客。

## 1、前置工作

* 了解GitHub、git

* 能够通过Windows和git向GitHub提交代码

* 安装node.js、npm，并了解其用法

* 安装git客户端

* Github创建名为：`你的用户名.github.io`

  > <!--more-->

* 配置SSH key

  ```powershell
  cd ~/. ssh #检查本机已存在的ssh密钥
  ```

  ```shell
  ssh-keygen -t rsa -C "邮件地址"
  ```

  

* 测试是否成功：` ssh -T git@github.com # 注意邮箱地址不用改 `

  ```shell
  $ git config --global user.name "liuxianan"// 你的github用户名，非昵称
  $ git config --global user.email  "xxx@qq.com"// 填写你的github注册邮箱
  ```

## 2、hexo写博客

* Hexo：[官网](http://hexo.io/) 、 [github](https://github.com/hexojs/hexo)

* 原理： hexo所做的就是将这些md文件都放在本地，每次写完文章后调用写好的命令来批量完成相关页面的生成，然后再将有改动的页面提交到github。 

* 安装：

  ```shell
  $ npm install -g hexo
  ```

* 初始化

  ```shell
  $ cd /d/Workspaces/blog/
  $ hexo init
  ```

  >  hexo会自动下载一些文件到这个目录，包括node_modules， 

  ```shell
  $ hexo g # 生成
  $ hexo s # 开启本地预览服务，打开浏览器访问 http://localhost:4000
  $ hexo d # 部署到Github
  ```

* 修改主题：[官方主题](https://hexo.io/themes/)

  * 下载主题：

    ```shell
    $ cd /d/Workspaces/blog/
    $ git clone https://github.com/litten/hexo-theme-yilia.git themes/yilia
    ```

  * 配置主题：

     修改`_config.yml`中的`theme: landscape`改为`theme: yilia`，然后重新执行`hexo clean` 、 `hexo g`来重新生成。 

* 上传到GitHub：

  >  `ssh key`肯定要配置好。

  *  配置Github： 

    > 配置`_config.yml`中有关deploy的部分： 

    ```yml
    deploy:
      type: git
      repository: git@github.com:kleinlsl/kleinlsl.github.io.git
      branch: master
    ```

  * 提交代码：`hexo d`

## 3、常用命令

```shell
hexo new "postName" #新建文章
hexo new page "pageName" #新建页面
hexo generate #生成静态页面至public目录
hexo server #开启预览访问端口（默认端口4000，'ctrl + c'关闭server）
hexo deploy #部署到GitHub
hexo help  # 查看帮助
hexo version  #查看Hexo的版本
```

```shell
hexo n == hexo new
hexo g == hexo generate
hexo s == hexo server
hexo d == hexo deploy
```

```shell
hexo s -g #生成并本地预览
hexo d -g #生成并上传
```

## 4、写博客

* 新建博客： ` hexo new 'my-first-blog' `  hexo会帮我们在`_posts`下生成相关md文件： 

* 打开编辑：`/source/_posts/my-first-blog.md`

  ```markdown
  ---
  title: postName #文章页面上的显示名称，一般是中文
  date: 2013-12-02 15:30:16 #文章生成时间，一般不改，当然也可以任意修改
  categories: 默认分类 #分类
  tags: [tag1,tag2,tag3] #文章标签，可空，多标签请用格式，注意:后面有个空格
  description: 附加一段文章摘要，字数最好在140字以内，会出现在meta的description里面
  ---
  
  以下是正文
  <!--more-->
  以下是更多
  ```

## 5、添加分类和标签

#### 5.1  创建“分类”选项

* 生成“分类”页 ：

```cpp
$ hexo new page categories
```

* 打开index.md,添加tpye属性:

```css
---
title: 分类
date: 2019-04-24 15:30:30
type: categories
---
```

> 保存并关闭文件。

* 给文章添加“categories”属性

> 打开需要添加分类的文章，为其添加categories属性。下方的categories:Hexo表示这篇文章添加到到“Hexo”这个分类。注意：一篇文章只会添加到一个分类中，如果是多个默认放到第一个分类中。

```css
---
title: Hexo 添加分类及标签
date: 2017-05-26 12:12:57
categories: Hexo
---
```

> 至此，成功给文章添加分类，点击首页的“分类”可以看到该分类下的所有文章。当然，只有添加了categories: xxx的文章才会被收录到首页的“分类”中

#### 5.2 创建“标签”选项

* 生成“标签”页:

```cpp
$ hexo new page tags
```

* 找到index.md这个文件,并添加tpye属性:

```css
---
title: 标签
date: 2019-04-24 15:40:24
type: tags
---
```

> 保存并关闭文件。

* 给文章添加“tags”属性,打开需要添加标签的文章，为其添加tags属性。

```css
---
title: Hexo 添加分类及标签
date: 2019-04-24 15:40:24
categories: 
           - Hexo
tags:
           - 博客
---
```

## 6、 Front-matter 区域配置：

* 使用YAML块标记，用于配置作品设置。

  | 设置              | 描述                                                         | 默认             |
  | :---------------- | :----------------------------------------------------------- | :--------------- |
  | `layout`          | 布局                                                         |                  |
  | `title`           | 标题                                                         | 文件名（仅帖子） |
  | `date`            | 发布日期                                                     | 文件创建日期     |
  | `updated`         | 更新日期                                                     | 文件更新日期     |
  | `comments`        | 为帖子启用评论功能                                           | 真正             |
  | `tags`            | 标签（不适用于页面）                                         |                  |
  | `categories`      | 类别（不适用于页面）                                         |                  |
  | `permalink`       | 覆盖帖子的默认永久链接                                       |                  |
  | `excerpt`         | 纯文本的页面摘录。使用[此插件](https://hexo.io/docs/tag-plugins#Post-Excerpt)格式化文本 |                  |
  | `disableNunjucks` | 启用时禁用Nunjucks标签`{{ }}`/`{% %}`和[标签插件的](https://hexo.io/docs/tag-plugins)呈现 |                  |

* 案例：

  ```
  categories:
  	- Sports
  	- Baseball
  tags:
  	- Injury
  	- Fight
  	- Shocking
  ```

* 其他设置

  ```
  top: true   # 是否置顶文章
  photos: [ "img_url_01","img_url_02"]  # 文章封面图
  ```

## 7、开启相关功能

> 搜索、目录、评论、置顶、相册等

### 7.1 开启搜索

* [hexo-generator-search](https://github.com/wzpan/hexo-generator-search) 本地检索
  * 安装

  ```shell
  $ npm install hexo-generator-searchdb --save
  ```

  * 配置：
    Hexo 的配置文件 `_config.yml` 添加插件配置（注意：不是主题的配置文件）：

  ```yaml
  search:  
  	path: search.xml  
  	field: post  
  	content: true
  ```

### 7.2 开启目录

使用 [Tocbot](http://tscanlin.github.io/tocbot/) 解析内容中的标题标签 (h1~h6) 并插入目录。使用方法：

> Ocean 配置 Toc 只作用于 Post 页面，在其他 Page 中不作用。

* 在主题配置文件 `ocean/_config.yml` 中开启

```yaml
# Toc
toc: true
```

* 在 Post 中关闭 Toc

如果在 `ocean/_config.yml` 开启了 Toc ，那么 Tocbot 会在每一篇 Blog 都解析内容中的标题标签生成 Toc 文章目录，但并不是所有的 Blog 中都需要 Toc，所以在 markdown 的 Front-matter 部分可以关闭：

```yaml
---
toc: false
---
```

### 7.3 开启评论：[here](https://zhwangart.github.io/2018/12/06/Gitalk/)

### 7.4 开启置顶

- 安装

  ```yaml
  $ cnpm uninstall hexo-generator-index --save
  $ cnpm install hexo-generator-index-pin-top --save
  ```

- 开启

  在需要置顶的文章的 Front-matter 区域加上 `top: ture` ，示例：

  ```yaml
  --- 
  title: 新增文章置顶 
  author: zhwangart 
  date: 2019-07-18 15:45:03 
  top: ture 
  ---
  ```

### 7.5 开启相册

首先需要创建一个 page ，关于页面也一样需要创建。

```shell
$ hexo new page gallery
```

然后在编辑 markdown 的时候需要写在 Front-matter 部分，这种写法可能不是特别特别的好，希望能有更好的方法。

```yaml
---
title: Gallery
albums: [        
	["img_url","img_caption"],
    ["img_url","img_caption"]
    ]
---
```

### 7.6 开启RSS

- 安装

  ```shell
  $ npm install hexo-generator-feed --save
  ```

- 配置
  Hexo 的配置文件 `_config.yml` 添加插件配置（注意：不是主题的配置文件）：

  ```yaml
  feed:     
  	type: atom
      path: atom.xml
      limit: 20
      hub:
      content:
      content_limit: 140
      content_limit_delim: ''
      order_by: -date
  ```

## 推荐主题

* [Ocean](https://github.com/zhwangart/hexo-theme-ocean)

  * 克隆： ` git clone https://github.com/zhwangart/hexo-theme-ocean.git themes/ocean`
* 配置文档：https://zhwangart.github.io/2018/11/30/Ocean/
  * 展示：https://zhwangart.github.io/

## 错误解决

* 错误：

  ```powershell
  D:\Workspaces\blog>hexo version
  INFO  Validating config
  WARN  Deprecated config detected: "use_date_for_updated" is deprecated, please use "updated_option" instead. See https://hexo.io/docs/configuration for more details.
  ```

  * 解决方案：

    ​	找到hexo的配置文件：`_config.yml` 更改其中的：`use_date_for_updated` 为 `updated_option`