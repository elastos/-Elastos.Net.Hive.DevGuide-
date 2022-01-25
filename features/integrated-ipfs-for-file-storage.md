---
description: >-
  Utilize the strengths of IPFS network to work for Hive and enhance its
  accessibility and redundancy of vault data.
---

# Integrated IPFS for file Storage

### Why Use an Integrated IPFS Solution?

At present, the decentralized public memory technology led by IPFS tends to mature and has been widely used in various data memory business scenarios, especially in the fields of website construction, NFT data storage, and streaming media CDN. However, the data types saved in IPFS tends to be inactive and cold, which is difficult to adapt to the hot data frequently interacted and updated in mobile applications - it also requires high response requirements of real-time data access. For these reasons, it's difficult to use IPFS technology directly and widely in mobile applications.

On the other hand, if the data redundancy and stability of the decentralized memory network of IPFS are fully utilized, it will be integrated into the Hive memory network as a multi-point backup mechanism for backend file data and backup data. By doing this, it can ensure the redundancy of saved data and backup data, thus solidifying the recoverability and accessibility of Vault data.

### How to Make IPFS Work for Hive

After the user builds and deploys the Hive Node nodes, three different sub-services will be initiated and run internally:

* Hive HTTP service
* IPFS Node service
* MongoDB service

Sub-services perform their respective duties and are indispensable to jointly complete the data access service of the entire Hive network. Among them, IPFS Node takes advantage of the characteristics of IPFS and is used for the following two aspects of file memory services:

* HTTP service receives the file data access request from the frontend SDK and saves the data in the corresponding IPFS Node.
* When the Vault data backup is triggered, the Vault data will be backed up to the corresponding IPFS Node after snapshot processing, and the backup data will be transferred to the designated backup service through the DHT mechanism of the IPFS network.

## Conclusion

By integrating IPFS into Hive as the backend file memory mechanism, not only can the file data be saved, but also the backup and restoration of the Vault data can be implemented with the lowest cost - this means that the vault service can be migrated among different Hive Nodes.
