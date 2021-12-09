---
description: >-
  Users are free to set up their own Hive node or choose a trusted hive node to
  subscribe to the vault service.
---

# Vault selection

Hive是一个开放的去中心化存储网络，任何结构和个人都可以参与搭建部署Hive Node节点并自行运营。Hive项目有计划推出经济激励机制来鼓励社区用户和机构部署Hive Node，并从运营Hive Node中获取利润回报，从而形成基于Hive网络的节点经济和应用生态。

普通用户可凭DID身份选择在可信Hive Node节点上订阅Vault服务，进而存取应用数据到该vault中。用户也可以自己搭建Hive Node，相信自己搭建的Hive Node并提供给自己的DID使用。

用户根据DID身份在信任的Hive Node上订阅vault后，需要将该vault service入口声明绑定到目标DID文档中，同时在EID侧链上进行更新。这样其他用户可以根据目标DID来解析发现对应的vault入口，从而参与不同用户间的数据交互，拓广数据存取业务场景。

### Users have choices to migrate Vault service

用户一旦在指定的Hive Node节点上订阅Vault服务后，并不表示该Vault服务与对应Hive Node永久绑定，不能进行任何的数据迁移或者删除。用户在指定Hive Node上使用vault服务体验过程中，一旦出现以下情况时：

* Hive节点运营者告知将停止运营该Hive Node节点
* 对应承载的Hive节点未能再提供很好的数据访问能力
* 用户自己搭建了Hive Node节点

用户可以将选择将已有的Vault迁移到指定新的可信Hive节点中。迁移完成后，同步更新DID文档中对应的vault服务入口。整个迁移过程中除了中间Vault会停止服务一段时间，其他更新过程对第三方应用完全透明。

Hive Node节点无权限制或者审核已订阅Vault的DID身份使用该Hive Node。Vault中存储的数据原则上对Hive Node服务运营者透明。一旦用户完成迁移vault数据后，用户有权删除在原来Hive Node存储的所有历史数据。
