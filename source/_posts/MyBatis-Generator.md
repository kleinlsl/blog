---
title: MyBatis Generator 详细配置 demo                  #文章页面上的显示名称，一般是中文
date: 2020-12-16 14:14:16 #文章生成时间，一般不改，当然也可以任意修改
tags: [MyBatis]
copyright: true
comments: false
---



# [MyBatis Generator 详细配置 demo](https://www.cnblogs.com/xuxiaobai13/p/11846772.html)

> MyBatis Generator 是 MyBatis 提供的一个代码生成工具。可以帮我们生成 表对应的持久化对象(po)、操作数据库的接口(dao)、CRUD sql的xml(mapper)。
>
> MyBatis Generator 是一个独立工具，你可以下载它的jar包来运行、也可以在 Ant 和 maven 运行。

> 目前已知的代码生成器有两种： [MyBatis-Plus](https://baomidou.com/)  和 [MyBatis Generator](http://mybatis.org/generator/index.html) ;前者是后者的加强版，对于MyBatis-Plus的使用可以参见： [MyBatis-Plus 3.4.1 代码生成器 配置demo](https://blog.csdn.net/qq_41971768/article/details/111178082) 

<!-- more -->

## 0、使用环境

* 开发工具：IDEA
* 数据库：mysql
* 包管理工具：maven


## 1、[配置 MyBatis Generator Config](https://blog.csdn.net/testcs_dn/article/details/79295065)

> MyBatis Generator 插件启动后，会根据你在 pom 中配置都路径找到该配置文件。
>
> 这个配置文件才是详细都配置 MyBatis Generator 生成代码的各种细节。
>
> 其中最重要的就是 **context** ，你的配置文件至少得包含一个**context**

* 配置示例：

  ```xml
  <?xml version="1.0" encoding="UTF-8"?>
  <!DOCTYPE generatorConfiguration
          PUBLIC "-//mybatis.org//DTD MyBatis Generator Configuration 1.0//EN"
          "http://mybatis.org/dtd/mybatis-generator-config_1_0.dtd">
  <generatorConfiguration>
      <!-- 引入配置文件 -->
      <properties resource="generator.properties"/>
  
      <!-- 一个数据库一个context,context的子元素必须按照它给出的顺序
          property*,plugin*,commentGenerator?,jdbcConnection,javaTypeResolver?,
          javaModelGenerator,sqlMapGenerator?,javaClientGenerator?,table+
      -->
  
      <context id="MySqlContext" targetRuntime="MyBatis3" defaultModelType="flat">
          <property name="beginningDelimiter" value="`"/>
          <property name="endingDelimiter" value="`"/>
          <property name="javaFileEncoding" value="UTF-8"/>
  
          <!-- 为模型生成序列化方法-->
          <plugin type="org.mybatis.generator.plugins.SerializablePlugin"/>
          <!-- 为生成的Java模型创建一个toString方法 -->
          <plugin type="org.mybatis.generator.plugins.ToStringPlugin"/>
  
          <!-- 注释 -->
          <!--注释生成配置-->
          <!--可以自定义生成model的代码注释-->
          <commentGenerator type="com.macro.mall.tiny.mbg.CommentGenerator">
              <!-- 是否去除自动生成的注释 true：是 ： false:否 -->
              <property name="suppressAllComments" value="true"/>
              <property name="suppressDate" value="true"/>
              <property name="addRemarkComments" value="true"/>
          </commentGenerator>
  
          <!--配置数据库连接-->
          <jdbcConnection driverClass="${jdbc.driverClass}"
                          connectionURL="${jdbc.connectionURL}"
                          userId="${jdbc.userId}"
                          password="${jdbc.password}">
              <!--解决mysql驱动升级到8.0后不生成指定数据库代码的问题-->
              <property name="nullCatalogMeansCurrent" value="true" />
          </jdbcConnection>
  
          <!--实体类配置-->
          <!--指定生成model的路径-->
          <javaModelGenerator targetPackage="com.macro.mall.tiny.mbg.model"
                              targetProject="mall-tiny-01\src\main\java">
              <property name="enableSubPackages" value="false"/>
              <property name="trimStrings" value="true"/>
          </javaModelGenerator>
  
          <!--映射文件的配置-->
          <!--指定生成mapper.xml的路径-->
          <sqlMapGenerator targetPackage="com.macro.mall.tiny.mbg.mapper"
                           targetProject="mall-tiny-01\src\main\resources">
              <property name="enableSubPackages" value="false"/>
          </sqlMapGenerator>
  
          <!--指定生成mapper接口的的路径-->
          <javaClientGenerator type="XMLMAPPER"
                               targetPackage="com.macro.mall.tiny.mbg.mapper"
                               targetProject="mall-tiny-01\src\main\java">
              <property name="enableSubPackages" value="false"/>
          </javaClientGenerator>
  
          <!-- schema为数据库名，oracle需要配置，mysql不需要配置。
              tableName为对应的数据库表名
              domainObjectName 是要生成的实体类名(可以不指定，默认按帕斯卡命名法将表名转换成类名)
              enableXXXByExample 默认为 true， 为 true 会生成一个对应Example帮助类，帮助你进行条件查询，不想要可以设为false
              -->
  <!--        <table schema="" tableName="t_admin" domainObjectName="Admin"-->
  <!--               enableCountByExample="false" enableDeleteByExample="false" enableSelectByExample="false"-->
  <!--               enableUpdateByExample="false" selectByExampleQueryId="false"-->
  <!--        >-->
  <!--            &lt;!&ndash;是否使用实际列名,默认为false&ndash;&gt;-->
  <!--            &lt;!&ndash;<property name="useActualColumnNames" value="false" />&ndash;&gt;-->
  <!--        </table>-->
          <!--生成全部表tableName设为%-->
          <table tableName="pms_brand">
              <generatedKey column="id" sqlStatement="MySql" identity="true"/>
          </table>
  
      </context>
  </generatorConfiguration>
  ```

## 2、使用MyBatis Generator 

### 2.1 使用 Maven 插件运行

> pom.xml 添加配置

```xml
<build>
    <plugins>
        <plugin>
            <groupId>org.mybatis.generator</groupId>
            <artifactId>mybatis-generator-maven-plugin</artifactId>
            <version>1.3.7</version>
            <configuration>
                <!--mybatis的代码生成器的配置文件-->
                <configurationFile>src/main/resources/mybatis-generator-config.xml</configurationFile>
                <!--允许移动生成的文件 -->
                <verbose>true</verbose>
                <!--允许覆盖生成的文件-->
                <overwrite>true</overwrite>
                <!--将当前pom的依赖项添加到生成器的类路径中-->
                <!--<includeCompileDependencies>true</includeCompileDependencies>-->
            </configuration>
            <executions>
                    <execution>
                        <id>Generate MyBatis Artifacts</id>
                        <phase>package</phase>
                        <goals>
                            <goal>generate</goal>
                        </goals>
                    </execution>
                </executions>
            <dependencies>
                <!--mybatis-generator插件的依赖包-->
                <!--<dependency>-->
                    <!--<groupId>org.mybatis.generator</groupId>-->
                    <!--<artifactId>mybatis-generator-core</artifactId>-->
                    <!--<version>1.3.7</version>-->
                <!--</dependency>-->
                <!-- mysql的JDBC驱动 -->
                <dependency>
                    <groupId>mysql</groupId>
                    <artifactId>mysql-connector-java</artifactId>
                    <version>8.0.17</version>
                </dependency>
            </dependencies>
        </plugin>
    </plugins>
<build>  
```



> 配置好后，双击 maven 中的 MyBatis Generator 运行 

#### 2.1.1 引入 MyBatis Generator 插件

```xml
<build>
    <plugins>
        <plugin>
            <groupId>org.mybatis.generator</groupId>
            <artifactId>mybatis-generator-maven-plugin</artifactId>
            <version>1.3.7</version>
        </plugin>
    <plugins>    
</build>
```

#### 2.1.2 配置 MyBatis Generator config 文件路径

MyBatis Generator 插件需要根据一个 **MyBatis Generator config** 文件，来具体运行

配置如下，版本我用的是目前最新的版本 1.3.7

```xml
<build>
    <plugins>
        <plugin>
            <groupId>org.mybatis.generator</groupId>
            <artifactId>mybatis-generator-maven-plugin</artifactId>
            <version>1.3.7</version>
            <configuration>
                <!--mybatis的代码生成器的配置文件-->
                <configurationFile>src/main/resources/mybatis-generator-config.xml</configurationFile>
            </configuration>
        </plugin>
    <plugins>    
</build>
```

>  注意，这个路径是你的配置文件相对于该 pom 文件的路径 

#### 2.1.3 允许覆盖生成的文件

> 有时候我们的数据库表添加了新字段，需要重新生成对应的文件。常规做法是手动删除旧文件，然后在用 MyBatis Generator 生成新文件。当然你也可以选择让 MyBatis Generator 覆盖旧文件，省下手动删除的步骤。

```xml
<build>
    <plugins>
        <plugin>
            <groupId>org.mybatis.generator</groupId>
            <artifactId>mybatis-generator-maven-plugin</artifactId>
            <version>1.3.7</version>
            <configuration>
                <!--mybatis的代码生成器的配置文件-->
                <configurationFile>src/main/resources/mybatis-generator-config.xml</configurationFile>
                <!--允许覆盖生成的文件-->
                <overwrite>true</overwrite>
            </configuration>
        </plugin>
    <plugins>    
</build>
```

>  值得注意的是，MyBatis Generator 只会覆盖旧的 po、dao、而 *mapper.xml 不会覆盖，而是追加，这样做的目的是防止用户自己写的 sql 语句一不小心都被 MyBatis Generator 给覆盖了 

* 问题解决

> 以前一直以为是MyBatis Generator生成的问题，直接删除mapper.xml所在文件夹，重新生成就好了,现在提供一种MyBatis Generator官方提供的解决方法。

* 升级MyBatis Generator的版本

MyBatis Generator 在1.3.7版本提供了解决方案，我们目前使用的版本为1.3.3。
```xml
<!-- MyBatis 生成器 -->
<dependency>
    <groupId>org.mybatis.generator</groupId>
    <artifactId>mybatis-generator-core</artifactId>
    <version>1.3.7</version>
</dependency>
```

* 在generatorConfig.xml文件中添加覆盖mapper.xml的插件

```xml
<!--生成mapper.xml时覆盖原文件-->
<plugin type="org.mybatis.generator.plugins.UnmergeableXmlMappersPlugin" />
```

#### 2.1.4 添加数据库驱动依赖

> MyBatis Generator 需要链接数据库，肯定是需要对应数据库驱动的依赖的。

> 如下，给 MyBatis Generator 添加数据库驱动依赖

```xml
<build>
    <plugins>
        <plugin>
            <groupId>org.mybatis.generator</groupId>
            <artifactId>mybatis-generator-maven-plugin</artifactId>
            <version>1.3.7</version>
            <configuration>
                <!--mybatis的代码生成器的配置文件-->
                <configurationFile>src/main/resources/mybatis-generator-config.xml</configurationFile>
                <!--允许覆盖生成的文件-->
                <overwrite>true</overwrite>
            </configuration>
            <dependencies>
                <!-- mysql的JDBC驱动 -->
                <dependency>
                    <groupId>mysql</groupId>
                    <artifactId>mysql-connector-java</artifactId>
                    <version>8.0.17</version>
                </dependency>
            </dependencies>
        </plugin>
    <plugins>    
</build>
```

>  大部分情况下，我们的项目中已经配置过了对应数据库的JDBC驱动。
>
> 现在在插件中又配置一次，感觉有些冗余，maven 提供了 **includeCompileDependencies** 属性，让我们在插件中引用 dependencies 的依赖，这样就不需要重复配置了。 

```xml
<build>
    <plugins>
        <plugin>
            <groupId>org.mybatis.generator</groupId>
            <artifactId>mybatis-generator-maven-plugin</artifactId>
            <version>1.3.7</version>
            <configuration>
                <!--mybatis的代码生成器的配置文件-->
                <configurationFile>src/main/resources/mybatis-generator-config.xml</configurationFile>
                <!--允许覆盖生成的文件-->
                <overwrite>true</overwrite>
                <!--将当前pom的依赖项添加到生成器的类路径中-->
                <includeCompileDependencies>true</includeCompileDependencies>
            </configuration>
        </plugin>
    <plugins>    
</build>
```

#### 2.1.5 添加其他依赖

> 一般配置了 **includeCompileDependencies** 后就不需要配置其他依赖了，因为 **includeCompileDependencies** 会将当前 pom 的 **dependencies** 中所以 **Compile** 期的依赖全部添加到生成器的类路径中。

> 但有的人不想配置 **includeCompileDependencies** ，或者想在MyBatis Generator插件中使用另一个版本的依赖，就可以配置 **dependencies**

### 2.2 Java 代码运行

```java
package com.macro.mall.tiny.mbg;

import org.mybatis.generator.api.MyBatisGenerator;
import org.mybatis.generator.config.Configuration;
import org.mybatis.generator.config.xml.ConfigurationParser;
import org.mybatis.generator.internal.DefaultShellCallback;

import java.io.InputStream;
import java.util.ArrayList;
import java.util.List;

/**
 * 用于生产MBG的代码
 * Created by macro on 2018/4/26.
 */
public class Generator {
    public static void main(String[] args) throws Exception {
        //MBG 执行过程中的警告信息
        List<String> warnings = new ArrayList<String>();
        //当生成的代码重复时，覆盖原代码
        boolean overwrite = true;
        //读取我们的 MBG 配置文件
        InputStream is = Generator.class.getResourceAsStream("/generatorConfig.xml");
        ConfigurationParser cp = new ConfigurationParser(warnings);
        Configuration config = cp.parseConfiguration(is);
        is.close();

        DefaultShellCallback callback = new DefaultShellCallback(overwrite);
        //创建 MBG
        MyBatisGenerator myBatisGenerator = new MyBatisGenerator(config, callback, warnings);
        //执行生成代码
        myBatisGenerator.generate(null);
        //输出警告信息
        for (String warning : warnings) {
            System.out.println(warning);
        }
    }
}

```



## 3、自定义注释生成器

```java
package com.macro.mall.tiny.mbg;

import org.mybatis.generator.api.IntrospectedColumn;
import org.mybatis.generator.api.IntrospectedTable;
import org.mybatis.generator.api.dom.java.Field;
import org.mybatis.generator.internal.DefaultCommentGenerator;
import org.mybatis.generator.internal.util.StringUtility;

import java.util.Properties;

/**
 * 自定义注释生成器
 * Created by macro on 2018/4/26.
 */
public class CommentGenerator extends DefaultCommentGenerator {
    private boolean addRemarkComments = false;

    /**
     * 设置用户配置的参数
     */
    @Override
    public void addConfigurationProperties(Properties properties) {
        super.addConfigurationProperties(properties);
        this.addRemarkComments = StringUtility.isTrue(properties.getProperty("addRemarkComments"));
    }

    /**
     * 给字段添加注释
     */
    @Override
    public void addFieldComment(Field field, IntrospectedTable introspectedTable,
                                IntrospectedColumn introspectedColumn) {
        String remarks = introspectedColumn.getRemarks();
        //根据参数和备注信息判断是否添加备注信息
        if (addRemarkComments && StringUtility.stringHasValue(remarks)) {
            addFieldJavaDoc(field, remarks);
        }
    }

    /**
     * 给model的字段添加注释
     */
    private void addFieldJavaDoc(Field field, String remarks) {
        //文档注释开始
        field.addJavaDocLine("/**");
        //获取数据库字段的备注信息
        String[] remarkLines = remarks.split(System.getProperty("line.separator"));
        for (String remarkLine : remarkLines) {
            field.addJavaDocLine(" * " + remarkLine);
        }
        addJavadocTag(field, false);
        field.addJavaDocLine(" */");
    }

}

```

 