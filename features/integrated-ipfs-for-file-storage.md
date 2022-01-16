---
description: >-
  Utilize the strengths of IPFS network to work for Hive, and enhance its
  accessiblity and redundancy of vault data as well.
---

# Integrated IPFS for file storage

### Why integrated IPFS solution

当前IPFS为首的去中心化公共存储技术趋于成熟，已被广泛应用于各种数据存储业务场景，特别是网站建设，NFT数据存储，以及流媒体CDN等领域。但是IPFS中所存储的数据类型倾向于非活动的冷数据，很难适应移动应用中频繁交互和更新的热数据，并且要求实时数据存取的高响应需求。基于这些原因，纯粹依靠IPFS技术很难在移动应用被直接广泛使用。

At present, the decentralized public memory technology led by IPFS tends to mature and has been widely used in various data memory business scenarios, especially in the fields of website construction, NFT data storage and streaming media CDN. However, the data types saved in IPFS tend to be inactive cold data, which is difficult to adapt to the hot data frequently interacted and updated in mobile applications and requires high response requirements of real-time data access. For these reasons, it is difficult to use IPFS technology directly and widely in mobile applications.

但从另一方面来说，如果充分利用IPFS去中心化存储网络的数据冗余性和稳定性，将其集成到Hive存储网络中，作为后端文件数据和备份数据的多点备份机制。通过将IPFS集成到Hive网络，使其能基本保证存储数据和备份数据的冗余性，从而保证Vault数据的可恢复性和可访问性。

On the other hand, if the data redundancy and stability of the decentralized memory network of IPFS are fully utilized, it will be integrated into the Hive memory network as a multi-point backup mechanism for back-end file data and backup data. By integrating IPFS into Hive network, it can basically ensure the redundancy of saved data and backup data, thus ensuring the recoverability and accessibility of Vault data.

### How make IPFS work for Hive

用户搭建和部署Hive Node节点后，内部会启动运行三个不同的子服务。

After the user builds and deploys the Hive Node nodes, three different sub-services will be initiated and run internally.

* Hive HTTP service，
* IPFS Node service
* MongoDB service

子服务各司其职，缺一不可，共同完成整个Hive网络的数据存取服务。其中IPFS Node利用IPFS的特性优势，用于以下两个方面的文件存储服务：

Sub-services perform their respective duties and are indispensable to jointly complete the data access service of the entire Hive network. Among them, IPFS Node takes advantage of the characteristics of IPFS and is used for the following two aspects of file memory services:

* HTTP service接收前端SDK对文件数据存取请求，将数据存储到对应的IPFS Node中；
* 在触发Vault数据备份时，会将Vault数据进行快照处理后备份到对应的IPFS Node中，并通过IPFS网络本身的DHT机制，将备份数据传递指定Backup服务中。
* HTTP service receives the file data access request from the front-end SDK and saves the data in the corresponding IPFS Node.
* When the Vault data Backup is triggered, the Vault data will be backed up to the corresponding IPFS Node after snapshot processing, and the backup data will be transferred to the designated backup service through the DHT mechanism of the IPFS network.

## Conclusion

总之，通过集成IPFS作为后端文件存储机制到Hive中，不仅实现了对文件数据的存储，同时以最低代价地实现对Vault数据的backup和restore，从而完成vault service在不同Hive Node间迁移。

In a word, by integrating IPFS into Hive as the back-end file memory mechanism, not only the file data can be saved, but also the backup and restoration of the Vault data can be implemented with the lowest cost, so that the vault service can be migrated among different Hive Node.
