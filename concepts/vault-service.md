# Vault service

Vault service 是基于DID身份的数据存取service。用户通过指定DID身份在可信Hive Node上订阅Vault service后，可将第三方应用程序数据保存到该Vault service中。

Vault service is a data access service based on DID identity. Users can save the third-party application data to the Vault service after subscribing to the Vault service on the trusted Hive Node by specifying DID identity.

目前Vault Service已经支持以下数据存取服务功能：

At present, Vault Service already supports the following data access service functions:

* 文件类型数据的存取；
* 非结构化类型数据的存取；
* 通过Scripting规则自定义数据访问权限支持不同DID身份的数据交互和数据聚合。
* Access of file type data
* Access of unstructured data
* Customize data access rights through Scripting rules, to support data interaction and data aggregation of different DID identities.

## Ownership of vault data

用户基于DID身份在可信 Hive Node上 订阅创建 Vault Service，可以通过 DID 来对Vault 中数据的Ownership 确权。

Users subscribe to create Vault Service on trusted Hive Node based on DID identity, and Ownership of data in the Vault can be confirmed by DID.

## Sandboxing management

Vault service 基于用户DID和应用程序DID进行数据沙箱式管理。也就是说，不同DID身份之间，以及不同应用之间的数据存取能够互相隔离，互不干扰。

The Vault service performs data sandboxing management based on the user DID and the application DID. Data access between different DID identities and between different applications can be isolated from each other without interference.

## Vault service migration

Vault service直接对Vault数据备份到指定的Backup service，也可以将vault中数据迁移到新的可信Hive node中生成一个新的 Vault Service。

The Vault service directly backs up the Vault data to the specified Backup service, or it can migrate the data in the vault to a new trusted Hive node to generate a new Vault Service.
