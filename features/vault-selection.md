---
description: >-
  Users are free to set up their own Hive node or choose a trusted hive node to
  subscribe to the vault service.
---

# Vault Selection

Hive is an open decentralized memory network where any institution or individual can participate in building and deploying Hive nodes and operate them by themselves. Hive has a plan to introduce an economic incentive mechanism to encourage community users and institutions to deploy nodes and subsequently earn profits, thus forming a fully-functioning node economy and application ecology based on the Hive network.

Ordinary users can choose to subscribe to the Vault service on the trusted Hive Node nodes with DID identity, and then access the application data to the vault. Users can also build, trust in, and use their own Hive Node for their own DID.

After the user subscribes to the vault on the trusted Hive Node according to the DID identity, it's important to bind the vault service entry declaration to the target DID document and update it on the EID sidechain. In this way, other users can analyze and find the corresponding vault entrance according to the target DID, thus participating in the data interaction among different users and expanding the data access business scenarios.

### Users Options to Migrate Vault Service

Once a user subscribes to the Vault service on the specified nodes within the Hive Node, it doesn't mean that the it's permanently bound with the corresponding Hive Node - and no data migration or deletion can be carried out. When users using the Vault service experience on the specified Hive Node, one of the following situations can occur:

* The operator of the Hive nodes informs that the nodes will be stopped
* The corresponding Hive nodes can no longer provide good data access capability
* Users built the Hive nodes by themselves

Users can choose to migrate the existing Vault to the designated new trusted Hive nodes. After the migration is completed, the corresponding vault service portal in DID document will be updated synchronously. In the whole migration process, except that the intermediate Vault will stop serving for a period, other update processes are completely transparent to third-party applications.

The Hive Node nodes has no right to restrict or audit the DID identity that has subscribed to the Vault to use this node. In principle, the data saved in the Vault is transparent to the Hive Node service operator. Once the user has migrated the vault data, the user has the right to delete all historical data saved in the original Hive Node.
