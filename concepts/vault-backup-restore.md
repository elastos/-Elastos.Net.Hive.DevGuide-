# Vault backup/restore

用户在可信Hive Node上订阅了Vault Service后，为了保证Vault数据的冗余性和可恢复性，支持用户将Vault数据进行快照生成备份数据，并将该备份数据存储到配对的IPFS Node中。

一旦备份数据被存储到对应的IPFS Node之后，只要用户在其他Hive Node上订阅了Backup服务，则自动将备份数据从原IPFS Node通过IPFS DHT机制备份到目标Backup服务中

## Vault backup

Vault backup 是指用户将基于指定DID身份的Vault数据备份到另一个可信Hive Node节点中。用户必须在该目标Hive Node上使用同一DID身份订阅Backup Service。源Hive Node的vault service将备份数据通过IPFS网络备份存储到目标Backup Service中。

## Vault restore

Vault restore是指用户可以从指定的Backup service中获取备份数据，并将其在可信目标Hive node上重建恢复vault service，继续向当前用户提供数据存取服务。
