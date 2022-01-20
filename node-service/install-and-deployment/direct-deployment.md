# Direct deployment

Hive Node 也支持以普通安装模式安装。

Hive Node also supports installation in normal installation mode.

## Requirements:

* System 要求
* System requirements
* 其他？
* Else?

## Installation

目前 Hive Node 通过脚本化运行自动安装所有Hive Node子服务：

At present, Hive Node automatically installs all Hive Node sub-services through scripted operation:

```shell
$ ./run.sh direct
```

该命令会执行如下操作：

This command will do the following:

* 以docker的方式运行mongodb服务和IPFS服务
* Run MongoDB service and IPFS service with the mode of docker.
* 直接运行Hive Node服务
* Run Hive Node service directly.
* 执行检测脚本确保Hive Node服务正常启动
* Execute the detection script to ensure that the Hive Node service starts normally.
