---
title: Maven笔记
tags: Maven
category: [Java, Maven]
date: 2020-05-27 00:48:29
updated: 2020-05-27 00:48:29
---


重新学习`Maven`，顺便记录一下自己的收获。

<!-- more -->

### 1. `Maven`是什么？

在百度搜索 
>**`Maven`** 是什么？

在`Maven`的[百度百科](https://baike.baidu.com/item/Maven/6094909?fr=aladdin)里是这么描述的:
>Maven项目对象模型(POM)，可以通过一小段描述信息来管理项目的构建，报告和文档的项目管理工具软件。

emmmmmmmmmm，尴尬...这段信息，我只看懂了最后面几个字-**项目管理工具**，但是，在目前的使用中，我一直都是把它当作一个**项目构建工具**，就是通过在`pom.xml`中修改依赖，让`Maven`自动帮我下载各种`jar`包，所以，在目前这个阶段，我就理解`Maven`是用来下载各种`jar`包的**项目构建工具**。

### 2. 为什么要用`Maven`?

这个问题似乎两个字就可以回答了，**好用**！但是，新的问题出现了，它为什么好用？说来话长。一切还要从`jvm`讲起。

#### 2.1. `jvm`完成了哪些工作？
`JVM`，全程`Java Virtual Machine`，顾名思义，它是一个虚拟机，它负责执行编译产物`.class`文件。在执行的过程中，如果发现未曾使用过的类，`JVM`会先去读取并加载这个类，然后再去执行。

#### 2.2. `JVM`从何处读取新的类

当使用命令行运行`Java`代码时，总是要使用`-classpath`参数指定类的路径，在Idea中写了一段`Hello World`：
```java
public class HelloWorld {
    public static void main(String[] args) {
        System.out.println("Hello World");
    }
}
```
在`IDEA`中运行时，输出中会有以下信息：
```bash
"C:\Program Files\Java\jdk-11.0.7\bin\java.exe" 
-javaagent:C:\Users\55393\AppData\Local\JetBrains\Toolbox\apps\IDEA-U\ch-0\201.7223.91\lib\idea_rt.jar=58484:C:\Users\55393\AppData\Local\JetBrains\Toolbox\apps\IDEA-U\ch-0\201.7223.91\bin -Dfile.encoding=UTF-8 
-classpath C:\Users\55393\IdeaProjects\MavenDemo\target\classes HelloWorld
```

可以看到，在运行这段代码的时候，`IDEA`自动地用`-classpath`参数指定了项目目录下`target\classes`的文件夹，于是，`JVM`在执行这段代码的时候就会从`target\classes`目录下去找这个类。

这时，我在这个`Maven`项目中引入了一个`spring-boot`的`jar`包。
```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter</artifactId>
    <version>2.3.0.RELEASE</version>
</dependency>
```
再次运行上面的代码 

```bash
"C:\Program Files\Java\jdk-11.0.7\bin\java.exe" 
-javaagent:C:\Users\55393\AppData\Local\JetBrains\Toolbox\apps\IDEA-U\ch-0\201.7223.91\lib\idea_rt.jar=63376:C:\Users\55393\AppData\Local\JetBrains\Toolbox\apps\IDEA-U\ch-0\201.7223.91\bin -Dfile.encoding=UTF-8 -classpath E:\MyRepository\Code\IdeaProjects\MavenDemo\target\classes;
E:\MyRepository\Maven\LocalRepository\org\springframework\boot\spring-boot-starter\2.3.0.RELEASE\spring-boot-starter-2.3.0.RELEASE.jar;
E:\MyRepository\Maven\LocalRepository\org\springframework\boot\spring-boot\2.3.0.RELEASE\spring-boot-2.3.0.RELEASE.jar;
E:\MyRepository\Maven\LocalRepository\org\springframework\spring-context\5.2.6.RELEASE\spring-context-5.2.6.RELEASE.jar;
E:\MyRepository\Maven\LocalRepository\org\springframework\spring-aop\5.2.6.RELEASE\spring-aop-5.2.6.RELEASE.jar;
E:\MyRepository\Maven\LocalRepository\org\springframework\spring-beans\5.2.6.RELEASE\spring-beans-5.2.6.RELEASE.jar;
E:\MyRepository\Maven\LocalRepository\org\springframework\spring-expression\5.2.6.RELEASE\spring-expression-5.2.6.RELEASE.jar;
E:\MyRepository\Maven\LocalRepository\org\springframework\boot\spring-boot-autoconfigure\2.3.0.RELEASE\spring-boot-autoconfigure-2.3.0.RELEASE.jar;
E:\MyRepository\Maven\LocalRepository\org\springframework\boot\spring-boot-starter-logging\2.3.0.RELEASE\spring-boot-starter-logging-2.3.0.RELEASE.jar;
E:\MyRepository\Maven\LocalRepository\ch\qos\logback\logback-classic\1.2.3\logback-classic-1.2.3.jar;
E:\MyRepository\Maven\LocalRepository\ch\qos\logback\logback-core\1.2.3\logback-core-1.2.3.jar;
E:\MyRepository\Maven\LocalRepository\org\slf4j\slf4j-api\1.7.25\slf4j-api-1.7.25.jar;
E:\MyRepository\Maven\LocalRepository\org\apache\logging\log4j\log4j-to-slf4j\2.13.2\log4j-to-slf4j-2.13.2.jar;
E:\MyRepository\Maven\LocalRepository\org\apache\logging\log4j\log4j-api\2.13.2\log4j-api-2.13.2.jar;
E:\MyRepository\Maven\LocalRepository\org\slf4j\jul-to-slf4j\1.7.30\jul-to-slf4j-1.7.30.jar;
E:\MyRepository\Maven\LocalRepository\jakarta\annotation\jakarta.annotation-api\1.3.5\jakarta.annotation-api-1.3.5.jar;
E:\MyRepository\Maven\LocalRepository\org\springframework\spring-core\5.2.6.RELEASE\spring-core-5.2.6.RELEASE.jar;
E:\MyRepository\Maven\LocalRepository\org\springframework\spring-jcl\5.2.6.RELEASE\spring-jcl-5.2.6.RELEASE.jar;
E:\MyRepository\Maven\LocalRepository\org\yaml\snakeyaml\1.26\snakeyaml-1.26.jar HelloWorld
```

只导入了一个`jar`包，为什么`-classpath`会多了这么多参数？这是因为，导入的`spring-boot-starter`这个包的代码实现上有很多依赖，当`spring-boot-starter`被使用时，必须将它依赖的所有包都导入，而被`spring-boot-starter`依赖的包也可能依赖许多其它的`jar`包，而其也会被一并导入，这样就造成了，虽然只是手动导入了一个包，但是实际上却导入了不止一个包的情况。

这种A包依赖了B包，B包依赖了C包，而导致在导入A包时，B包，C包被一起导入的情况称作**传递依赖**。

在以前没有任何构建工具的时候，开发者们都是通过手写`-classpath`参数来编译运行代码的。想想真是让人头大，于是，后来出现了`apache ant`的构建工具，开发者需要手动到各种官网下载`jar`包，然后写一堆`xml`指定源码路径，以及编译的输出路径，起到简化流程的作用（应该是这样吧......）。但是，这个工具还是有问题，如果漏了`jar`包，还是很麻烦。

于是，`Maven`出现了，只需少量的代码，就能自动将需要的`jar`包，以及被依赖的`jar`包自动下载下来并导入。一站式服务，非常贴心。

### 3. `Maven`的约定

#### 3.1 项目目录结构 

为了更好地复用其他人的代码，或者使其他人复用自己的代码，`Maven`约定了一套项目目录。
- `src`
  - `main`
    - `java`：存放项目代码
    - `resources`：项目资源文件
  - `test`
    - `java`：测试代码文件夹
    - `resources`：测试资源文件夹
- `pom.xml`

#### 3.2 包的命名

`Maven`有一个**中央仓库**，它存储着绝大多数的`jar`包，需要的时候就从中央仓库下载。为了对需要的`jar`包进行定位，需要对所有包进行编号。所有的包都要有以下属性
- `groupId`：项目组织唯一的标识符，实际对应JAVA的包的结构，是main目录里java的目录结构
- `artifactId`：项目的唯一的标识符，实际对应项目的名称，就是项目根目录的名称
- `version`：包的版本号。

当需要某个`jar`包时，`Maven`会根据`groupId/artifactId/version`的格式在中央仓库中定位。例如，从`Maven`中央仓库中访问`spring-boot-starter`这个包2.3版本的资源时，应该访问[https://repo1.maven.org/maven2/org/springframework/boot/spring-boot-starter/2.3.0.RELEASE/](`https://repo1.maven.org/maven2/org/springframework/boot/spring-boot-starter/2.3.0.RELEASE/`)，正如前面在引入这个包的时候，也有`groupId/artifactId/version`等属性，这样才能准确定位一个包。

#### 3.3 `Maven`还有本地仓库

当在`Maven`项目中引入一个依赖时，`Maven`会在本地仓库寻找对应的`jar`包，如果没有才会去中央仓库中寻找这个包，找到之后会下载到本地仓库中，以便断网之后还能继续使用。

### 4. `Maven`解决冲突

当在项目中引入不同的`jar`包，而这些`jar`包有出现了传递依赖的现象，以至于，这些依赖的`jar`包中有可能会出现同名包的不同版本，这些同名包的不同版本就出现了冲突，这些不同版本的`jar`包对于`api`的定义和实现可能会有出入，导致项目出现bug。

#### 4.1 同样的包不允许出现在`-classpath`两次

当项目依赖中出现了同名包的不同版本，`Maven`只会留下其中一个，因为`JVM`在根据类的全限定名找到一个类时，就不会再继续在后面的参数中去寻找这个类了，这也是造成冲突的根本原因。

#### 4.2 `Maven`如何处理冲突

`Maven`会根据`jar`包的距离项目的路径长度来决定留下哪个包。例如，某个项目中引入了**A包**和**E包**，它们都依赖了**C包**。这两个包各自的依赖为:
- A:0.1
  - B:0.1
    - C:0.2
- E:0.1
  - C:0.1

项目引用`C:0.2`包经历了三级依赖，引用`C:0.1`包经历了二级依赖，由于引用`C:0.1`包的路径更短，所以，`Maven`在处理这个冲突时，会留下`C:0.1`包，去掉`C:0.2`包。

还有一种情况是，冲突的包距离项目的路径长度相同，这种情况下，会留下更早引入的包。还是前面的例子，但是如果依赖关系变成了这样:
- A:0.1
  - C:0.1
- E:0.1
  - C:0.2

项目引用`C:0.1`和`C:0.2`都经历了二级依赖，距离项目的路径相同，这时候，由于项目引用`A:0.1`在`E:0.1`之前，所以`Maven`会留下`C:0.1`包，去掉`C:0.2`包。

#### 4.3 手动处理冲突

`Maven`默认的处理冲突的方式有可能会引起很多问题，例如在前面提到的例子中，`Maven`把是新版本，但是项目依赖路径较长的包给干掉了，这就可能导致一些已经在使用中的，而且是发布在新版本中的`api`无法使用了，从而导致出现了bug。所以在这个时候，就需要手动处理依赖冲突。

手动处理依赖冲突有两种方法：

##### 4.3.1 直接在项目中引用有争议的依赖

直接在项目中引用依赖，那么依赖的路径必定是短于其它的传递依赖的，所以，其它的有争议的依赖版本就会被`Maven`自动屏蔽。同样是上面提到的例子，依赖关系为:
- A:0.1
  - B:0.1
    - C:0.2
- E:0.1
  - C:0.1

**C包**的版本有了争议，这时，可以直接在项目中引用C包，使得项目直接依赖：
- A:0.1
- E:0.1
- C:0.2

此时，`Maven`就会干掉被**A包**和**E包**传递依赖的`C:0.1`包和`C:0.2`，只留下项目直接依赖的`C:0.2`：
- A:0.1
  - B:0.1
    - ~~C:0.2~~
- E:0.1
  - ~~C:0.1~~
- C:0.2

##### 4.3.2 去掉不需要的循环依赖

通过手动配置来告诉`Maven`，将传递依赖去掉，来避免依赖冲突。举个例子，有依赖关系如：
- A:0.1
  - B:0.1
    - C:0.2
- E:0.1
  - C:0.1

在引入依赖**E包**的时候，手动去掉`C:0.1`：
```xml
<dependency>
    <groupId>library</groupId>
    <artifactId>E</artifactId>
    <version>0.1</version>
    <exclusions>
        <exclusion>
            <groupId>library</groupId>
            <artifactId>C</artifactId>
        </exclusion>
    </exclusions>
</dependency>
```
这时，`Maven`在引入**E包**的时候就不会引入**C包**了，依赖关系就会变成：
- A:0.1
  - B:0.1
    - C:0.2
- E:0.1
  - ~~C:0.1~~

### 5. 依赖作用范围

在引入依赖的时候可以通过加入一对`<scope><scope/>`标签限制依赖的作用范围，依赖作用范围主要是以下三种:
1. `test`：将作用范围限制在测试代码中，正常的项目代码无法使用。
2. `provided`：可以在测试或者项目代码中使用，但是仅能在编译中使用，无法在项目运行时使用。
3. `compile`：可以在测试或者项目代码中使用，也可以在编译或运行中使用

以上，就是这一篇笔记的全部内容。