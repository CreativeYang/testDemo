# 微服务工程

此工程是为了让开发人员快速搭建可用的JCF 微服务所生成的Maven聚合工程，其主要包括`parent`,`dependencies`、`common`和`server`。

![img](http://media.teamshub.com/10000/tm/2020/06/03/949e0351-fae7-4b49-a4a6-48493a737264/b93dd0be-22df-4f47-9638-93ad840945d0.png)


- parent:所有模块的父工程，父工程依赖`ijcf-boot-parent`,管理了jcf的版本
- dependencies：所有子模块的版本控制
- common：公共模块，添加公共依赖或者实体Bean和工具类
- server：微服务模块
目录结构如下

```
crm-base-parent
|   .gitignore
|   HELP.md
|   mvnw
|   mvnw.cmd
|   pom.xml
+---.mvn
|   \---wrapper
|           maven-wrapper.jar
|           maven-wrapper.properties
|           MavenWrapperDownloader.java
+---crm-base-common
|   |   pom.xml
|   |
|   \---src
|       +---main
|       |   \---java
|       |       \---com
|       |           \---sitech
|       |               \---common
|       |                       CrmBaseCommonApplication.java
|       \---test
|           \---java
|               \---com
|                   \---sitech
|                       \---common
|                               CrmBaseCommonApplicationTests.java
+---crm-base-dependencies
|       pom.xml
\---crm-base-server
    |   pom.xml
    \---src
        +---main
        |   +---java
        |   |   \---com
        |   |       \---sitech
        |   |           \---server
        |   |                   CrmBaseServerApplication.java
        |   \---resources
        |       |   application.properties
        |       +---static
        |       \---templates
        \---test
            \---java
                \---com
                    \---sitech
                        \---server
                                CrmBaseServerApplicationTests.java
```


## parent 

- jcf版本控制

`parent`工程是所有模块的父工程，同时其继承自`ijcf-boot-parent`，所以其子模块中的jcf依赖均受到`ijcf-boot-parent`的版本号控制。
pom.xml:
```xml
<parent>
    <groupId>com.sitech.ijcf</groupId>
    <artifactId>ijcf-boot-parent</artifactId>
    <version>3.3.0.SNAPSHOT</version>
    <relativePath/> <!-- lookup parent from repository -->
</parent>
```
所有子模块的jcf版本即为`3.3.0.SNAPSHOT`  


- 模块管理  
   
父工程各模块的管理，新建模块需要添加新的工程
```xml
<modules>
    <module>crm-base-dependencies</module>
    <module>crm-base-common</module>
    <module>crm-base-server</module>
</modules>
```



## dependencies 版本控制

dependencies模块的主要功能是作为统一的版本依赖，微服务项目中各个模块依赖较多，大量的依赖版本号在更新迭代过程中难免会出现版本冲突问题。dependencies 为各个模块做出了版本管理，如果希望更改某个依赖的版本版本只需要更改`denpendencies`中的版本号即可。
例如：dependencies/pom.xml


```xml
<properties>
    <spring-cloud.version>Hoxton.SR5</spring-cloud.version>
    <fastjson.version>1.2.60</fastjson.version>
    <druid.version>1.1.12</druid.version>
</properties>

<dependencyManagement>
    <dependencies>
      <dependency>
        <groupId>org.springframework.cloud</groupId>
        <artifactId>spring-cloud-dependencies</artifactId>
        <version>${spring-cloud.version}</version>
        <type>pom</type>
        <scope>import</scope>
      </dependency>
      <dependency>
        <groupId>com.alibaba</groupId>
        <artifactId>fastjson</artifactId>
        <version>${fastjson.version}</version>
      </dependency>
      <dependency>
        <groupId>com.alibaba</groupId>
        <artifactId>druid</artifactId>
        <version>${druid.version}</version>
      </dependency>
    </dependencies>
</dependencyManagement>

```

在server 微服务模块中引入依赖,则不需要添加版本。如果想用使用不同版本，可添加版本号，maven就近原则会进行版本覆盖
  
server/pom.xml
```xml
<dependencies>
  <dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-config-server</artifactId>
  </dependency>
  <dependency>
    <groupId>com.alibaba</groupId>
    <artifactId>fastjson</artifactId>
  </dependency>
  <!-- maven就近原则进行版本覆盖 -->
  <dependency>
    <groupId>com.alibaba</groupId>
    <artifactId>druid</artifactId>
    <version>1.1.6</version>
  </dependency>
</dependencies>
```

## Common
common模块主要是添加公共依赖和添加一些公共类，然后微服务工程添加common依赖。

## Server
服务模块



