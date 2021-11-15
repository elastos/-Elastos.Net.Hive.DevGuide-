# Build App

Hive Java SDK提供了一个Application Demo，展示了如何在android应用中使用SDK。其位于如下位置：

- <SOURCE_ROOT>/samples/hellosdk

若要编译此app，首先需要准备android开发环境，具体可以参考这里：

- https://developer.android.com/docs

配置好android开发环境之后，只需进入源代码目录，然后执行如下命令：

- ./gradlew build

编译完成后，生成的apk文件位于如下位置：

- <APP_ROOT>/app/build/outputs/apk/debug/app-debug.apk
