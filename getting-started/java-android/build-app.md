# Build App

用户一旦完成完成相应的代码开发后，需要在应用工程的 build.gradle 文件添加以下 SDK 库依赖：

Once the user completes the corresponding code development, the following SDK library dependencies need to be added to the build.gradle file of the application project:

```
dependencies {
	implmentation 'org.elastos.hive.hivesdk:2.3.1'
}
```

建议用户使用最新的 Hive Java SDK 版本。 配置完 Android 应用开发环境之后，进入源代码工程目录，并在Terminal命令行下执行如下编译命令

Users are advised to use the latest version of Hive Java SDK. After configuring the Android application development environment, enter the source code engineering directory, and execute the following compilation command under the Terminal command line

```shell
$ ./gradlew build
```

编译完成后，生成的apk 应用包在以下工程目录中

After compilation, the generated apk application package is in the following project directory

```
<APP_ROOT>/app/build/outputs/apk/debug/app-debug.apk
```

最后，将编译好的 apk 文件安装至运行 Android 系统的模拟器或真机，可以查看到示例 App 演示的功能。

Finally, install the compiled apk file into the simulator or real machine running Android system, and you can see the functions demonstrated by the sample App.
