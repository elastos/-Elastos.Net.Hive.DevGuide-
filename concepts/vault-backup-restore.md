# Vault backup/restore

用户在可信Hive Node上订阅了Vault Service后，为了保证Vault数据的冗余性和可恢复性，支持用户将Vault数据进行快照生成备份数据，并将该备份数据存储到配对的IPFS Node中。

After the user subscribes to the Vault Service on the trusted Hive Node, in order to ensure the redundancy and recoverability of the Vault data, the user is supported to snapshot the Vault data to generate backup data and save the backup data in the paired IPFS Node.

一旦备份数据被存储到对应的IPFS Node之后，只要用户在其他Hive Node上订阅了Backup服务，则自动将备份数据从原IPFS Node通过IPFS DHT机制备份到目标Backup服务中

Once the Backup data is saved in the corresponding IPFS Node, the Backup data will be automatically backed up from the original IPFS Node to the target Backup service through the IPFS DHT mechanism if the user subscribes to the Backup service on other Hive Node.

## Vault backup

Vault backup 是指用户将基于指定DID身份的Vault数据备份到另一个可信Hive Node节点中。用户必须在该目标Hive Node上使用同一DID身份订阅Backup Service。源Hive Node的vault service将备份数据通过IPFS网络备份存储到目标Backup Service中。

Vault backup means that the user backs up the vault data based on the specified DID identity to another trusted Hive Node node. Users must subscribe to the Backup Service with the same DID identity on the target Hive Node. The vault service of the source Hive Node saves the backup data in the target Backup Service through the IPFS network backup.

## Vault restore

Vault restore是指用户可以从指定的Backup service中获取备份数据，并将其在可信目标Hive node上重建恢复vault service，继续向当前用户提供数据存取服务。

Vault restore means that users can obtain backup data from the specified Backup service, rebuild it on the trusted target Hive node to restore the vault service, and continue to provide data access services to current users.
