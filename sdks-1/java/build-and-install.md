# Build & Install

Developers use Hive Java SDK in the following two ways:

* Use the package manager of Gradle or Maven to download the distribution package compiled by developers directly from the package repository, which is suitable for junior developers to facilitate integration
* Download the source code to build environment compilation integration

## How to Use the SDK package

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

## Build from Source

Prepare with the developer toolkit, then run the following commands to clone the source from GitHub:

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
