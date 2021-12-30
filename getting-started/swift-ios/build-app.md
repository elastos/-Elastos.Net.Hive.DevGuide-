# Build App

用户一旦完成完成相应的代码开发后，需要在应用工程的 Podfile 文件添加以下 SDK 库依赖：

```
pod 'ElastosHiveSDK', '~> 2.3.2'
```

建议用户使用最新的 Hive Swift SDK 版本。
配置完 iOS 应用开发依赖之后，进入源代码工程目录，并在Terminal命令行下执行如下编译命令

```shell
pod install
```

pod install 成功之后，打开ElastosHiveSDK.xcworkspace，选择HiveExamples target -->  选择运行真机或者模拟器 --> 点击Run --> 可以查看到示例 App 演示的功能。
