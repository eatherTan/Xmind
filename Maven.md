# Maven
## Maven的作用：

1. **解决依赖管理：**

   在没有maven的时候，如：项目需依赖abc 这个jar包，但是abc jar包又依赖xyz这个jar包。如果使用了maven，那么我们就不需要知道jar之间的依赖，maven会自动导入abc的jar包，再判断abc需要xyz，又会自动导入xyz的jar包，这样，最终我们的项目会依赖abc和xyz两个jar包

复杂依赖的示例：

```
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
    <version>1.4.2.RELEASE</version>
</dependency>
```

当我们项目要使用到 `spring-boot-starter-web`，maven会自动解析并判断所需依赖，最终需要二三十个其他依赖

```
spring-boot-starter-web
  spring-boot-starter
    spring-boot
    sprint-boot-autoconfigure
    spring-boot-starter-logging
      logback-classic
        logback-core
        slf4j-api
      jcl-over-slf4j
        slf4j-api
      jul-to-slf4j
        slf4j-api
      log4j-over-slf4j
        slf4j-api
    spring-core
    snakeyaml
  spring-boot-starter-tomcat
    tomcat-embed-core
    tomcat-embed-el
    tomcat-embed-websocket
      tomcat-embed-core
  jackson-databind
  ...
```

## 依赖关系

Maven定义了几种依赖关系，分别是`compile`、`test`、`runtime`和`provided`：其中，`compile`是最常用的，maven会把这种类型的依赖直接放入到classpath。

| scope    | 说明                                            | 示例            |
| -------- | ----------------------------------------------- | --------------- |
| compile  | 编译时需要用到该jar包（默认）                   | commons-logging |
| test     | 编译Test时需要用到该jar包                       | junit           |
| runtime  | 编译时不需要，但是运行时需要用到                | mysql           |
| provided | 编译时需要用到，但是运行时由JDK或者某个服务提供 | servlet-api     |

示例：`runtime`依赖

```
<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
    <version>5.1.48</version>
    <scope>runtime</scope>
</dependency>
```



提问：如果不写依赖关系，默认是什么？如表格所示，默认是`compile`

## Maven生命周期

使用maven需要先了解maven的生命周期是什么

maven的生命周期由一系列阶段phrase构成，以内置的生命周期`default`为例，包含了以下phrase

> - validate
> - initialize
> - generate-sources
> - process-sources
> - generate-resources
> - process-resources
> - compile
> - process-classes
> - generate-test-sources
> - process-test-sources
> - generate-test-resources
> - process-test-resources
> - test-compile
> - process-test-classes
> - test
> - prepare-package
> - package
> - pre-integration-test
> - integration-test
> - post-integratiob-test
> - verify
> - install
> - deploy

如果我们运行 `mvn package`，maven就会执行`default`生命周期，它会从开始运行到`package`为止：validate ... 



> 经常会用到Maven，但是对于maven常用到的指令，不明白有什么功能。特此来解析maven的指令

## Maven指令

### 1.maven install 安装指令
maven install 做了两件事情：
1. 将项目打包（jar/war）,将打包结果放到项目下的target目录下
2. 同时将上述打包结果放到本地仓库的相应目录中，供其他项目或模块引用


### 2.Maven package 打包指令
maven package，就做了一件事：

1. 将项目打包（jar/war），将打包结果放到项目下的 target 目录下