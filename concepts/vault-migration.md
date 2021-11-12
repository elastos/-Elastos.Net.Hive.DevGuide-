# Vault migration

Vault migration 即Vault service migration，具体是指用户将指定DID身份的vault service 从源Hive Node迁移到目标可信Hive node节点上。

Hive 支持两种以下两种方式的Vault service migration：

* 通过先将vault 数据备份到指定的Backup Service中，同时在新的可信Hive Node注册新的Vault service，然后从目标Backup Service中同步vault 备份数据，从而恢复新的Vault service；
* 通过将vault 数据备份到指定的Backup service中，直接将该Backup Service提升为Vault service

需要注意的是，一旦开始迁移vault service，为保持vault 数据的一致性，必须将原vault service停止服务，然后在迁移完成后，再开启新的vault service。开启新的Vault service后，需要更新该DID文档中的vault service入口地址并更新公布到EID中。
