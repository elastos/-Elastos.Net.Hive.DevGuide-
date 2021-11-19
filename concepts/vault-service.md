# Vault service

Vault service 是基于DID身份的数据存取service。用户通过指定DID身份在可信Hive Node上订阅Vault service后，可将第三方应用程序数据保存到该Vault service中。

目前Vault Service已经支持以下数据存取服务功能：

* 文件类型数据的存取；
* 非结构化类型数据的存取；
* 通过Scripting规则自定义数据访问权限支持不同DID身份的数据交互和数据聚合。

## Ownership of vault data

用户基于DID身份在可信 Hive Node上 订阅创建 Vault Service，可以通过 DID 来对Vault 中数据的Ownership 确权。

## Sandboxing management

Vault service 基于用户DID和应用程序DID进行数据沙箱式管理。也就是说，不同DID身份之间，以及不同应用之间的数据存取能够互相隔离，互不干扰。

## Vault service migration

Vault service直接对Vault数据备份到指定的Backup service，也可以将vault中数据迁移到新的可信Hive node中生成一个新的 Vault Service。
