---
title: MyBatis-Plus 代码生成器                   #文章页面上的显示名称，一般是中文
date: 2020-12-14 16:14:16 #文章生成时间，一般不改，当然也可以任意修改
tags: [MyBatis]
copyright: true
comments: false
description: MyBatis-Plus 代码生成器
top: 
---



>  全新的 `MyBatis-Plus` 3.0 版本基于 JDK8，提供了 `lambda` 形式的调用，所以安装集成 MP3.0 要求如下： 
>
> [官方文档](https://mybatis.plus/guide/config.html)

* Spring Boot:

  ```xml
  <dependency>
      <groupId>com.baomidou</groupId>
      <artifactId>mybatis-plus-boot-starter</artifactId>
      <version>3.4.1</version>
  </dependency>
  ```

* Spring MVC:

  ```xml
  <dependency>
      <groupId>com.baomidou</groupId>
      <artifactId>mybatis-plus</artifactId>
      <version>3.4.1</version>
  </dependency>
  ```

<!-- more -->

# [MyBatis-Plus 代码生成器](https://mybatis.plus/guide/generator.html#%E8%87%AA%E5%AE%9A%E4%B9%89%E5%B1%9E%E6%80%A7%E6%B3%A8%E5%85%A5)

## 1、添加依赖：

 MyBatis-Plus 从 `3.0.3` 之后移除了代码生成器与模板引擎的默认依赖，需要手动添加相关依赖： 

*  添加 代码生成器 依赖 

```xml
<dependency>
    <groupId>com.baomidou</groupId>
    <artifactId>mybatis-plus-generator</artifactId>
    <version>3.4.1</version>
</dependency>
```

*  添加 模板引擎 依赖： MyBatis-Plus 支持 Velocity（默认）、Freemarker、Beetl

  **Velocity**

```xml
<dependency>
    <groupId>org.apache.velocity</groupId>
    <artifactId>velocity-engine-core</artifactId>
    <version>2.2</version>
</dependency>
```

​	 **Freemarker**

```xml
<dependency>
    <groupId>org.freemarker</groupId>
    <artifactId>freemarker</artifactId>
    <version>2.3.30</version>
</dependency>
```

​	**Beetl**

```xml
<dependency>
    <groupId>com.ibeetl</groupId>
    <artifactId>beetl</artifactId>
    <version>3.3.0.RELEASE</version>
</dependency>
```

## 2、编写配置：[参考](https://mybatis.plus/config/generator-config.html#%E5%9F%BA%E6%9C%AC%E9%85%8D%E7%BD%AE)

* 创建生成器对象：

  ```java
  		// 1、代码生成器
          AutoGenerator mpg = new AutoGenerator();
  ```

  

*  配置 GlobalConfig ：

  ```java
          // 2、全局配置
          GlobalConfig gc = new GlobalConfig();
          String projectPath = System.getProperty("user.dir");
          log.info(projectPath);
          gc.setOutputDir(projectPath+"/src/main/java");
          gc.setAuthor("dell");
          gc.setOpen(false);
          gc.setFileOverride(false);                      //重新生成时文件是否覆盖
          gc.setServiceName("%sService");	                //去掉Service接口的首字母I
          gc.setIdType(IdType.ID_WORKER_STR);             //主键策略
          gc.setDateType(DateType.ONLY_DATE);             //定义生成的实体类中日期类型
          gc.setSwagger2(true);                           //开启Swagger2模式
  ```

* 配置数据源：

  ```java
          // 3、数据源配置
          DataSourceConfig dsc = new DataSourceConfig();
          dsc.setUrl("jdbc:mysql://192.168.121.152:3306/newbee_mall_db?useUnicode=true&useSSL=false&characterEncoding=utf8");
          dsc.setDriverName("com.mysql.cj.jdbc.Driver");
          dsc.setUsername("root");
          dsc.setPassword("123456");
          dsc.setDbType(DbType.MYSQL);
          mpg.setDataSource(dsc);
  ```

* 包配置：

  ```java
          // 4、包配置
          PackageConfig pc = new PackageConfig();
  //        pc.setModuleName("provider");    //模块名
          pc.setParent("edu.lsl");
          pc.setController("controller");
          pc.setEntity("bean");
          pc.setService("service");
          pc.setMapper("mapper");
          mpg.setPackageInfo(pc);
  ```

* 策略配置：

  ```java
          // 5、策略配置
          StrategyConfig strategy = new StrategyConfig();
          strategy.setNaming(NamingStrategy.underline_to_camel);          //数据库表映射到实体的命名策略
          strategy.setColumnNaming(NamingStrategy.underline_to_camel);    //数据库表字段映射到实体的命名策略
          strategy.setEntityLombokModel(true);                            // lombok 模型 @Accessors(chain = true) setter链式操作
          strategy.setRestControllerStyle(true);                          //restful api风格控制器
          strategy.setControllerMappingHyphenStyle(true);                 //url中驼峰转连字符
          // 公共父类
  //        strategy.setSuperControllerClass("你自己的父类控制器,没有就不用设置!");
          // 写于父类中的公共字段
          strategy.setSuperEntityColumns("id");
  //        strategy.setInclude(scanner("表名，多个英文逗号分割").split(","));
          strategy.setInclude("tb_newbee_mall_admin_user");
          strategy.setControllerMappingHyphenStyle(true);
  //        strategy.setTablePrefix(pc.getModuleName() + "_");            //生成实体时去掉表前缀
          strategy.setTablePrefix("tb_newbee_");                          //生成实体时去掉表前缀
          mpg.setStrategy(strategy);
  ```

* 模板配置：

  ```java
          mpg.setTemplateEngine(new FreemarkerTemplateEngine());          // 添加 模板引擎
  ```

* 执行：

  ```java
          // 6、执行
          mpg.execute();
  ```

## 3、Demo测试

```java
package edu.lsl.mbg;

import com.baomidou.mybatisplus.annotation.DbType;
import com.baomidou.mybatisplus.annotation.IdType;
import com.baomidou.mybatisplus.generator.AutoGenerator;
import com.baomidou.mybatisplus.generator.config.DataSourceConfig;
import com.baomidou.mybatisplus.generator.config.GlobalConfig;
import com.baomidou.mybatisplus.generator.config.PackageConfig;
import com.baomidou.mybatisplus.generator.config.StrategyConfig;
import com.baomidou.mybatisplus.generator.config.rules.DateType;
import com.baomidou.mybatisplus.generator.config.rules.NamingStrategy;
import com.baomidou.mybatisplus.generator.engine.FreemarkerTemplateEngine;

/**
 * @project: Mybatis-plus
 * @description: Mybatis-plus 代码生成器demo
 * @author: SongLin.Lu
 * @date: 2020/12/14 - 15:56
 */
public class CodeGenerator {
    private static String DataBaseName = "newbee_mall_db";
    private static String Url = "jdbc:mysql://192.168.121.152:3306/"+DataBaseName+"?useUnicode=true&useSSL=false&characterEncoding=utf8";
    private static String DriverName = "com.mysql.cj.jdbc.Driver";
    private static String Username = "root";
    private static String Password = "123456";
    private static String projectPath = System.getProperty("user.dir");
    
    public static void main(String[] args) {
        // 1、代码生成器
        AutoGenerator mpg = new AutoGenerator();

        // 2、全局配置
        GlobalConfig gc = new GlobalConfig()
                .setAuthor("dell")
                .setDateType(DateType.ONLY_DATE)            //定义生成的实体类中日期类型
                .setFileOverride(false)                     //重新生成时文件是否覆盖
                .setIdType(IdType.ID_WORKER_STR)            //主键策略
                .setOpen(false)
                .setOutputDir(projectPath+"/src/main/java") //输出路径
                .setServiceName("%sService")                //去掉Service接口的首字母I
                .setSwagger2(true)                          //开启Swagger2模式
                .setActiveRecord(false)
                .setControllerName(null);
        mpg.setGlobalConfig(gc);

        // 3、数据源配置
        DataSourceConfig dsc = new DataSourceConfig()
//                .setDbQuery()               // 数据库信息查询类
//                .setSchemaName()
//                .setTypeConvert()
                .setDbType(DbType.MYSQL)
                .setDriverName(DriverName)
                .setPassword(Password)
                .setUrl(Url)
                .setUsername(Username);
        mpg.setDataSource(dsc);

        // 4、包配置
        PackageConfig pc = new PackageConfig()
                .setParent("edu.lsl")
                .setController("controller")
                .setEntity("bean")
                .setMapper("mapper")
                .setService("service")
                .setModuleName("")
//                .setPathInfo()              // 路径配置信息
                .setServiceImpl("service.impl")
                .setXml("mapper.xml");
        mpg.setPackageInfo(pc);

        // 5、策略配置
        StrategyConfig strategy = new StrategyConfig()
                .setControllerMappingHyphenStyle(true)                  //url中驼峰转连字符
                .setColumnNaming(NamingStrategy.underline_to_camel)     //数据库表映射到实体的命名策略
                .setNaming(NamingStrategy.underline_to_camel)
                .setEntityLombokModel(true)
                .setRestControllerStyle(true)                           //restful api风格控制器
                .setTablePrefix("tb_newbee_")                            //生成实体时去掉表前缀
//                .setSuperControllerClass()                              //公共父类 , 你自己的父类控制器,没有就不用设置!
//                .setSuperEntityClass()                                  //自定义继承的Entity类全称，带包名
//                .setSuperEntityColumns()                                //写于父类中的公共字段
                .setInclude(
                        "tb_newbee_mall_admin_user",
                        "tb_newbee_mall_carousel"
                );
        mpg.setStrategy(strategy);

        // 6、添加模板引擎
        mpg.setTemplateEngine(new FreemarkerTemplateEngine());          // 添加 模板引擎

        // 7、执行
        mpg.execute();
    }
}

```

## 4、参考博客：

- [[累成一条狗]](https://www.cnblogs.com/l-y-h/p/12859477.html)