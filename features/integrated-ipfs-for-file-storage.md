---
description: Utilize the strengths of IPFS network to work for Hive, and enhance its accessiblity and redundancy of vault data as well.
---

# Integrated IPFS for file storage

### Why integrated IPFS solution&#x20;

当前IPFS为首的去中心化公共存储技术趋于成熟，已被广泛应用于各种数据存储业务场景，特别是网站建设，NFT数据存储，以及流媒体CDN等领域。但是IPFS中所存储的数据类型倾向于非活动的冷数据，很难适应移动应用中频繁交互和更新的热数据，并且要求实时数据存取的高响应需求。基于这些原因，纯粹依靠IPFS技术很难在移动应用被直接广泛使用。

但从另一方面来说，如果充分利用IPFS去中心化存储网络的数据冗余性和稳定性，将其集成到Hive存储网络中，作为后端文件数据和备份数据的多点备份机制。通过将IPFS集成到Hive网络，使其能基本保证存储数据和备份数据的冗余性，从而保证Vault数据的可恢复性和可访问性。

### How make IPFS work for Hive

用户搭建和部署Hive Node节点后，内部会启动运行三个不同的子服务。

* Hive HTTP service，
* IPFS Node service
* MongoDB service

子服务各司其职，缺一不可，共同完成整个Hive网络的数据存取服务。其中IPFS Node利用IPFS的特性优势，用于以下两个方面的文件存储服务：

* HTTP service接收前端SDK对文件数据存取请求，将数据存储到对应的IPFS Node中；
* 在触发Vault数据备份时，会将Vault数据进行快照处理后备份到对应的IPFS Node中，并通过IPFS网络本身的DHT机制，将备份数据传递指定Backup服务中。

## Conclusion

总之，通过集成IPFS作为后端文件存储机制到Hive中，不仅实现了对文件数据的存储，同时以最低代价地实现对Vault数据的backup和restore，从而完成vault service在不同Hive Node间迁移。
