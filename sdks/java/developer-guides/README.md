# Developer guides

Hive Java SDK 封装了 Hive Node 的 HTTP 接口，同时提供了包装好的类，供应用使用。从整体而言，Java SDK 的服务分为以下几个模块：

- 隐式的登陆机制：对于需要 Token 的接口，SDK 在调用的时候，会去检查缓存是否有 Token，没有会去执行登陆，获取到接口访问需要的 Token。
- Vault Service：基于 Vault 的服务，也是整个 Hive Node 服务的核心。
- Payment Service：支付服务，用于升级 Vault 的功能，比如：存储空间。
- Backup Service：用于备份 Vault 数据到其它的 Hive Node 上，或者从其它 Hive Node 上恢复备份好的数据。
- Promotion Service：用于将从其它 Hive Node 备份的 Vault 数据升级为一个新的 Vault，相当于进行了 Vault 迁移。

基于Vault的服务，又被分为以下几块：

- File Service：文件服务属于 Vault 服务的一部分，它通过结合 IPFS 分布式文件存储服务向用户提供文件存储空间，用户可以保存应用中用到的文件数据。
- Database Service：基于 mongodb 的数据库存储。可以保存应用的数据，比如配置信息、应用数据等。
- Scripting Service：提供了一种让访问者访问数据拥有者数据的机制。
