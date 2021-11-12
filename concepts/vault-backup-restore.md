# Vault backup/restore

## Vault backup

Vault backup 是指用户将基于指定DID身份的Vault数据备份到另一个可信Hive Node节点中。用户必须在该目标Hive Node上使用同一DID身份订阅Backup Service。源Hive Node的vault service将备份数据通过IPFS网络备份存储到目标Backup Service中。

## Vault restore

Vault restore是指用户可以从指定的Backup service中获取备份数据，并将其在可信目标Hive node上重建恢复vault service，继续向当前用户提供数据存取服务。
