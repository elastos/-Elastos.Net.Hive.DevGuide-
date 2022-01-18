# Vault Service

Vault service is a data access service based on DID identity. Users can save the third-party application data to the Vault service after subscribing to the Vault service on the trusted Hive Node by specifying DID identity.

At present, Vault Service already supports the following data access service functions:

* Access of file type data
* Access of unstructured data
* Customize data access rights through scripting rules, supporting data interaction and data aggregation of different DID identities.

## Ownership of Vault Data

Users can subscribe to create a Vault Service on a trusted Hive Node based on their DID identity, and the ownership of data in the Vault can be confirmed by DID.

## Sandboxing Management

The Vault service performs data sandboxing management based on the user DID and the application DID. Data access between different DID identities and between different applications can be isolated from each other without interference.

## Vault Service Migration

The Vault service directly backs up the Vault data to the specified Backup service, or it can migrate the data in the vault to a new trusted Hive node to generate a new Vault Service.
