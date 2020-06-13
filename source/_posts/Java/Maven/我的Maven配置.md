---
title: 我的Maven配置
date: 2020-05-31 14:23:31
updated: 2020-05-31 14:23:31
tags: [Maven, 镜像仓库]
category: 
- [Java, Maven] 
---

这一篇是我自用的Maven开发环境配置经历，我将此次开发环境的配置记录下来，用作备忘。
<!-- more -->

### 1. 工具介绍

- 操作系统： **Windows 10 pro**
- Jdk版本： **Oracle JDK 1.8.0_241**
- IDE： **IDEA 2019.3.2**
- Maven版本：**3.6.1（IDEA自带）**

### 2. 配置介绍

`Maven`的配置文件位于`Maven`文件夹的`conf/settings.xml`，在这部分只介绍此次需要修改的配置。

#### 2.1 本地仓库

在`settings.xml`中有关于**本地仓库**设置的描述如下。

```xml
	<!--
	localRepository
	| The path to the local repository maven will use to store artifacts.
	|
	| Default: ${user.home}/.m2/repository
	<localRepository>/path/to/local/repo</localRepository>
	-->
```
这部分的内容是说：
1. **需要设置一个存储库的路径，Maven会用设置的这个存储库存储下载下来的依赖项。**
    > The path to the local repository maven will use to store artifacts.
2. **默认的路径是在用户个人目录下的一个隐藏文件夹`.m2/repository`。** 
    > Default: ${user.home}/.m2/repository
    
    具体位置为:
   - `Windows`系统：`C:\User\用户名\.m2\repository\`
   - `Linux`系统: `/home/用户名/.m2/repository/`
3. 通过`<localRepository>`标签设置本地仓库的路径。
   > `<localRepository>/path/to/local/repo</localRepository>`

#### 2.2 国内镜像源

在`settings.xml`中有关于**镜像源**设置的描述如下。

```xml
	<mirrors>
		<!--
		mirror
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
	</mirrors>
```

这部分的内容是：
1. **指定一个仓库镜像地址来代替默认的仓库地址。**
	> Specifies a repository mirror site to use instead of a given repository.
2. **这份仓库的镜像服务的ID必须和`<mirrorOf>`元素相匹配。**
	> The repository that this mirror serves has an ID that matches the mirrorOf element of this mirror.
3. **镜像的ID在一组中必须是独一无二的，似乎是直接查找或者项目被继承时候需要用到（关于被继承的东西是什么目前不确定，应当是这个Maven项目吧）。**
   > IDs are used for inheritance and direct lookup purposes, and must be unique across the set of mirrors.

### 3. 修改配置

#### 3.1 设置本地仓库

在`settings.xml`中加入
```xml
	<localRepository>
		E:\MyRepository\Maven\localRepo
	</localRepository>
```
***注意：*** 这个路径应该是某个文件夹的绝对路径。

#### 3.2 设置阿里`Maven`镜像源

在`settings.xml`的`<mirrors></mirrors>`标签中加入阿里`Maven`镜像信息。

加入之后应该是这样：

```xml
	<mirrors>
		<mirror>
			<id>alimaven</id>
			<name>aliyun maven</name>
			<url>
				http://maven.aliyun.com/nexus/content/groupspublic/
			</url>
			<mirrorOf>
				central
			</mirrorOf>
		</mirror>
	</mirrors>
```

***注意：*** 还可以在`<mirrors>`标签中加入其他的镜像信息。

#### 3.3. 应用修改

**这一部分的操作是可选的。**

可以将修改后的`settings.xml`复制一份放在当前用户的`.m2`文件夹下，因为IDEA默认的`Maven`设置会引用`.m2`文件夹下的`settings.xml`，如果没有才回去使用默认的。设置如下图。

![IDEA默认的Maven路径设置](https://raw.githubusercontent.com/LeoFeng233/PicBed/master/20200202015003.png?token=AG766L44EISRUFAWMVU6JN26GW5IU)

之所以说这一步是可选的是因为除了复制`settings.xml`这个方法之外还可以修改通过勾选`User settings file`选项最后面的`Override`框直接选择自己的`settings.xml`.甚至可以直接更换本地仓库或者使用自己下载配置的`Maven`来替换IDEA自带的`Maven`。

以上。