# Deploy as Docker containers

若要以docker的方式运行，请切换至源代码目录下，执行如下命令：

```shell script
./run.sh docker
```

该命令会执行如下操作：

- 以docker的方式运行mongodb服务和IPFS服务
- 以docker的方式运行Hive Node服务
- 执行检测脚本确保Hive Node服务正常启动
