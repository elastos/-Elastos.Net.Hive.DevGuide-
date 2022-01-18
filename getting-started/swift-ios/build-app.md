# Build App

Once the user completes the corresponding code development, the following SDK library dependencies need to be added to the Podfile file of the application project:

```
pod 'ElastosHiveSDK', '~> 2.3.2'
```

Users are advised to use the latest version of Hive Swift SDK. After configuring the iOS application development dependency, enter the source code engineering directory and execute the following compilation command under the Terminal command line:

```shell
pod install
```

After the success of pod install, open the ElastosHiveSDK.xcworkspace, select HiveExamples target -- > Select to run the real machine or simulator-> Click Run -- > to view the functions demonstrated by the sample app.
