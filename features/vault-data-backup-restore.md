---
description: Always remember to backup your Vault data into backup service
---

# Vault Data Backup/Restore

After the user subscribes to the Vault Service on the trusted Hive Node, in order to ensure the redundancy and recoverability of its data, the user is encouraged to take a snapshot of the Vault data to generate backup information, then saving it in the paired IPFS Node.&#x20;

Once the backup data is saved in the corresponding IPFS Node, it will be automatically backed up from the original IPFS Node to the target backup service through the IPFS DHT mechanism, as long as the user subscribes to the backup service on other Hive Nodes.

## Vault Backup

The user subscribes to the backup service on other trusted Hive Nodes through the same DID identity and associates it with the Vault Service as the backup source through the DID identity.

When the user triggers the backup event, the backup data will be exported from the specified Vault Service to the paired IPFS Node and a unique root CID value will be generated, as well as other metadata, such as memory space. The Vault Service sends this information to the backup service, and the backup service synchronizes the corresponding backup data to the current Hive IPFS according to the root CID, thus completing the entire Vault backup process.

## Vault Restore

A user who backs up their Vault data to the backup service can synchronize that data to a corresponding Vault service to complete the Vault restore process.
