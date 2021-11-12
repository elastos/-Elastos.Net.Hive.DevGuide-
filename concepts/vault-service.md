# Vault service

Vault service 是基于通过指定DID身份的数据存取service。用户通过指定DID身份在可信Hive Node上订阅Vault service后，可将第三方应用程序数据保存到该Vault service中。

目前Vault Service已经支持以下特性的数据存取服务：

* 文件类型数据的存取；
* 非结构化类型数据的存取；
* 通过Scripting规则自定义数据访问权限支持不同DID身份的数据交互和数据聚合。

## Sandboxing vault service

Vault service 基于用户DID和应用程序DID进行数据沙箱管理。也就是说，不同DID身份之间，以及不同应用之间的数据存取能够互相隔离，互不干扰。

## Vault service migration

Vault service直接对Vault数据备份到指定的Backup service，也可以将vault service 迁移到新的可信Hive node中。
