# Vault Migration

Vault migration is the Vault service migration, which specifically means that the user migrates the Vault service with the specified DID identity from the source Hive Node to the target trusted one.

Hive supports Vault service migration in the following two ways:

* The new Vault service can be restored by backing up the vault data to the designated Backup service, registering a new Vault service in the new trusted Hive Node, and then synchronizing the Vault backup data from the target Backup service.
* By backing up the vault data to the specified Backup service, the Backup service is directly promoted to Vault service.

It should be noted that once the Vault service is migrated, in order to maintain the consistency of vault data, the original Vault service must be stopped. Then, the new Vault service should be started after the migration is completed. After opening a new Vault service, it's critical to update the vault service entry address in the DID document and publish the update to EID.
