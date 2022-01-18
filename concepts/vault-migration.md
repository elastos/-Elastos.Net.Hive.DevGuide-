# Vault migration

Vault migration 即Vault service migration，具体是指用户将指定DID身份的vault service 从源Hive Node迁移到目标可信Hive node节点上。

Vault migration is the Vault service migration, which specifically refers to that the user migrates the vault service with the specified DID identity from the source Hive Node to the target trusted Hive node.

Hive 支持两种以下两种方式的Vault service migration：

Hive supports Vault service migration in the following two ways:

* 通过先将vault 数据备份到指定的Backup Service中，同时在新的可信Hive Node注册新的Vault service，然后从目标Backup Service中同步vault 备份数据，从而恢复新的Vault service；
* 通过将vault 数据备份到指定的Backup service中，直接将该Backup Service提升为Vault service
* The new Vault service can be restored by backing up the vault data to the designated Backup Service, registering a new Vault service in the new trusted Hive Node, and then synchronizing the vault backup data from the target Backup Service.
* By backing up the vault data to the specified Backup service, the Backup Service is directly promoted to Vault service.

需要注意的是，一旦开始迁移vault service，为保持vault 数据的一致性，必须将原vault service停止服务，然后在迁移完成后，再开启新的vault service。开启新的Vault service后，需要更新该DID文档中的vault service入口地址并更新公布到EID中。

It should be noted that once the vault service is migrated, in order to maintain the consistency of vault data, the original vault service must be stopped, and then the new vault service should be started after the migration is completed. After opening a new Vault service, it is necessary to update the vault service entry address in the DID document and publish the update to EID.
