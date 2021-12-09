# Build & Install

开发者通过以下两种方式使用Hive Java SDK：

* 使用 Gradle 或者 Maven 的包管理器，直接从 package repository 中下载由开发者编译的分发包，适合于初级开发者便利集成
* 下载源代码来构建环境编译集成

## How To Use SDK package

Here are examples to integrate this SDK into the projects with Gradle or Maven build system.

### Maven

Add the following snippet of dependency declaration in their pom scripts:

```
<dependency>
	<groupId>org.elastos</groupId>
	<artifactId>hive</artifactId>
	<version>2.3.1</version>
	<type>pom</type>
</dependency>
```

### Glide

Just add the following snippet scripts into the dependency item in build.gradle:

```
dependencies {
	implmentation 'org.elastos.hive.hivesdk:2.3.1'
}
```

## Build from source

Preparing with the developer tool git, and then run the following commands to clone the source from github:

```shell
$ git clone https://github.com/elastos/Elastos.NET.Hive.Java.SDK
$ cd Elastos.NET.Hive.Java.SDK
```

Then open the Eclipse to build the project and run the test cases. As an alternative to Eclipse, you can also use the following commands.

With the simple command below, build the whole project and run the test cases from terminal: 

```
$ ./gradlew build
```

With the option -x , try to build the project without running the test cases.

```
$ ./gradlew build -x test
```

Or just directly run the test cases:

```
$ ./gradlew test
```