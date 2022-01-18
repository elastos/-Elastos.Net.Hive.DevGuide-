# Build App

Once the user completes the corresponding code development, the following SDK library dependencies need to be added to the build.gradle file of the application project:

```
dependencies {
	implmentation 'org.elastos.hive.hivesdk:2.3.1'
}
```

Users are advised to use the latest version of Hive Java SDK. After configuring the Android application development environment, enter the source code engineering directory and execute the following compilation command under the Terminal command line:

```shell
$ ./gradlew build
```

After compilation, the generated apk application package is in the following project directory

```
<APP_ROOT>/app/build/outputs/apk/debug/app-debug.apk
```

Finally, install the compiled apk file into the simulator or real machine running the Android system - you'll be able to see the functions demonstrated by the sample app.
