# Vault Backup/Restore

After the user subscribes to the Vault Service on the trusted Hive Node, in order to ensure the redundancy and recoverability of the Vault data, the user is encouraged to take a snapshot of the Vault data in order to generate backup data and save it in the paired IPFS Node.

Once the Backup data is saved in the corresponding IPFS Node, it will be automatically backed up from the original IPFS Node to the target Backup service through the IPFS DHT mechanism, if the user subscribes to the Backup service on another Hive Node.

## Vault Backup

Vault backup means that the user backs up the Vault data based on the specified DID identity to another trusted Hive Node node. Users must subscribe to the Backup Service with the same DID identity on the target Hive Node. The Vault service of the source Hive Node saves the backup data in the target Backup Service through the IPFS network backup.

## Vault Restore

Vault restore means that users can obtain backup data from the specified Backup service, rebuild it on the trusted target Hive node to restore the Vault service, and continue to provide data access services to current users.
