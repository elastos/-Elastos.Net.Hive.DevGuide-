# Introduction

Hive Node 服务节点是Hive去中心化存储的服务节点，并需要24\*7常驻运行通过前端SDKs向用户和应用提供数据存取服务。Hive Node是开放的源代码项目，任何社区成员和组织根据安装指南进行部署运行。

所有运行的Hive Node服务节点构成整个Hive存储网络。

Hive Node运行由以下一组子服务组成：

* Http Service
* IPFS node
* MongoDB service

其中Http Servic负责与前端SDs通过Http request/response进行数据存取请求。Hive服务会将文件数据存储到本地的IPFS Node，而将数据库交由MongoDB服务负责存储。


本章主要介绍社区人员和组织如何搭建部署Hive 服务节点：
* 节点配置
* 节点部署

以及提供Restful APIs接口文档，供前端开发人员根据Http接口SDKs库
* HTTP API Reference.
