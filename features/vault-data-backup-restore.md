# Vault data backup/restore

用户在可信的Hive Node上订阅了Vault Service后，为了保证Vault 数据的冗余性，支持用户将Vault数据进行快照生成备份数据，并将该备份数据存储到配对的IPFS Node中。

## Vault backup

用户需要通过同一DID身份在其他可信Hive Node上订阅Backup service，并将该Backup Service与需要备份的Vault Service通过同一DID身份挂钩。当在Vault Service中导出备份数据到配对的IPFS Node后生成唯一的根CID值，以及其他Vault数据信息，比如使用空间总量等。Vault Service将该CID发送到Backup Service后，转而由Backup Service根据CID将对应的备份数据同步到当前的Hive IPFS中，从而完成整个Vault backup过程。

## Vault restore

同样，用户一旦将vault数据备份到Backup service中，可以通过Backup service中的vaut数据同步到一个对应的Vault service中，完成vault restore过程。
