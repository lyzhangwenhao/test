### 1. 课程内容

Maven项目管理工具

Maven课程=Maven基础+Maven的高级

介绍Maven（Maven涉及到的一些概念和它的好处）

使用Maven（安装配置、创建普通Java工程和Web工程）**重点**


Maven扩展[了解]

- 生命周期
- 依赖范围
- 插件


### 2. 项目管理工具Maven


官方网址：http://maven.apache.org/download.cgi

仓库地址：https://mvnrepository.com/

Maven是一个优秀的项目构建工具，Maven提供了契约式的开发

为什么使用Maven

它可以方便将整个项目划分不同模块,模块和模块之间是有一定联系，具有依赖、聚合...等特点

它为我们的jar文件提供一个统一的仓库,极大的方便我们对Jar依赖的引入


#### 2.1. 项目构建工具帮我们解决了什么问题

- 目录结构不同，项目构建工具让我们的目录结构标准化

- 帮助我们管理项目依赖

  - 解决依赖的版本兼容问题

  - 解决依赖没有办法按需加载

- 项目模块化


#### 2.2. Maven标准目录

| 目录名称             | 描述                                                         |
| :------------------- | :----------------------------------------------------------- |
| `src/main/java`      | Application/Library sources                                  |
| `src/main/resources` | Application/Library resources                                |
| `src/main/filters`   | Resource filter files                                        |
| `src/main/webapp`    | Web application sources                                      |
| `src/test/java`      | Test sources                                                 |
| `src/test/resources` | Test resources                                               |
| `src/test/filters`   | Test resource filter files                                   |
| `src/it`             | Integration Tests (primarily for plugins)                    |
| `src/assembly`       | Assembly descriptors                                         |
| `src/site`           | Site                                                         |
| `LICENSE.txt`        | Project's license                                            |
| `NOTICE.txt`         | Notices and attributions required by libraries that the project depends on |
| `README.txt`         | Project's readme                                             |

#### 2.3. 仓库


| 仓库名称             | 作用                                                         |
| -------------------- | ------------------------------------------------------------ |
| 本地仓库             | 相当于缓存，工程第一次会从远程仓库（互联网）去下载jar 包，将jar包存在本地仓库（在程序员的电脑上）。第二次不需要从远程仓库去下载。先从本地仓库找，如果找不到才会去远程仓库找。 |
| 中央仓库（远程仓库） | 是一种远程仓库，仓库中的jar包由专业团队（maven团队）统一维护。里面存放了全世界大多数流行开源软件jar包 |
| 私服（远程仓库）     | 在公司内部架设一台私服，其它公司架设一台仓库，对外公开。     |


>    中央仓库的地址：http://mvnrepository.com/tags/maven


#### 2.4. 坐标

```
	项目名称		模块命名			版本号

	Groupid		Artifactid		SNAPSHOT(快照)	RELEASE(稳定版、发布版)
```

### 3. 1.Maven的安装

因为maven本身就是开源,所以官方提供了免安装的压缩包，解压就可以使用：下载地址：http://maven.apache.org/download.cgi

> Maven3.3.x要求JDK版本必须是1.7及以上版本

> 注意解压路径中不要出现中文

配置环境变量(不是必须的)

JDK的环境变量必须是JAVA_HOM的形式配置

如果需要在任意目录下使用maven的话,那么需要配置maven的环境

M2_HOME=D:\apache-maven-3.3.9
PATH=%M2_HOME%\bin


#### 3.1. 目录介绍

- bin：存放执行脚本文件的地方

- boot：存放一些扩展的地方

- conf：maven的核心配置文件存放的路径

- lib：maven的依赖包

#### 3.2. Maven基础设置

1. Maven默认读取的配置文件为当前用户目录(USER_HOME)/.m2文件夹下settings.xml

2. 将Maven安装目录下conf文件夹中的settings.xml复制到当前用户目录(USER_HOME)/.m2文件夹下

3. 修改配置文件：.m2/settings.xml

本地仓库存储位置：大概位于配置文件的49行

```xml
<localRepository>D:/maven-repository</localRepository>

```

添加国内镜像，大概位于配置文件的136行

```xml
<mirror>
    <id>alimaven</id>
    <name>aliyun maven</name>
    <url>http://maven.aliyun.com/nexus/content/groups/public/</url>
    <mirrorOf>central</mirrorOf>
</mirror>
```

创建Maven项目

- Java项目：maven-archetype-quickstart

- JavaWeb项目：maven-archetype-webapp

#### 3.3. 初识pom.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>

<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
  <modelVersion>4.0.0</modelVersion>

  <groupId>cc</groupId>
  <artifactId>c1</artifactId>
  <version>1.0-SNAPSHOT</version>
  <packaging>war</packaging>
  <name>c1 Maven Webapp</name>
  <properties>
    <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    <maven.compiler.source>11</maven.compiler.source>
    <maven.compiler.target>11</maven.compiler.target>
  </properties>

  <dependencies>
  </dependencies>

  <build>
  </build>
