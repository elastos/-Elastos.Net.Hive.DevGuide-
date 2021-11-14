# Build & Install

## Build from source

Preparing with the developer tool git, and then run the following commands to clone the source:

```shell
$ git clone https://github.com/elastos/Elastos.NET.Hive.Java.SDK
$ cd Elastos.NET.Hive.Java.SDK
$ open -a Eclipse .
```

Then open the Eclipse to build the project and run the test cases. As an alternative to Eclipse, you can also use the following commands.

With the simple command below, build the whole project and run the test cases:

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

## Build API Docs

Run the command below to build the APIs document, the output of documents would be generated at doc subdirectory.

```
$ ./gradlew createjavadoc
```

Then open index.html with the browser; you can see the whole APIs document there.
## How To Use SDK

Here are examples to integrate this SDK into the projects with Gradle or Maven build system.
### Gradle

Just add the following snippet scripts into the dependencies items build.gradle:

```
implementation 'org.elastos:hive:v2.0.0'
```

### Maven

Add the following snippet of dependency declaration in their pom scripts:

```
<dependency>
	<groupId>org.elastos</groupId>
	<artifactId>hive</artifactId>
	<version>v2.0.0</version>
	<type>pom</type>
</dependency>
```

Here also is a brief guide document about How-to use the APIs. Please read it carefully and feedback as issues.
