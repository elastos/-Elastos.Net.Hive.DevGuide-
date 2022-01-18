# Hive Node

Hive Node refers to a service node of Hive decentralized memory network, which can be deployed and operated by community personnel or organizations according to the installation instructions and needs the node service of a 24/7 resident operation. Running all of these Hive nodes makes up the entire Hive memory network.

After Hive Node is deployed and run, there will be three sub-service systems that reside as a whole service:

* Hive HTTP service
* IPFS Node
* MongoDB service

The three sub-services perform their duties and jointly complete the data access service of the entire Hive network. The frontend SDK accesses user application data with the backend Hive Node through the HTTP protocol. Each Hive Node provides a separate Vault service for each DID identity, and the Vault service is managed by the frontend users according to the DID and data access services.

## HTTP Service

Hive Node service provides the following component services to frontend SDK through HTTP protocol:

* Subscription creation and destruction of Vault service
* Subscription creation and destruction of Backup service
* Application data memory based on Vault service, with data types including block file data and unstructured data compatible with MongoDB type
* Authorized data memory processing for Vault based on the definition of self-defined rules (Scripting)
* Utilized Backup service to deal with Vault data, such as backup/restore and Vault migration

The frontend application uses Hive SDKs, directly interacts with the backend HTTP service based on HTTP request/response, and sends the data requests that need further processing to the right-end IPFS Node and MongoDB service.

## IPFS Node

The IPFS Node subsystem is responsible for receiving the file memory request forwarded from the left HTTP Service and saving the corresponding file data to the IPFS Node node pin for saving. When the Vault data is backed up, the IPFS Node will also receive and back up the data exported from the Vault, and at the same time synchronize the backup data copy to the corresponding Backup service through the IPFS Network to ensure the redundancy and recoverability of the vault data.

## MongoDB Service

The MongoDB subservice receives the unstructured data request (that is, MongoDB compatible database data) forwarded from the left HTTP Service and saves its data in MongoDB internal database for persistent memory.
