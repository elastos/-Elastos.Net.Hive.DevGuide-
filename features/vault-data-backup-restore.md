---
description: Keep remember to backup your vault data into backup service
---

# Vault data backup/restore

用户在可信Hive Node上订阅了Vault Service后，为了保证Vault数据的冗余性和可恢复性，支持用户将Vault数据进行快照生成备份数据，并将该备份数据存储到配对的IPFS Node中。

After the user subscribes to the Vault Service on the trusted Hive Node, in order to ensure the redundancy and recoverability of the Vault data, the user is supported to snapshot the Vault data to generate backup data and save the backup data in the paired IPFS Node.

一旦备份数据被存储到对应的IPFS Node之后，只要用户在其他Hive Node上订阅了Backup服务，则自动将备份数据从原IPFS Node通过IPFS DHT机制备份到目标Backup服务中。

Once the backup data is saved in the corresponding IPFS Node, it will be automatically backed up from the original IPFS Node to the target Backup service through the IPFS DHT mechanism as long as the user subscribes to the Backup service on other Hive Node.

## Vault backup

用户通过同一DID身份在其他可信Hive Node上订阅Backup service，并将该Backup Service与作为备份源的Vault Service通过DID身份关联。

The user subscribes to the Backup service on other trusted Hive Node through the same DID identity and associates the Backup Service with the Vault Service as the backup source through the DID identity.

当用户触发备份事件后，将从指定Vault Service导出备份数据到配对的IPFS Node并生成唯一的根CID值，以及其他元数据，比如使用存储空间。Vault Service将该些信息发送到Backup Service，由Backup Service根据根CID将对应的备份数据同步到当前的Hive IPFS中，从而完成整个Vault backup过程。

When the user triggers the backup event, the backup data will be exported from the specified Vault Service to the paired IPFS Node and a unique root CID value will be generated, as well as other metadata, such as memory space of using. The Vault Service sends this information to the Backup Service, and the Backup Service synchronizes the corresponding backup data to the current Hive IPFS according to the root CID, thus completing the whole Vault backup process.

## Vault restore

用户将vault数据备份到Backup service中，可以通过Backup service中的vaut数据同步到一个对应的Vault service中，完成vault restore过程。

User backs up the vault data to the Backup service and can synchronize vault data in the Backup service to a corresponding Vault service to complete the vault restore process.