</project>
```

#### 3.4. 常用命令


| 命令        | 描述                                     |
| :---------- | :--------------------------------------- |
| mvn compile | 编译项目                                 |
| mvn test    | 编译项目并且进行单元测试                 |
| mvn clean   | 清除targe目录                            |
| mvn package | 将项目进行编译、测试、打包 jar\\war\\pom |
| mvn install | 将项目进行编译、测试、打包、发布到仓库中 |


#### 3.5. 基本标签

| 标签名称                             | 描述                                                         |
| :----------------------------------- | :----------------------------------------------------------- |
| `<modelVersion>4.0.0</modelVersion>` | maven的版本信息,固定值.不用管                               |
| `<groupId>`                          | 项目名称`<groupId>shop</groupId>`                            |
| `<artifactId>`                       | 模块的名称`<artifactId>user</artifactId>`                    |
| `<version>`                          | 项目发布的版本：测试版、快照版本、稳定版(发布版)             |
| `<packaging>`                        | 项目的打包方式：jar、war、pom                               |
| `<name>`                             | 随意                                                         |
| `<url>`                              | 仓库的地址,maven默认情况会先检查本地仓库,如果本地仓库没有所需要的jar,那么会去中央仓库下载 |
| `<dependencies>`                     | 依赖,所有的依赖包都需要写在这个标签之内                     |
| `<dependency>`                       | 具体的依赖                                                   |


> 注意：Maven在仓库中是通过坐标进行jar文件定位的

### 4. 依赖范围

| scope的取值 | 描述                                                         |
| :---------- | :----------------------------------------------------------- |
| compile     | 默认值，表示编译依赖范围。即编译、测试、运行时都需要，会被打包;默认值compile表明该jar一直全程存在/需要 |
| test        | 表示测试依赖范围。即测试时需要，编译和运行时不需要，不会被打包。比如：junit |
| provided    | 表示已提供依赖范围。即编译、测试时需要，运行时不需要，不会被打包；比如：servlet-api和jsp-api被tomcat容器提供 |
| runtime     | 表示运行时提供依赖范围。即编译时不需要，运行和测试时需要，会被打包。比如：jstl、jdbc驱动 |
| system      | system范围依赖与provided类似，但是你必须显式的提供一个对于本地系统中JAR文件的路径，需要指定systemPath磁盘路径，system依赖不推荐使用 |


### 5. 插件

Maven的核心仅仅定义了抽象的生命周期，具体的任务都是交由插件完成的，每个插件都能实现多个功能，每个功能就是一个插件目标

Maven插件可以完成一些特定的功能。例如，集成jdk插件可以方便的修改项目的编译环境；集成tomcat插件后，无需安装tomcat服务器就可以运行tomcat进行项目的发布与测试。

在pom.xml中通过plugin标签引入maven的功能插件。

```xml
<plugins>

  <!-- 打包跳过Test -->
  <plugin>
    <groupId>org.apache.maven.plugins</groupId>
    <artifactId>maven-surefire-plugin</artifactId>
    <version>2.18.1</version>
    <configuration>
      <skipTests>true</skipTests>
    </configuration>
  </plugin>

</plugins>
```

### 6. 1．生命周期

Maven生命周期就是为了对所有的构建过程进行抽象和统一

包括项目清理，初始化，编译，打包，测试，部署等几乎所有构建步骤

Maven三大生命周期,生命周期Maven有三套相互独立的生命周期

- clean：在进行真正的构建之前进行一些清理工作

- default：构建的核心部分，编译，测试，打包，部署等等

- site：生成项目报告，站点，发布站点

#### 6.1. clean生命周期


每套生命周期都由一组阶段(Phase)组成，我们平时在命令行输入的命令总会对应于一个特定的阶段。比如，运行mvn clean ，这个的clean是Clean生命周期的一个阶段。有Clean生命周期，也有clean阶段。Clean生命周期一共包含了三个阶段

- pre-clean 执行一些需要在clean之前完成的工作
- clean 移除所有上一次构建生成的文件
- post-clean 执行一些需要在clean之后立刻完成的工作

mvn clean 中的clean就是上面的clean，在一个生命周期中，运行某个阶段的时候，它之前的所有阶段都会被运行，也就是说，mvn clean 等同于 mvn pre-clean clean ，如果我们运行 mvn post-clean ，那么 pre-clean，clean 都会被运行。这是Maven很重要的一个规则，可以大大简化命令行的输入。


#### 6.2. default生命周期

Default生命周期是Maven生命周期中最重要的一个，绝大部分工作都发生在这个生命周期中。这里，只解释一些比较重要和常用的阶段：


| 命令                   | 描述                                                         |
| :--------------------- | :----------------------------------------------------------- |
| process-resources      | 复制并处理资源文件，至目标目录，准备打包。                   |
| compile                | 编译项目的源代码。                                           |
| process-test-resources | 复制并处理资源文件，至目标测试目录。                         |
| test-compile           | 编译测试源代码。                                             |
| test                   | 使用合适的单元测试框架运行测试。这些测试代码不会被打包或部署。 |
| package                | 接受编译好的代码，打包成可发布的格式，如 JAR                 |
| install                | 将包安装至本地仓库，以让其它项目依赖                         |
| deploy                 | 将最终的包复制到远程的仓库，以让其它开发人员与项目共享。     |


> - default生命周期中不包含clean,开发中如果修改代码后打包运行没有更改的效果,需要重新执行clean命令
> - 运行任何一个阶段的时候，它前面的所有阶段都会被运行

#### 6.3. site生命周期

pre-site 执行一些需要在生成站点文档之前完成的工作

site 生成项目的站点文档

post-site 执行一些需要在生成站点文档之后完成的工作，并且为部署做准备

site-deploy 将生成的站点文档部署到特定的服务器上

这里经常用到的是site阶段和site-deploy阶段，用以生成和发布Maven站点

