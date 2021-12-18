# Deploy as Docker containers

随着 Docker 技术的不断成熟，基于 Docker 环境的服务部署已成为一种标准化的部署方式。Hive Node 首先提供基于 Docker 的部署模式，由于其内部包含三个子服务，Docker 方式部署后会形成三个Docker Container 服务进程。

## Requirements

 - Docker 安装；
 - Python 环境安装
 - 其他工具 ？

原则上 Hive Node 可以部署在任何 Linux 系统中，但是目前 Hive Node 团队仅在 Ubuntu Linux 22.04 环境验证，请社区用户安装 Hive Node 时尽量考虑选择在 Ubuntun Linux 中环境部署运行。

## Deployment

目前 Hive Node 通过脚本化运行自动安装所有Hive Node子服务：

```shell
$ ./run.sh docker
```

该命令会执行如下操作：

- 以docker的方式运行mongodb服务和IPFS服务
- 以docker的方式运行Hive Node服务
- 执行检测脚本确保Hive Node服务正常启动
