# Install & Deployment

Hive Node Service源代码是基于Python的，它本身依赖于2个服务：

- Mongodb Service：Mongodb数据库，为Hive Node提供数据库服务
- IPFS Service：为Hive Node提供文件存储服务

Mongodb Service和IPFS Service都是以docker的方式部署的，而Hive Node Service可以直接运行也可以以docker的方式运行，这两种部署方式都被设置在脚本run.sh中。
