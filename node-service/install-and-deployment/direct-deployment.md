# Direct deployment

Hive Node 也支持以普通安装模式安装。

## Requirements:

- System 要求
- 其他？

## Installation

目前 Hive Node 通过脚本化运行自动安装所有Hive Node子服务：

```shell
$ ./run.sh direct
```

该命令会执行如下操作：

- 以docker的方式运行mongodb服务和IPFS服务
- 直接运行Hive Node服务
- 执行检测脚本确保Hive Node服务正常启动

