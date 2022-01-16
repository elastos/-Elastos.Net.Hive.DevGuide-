# Hive Node

Hive Node是指Hive去中心化存储网络的一个服务节点，可由社区人员或者组织根据安装指导进行部署运行，并需要24\*7方式常驻运行的节点服务。所有运行的Hive节点组成整个Hive存储网络。

Hive Node refers to a service node of Hive decentralized memory network, which can be deployed and operated by community personnel or organizations according to the installation instructions and needs the node service of 24\*7 resident operation. All the running Hive nodes make up the whole Hive memory network.

Hive Node部署运行后，会有三个子服务系统常驻作为整体服务一体运行：

After Hive Node is deployed and run, there will be three sub-service systems that reside as a whole service:

* Hive HTTP service
* IPFS Node
* MongoDB服务
* Hive HTTP service
* IPFS Node
* MongoDB service

三个子服务各司其职，共同完成整个Hive网络的数据存取服务。前端SDK通过HTTP协议与后端Hive Node进行用户应用数据存取。每个Hive Node为每个DID身份提供单独的Vault服务，Vault服务由前端用户根据DID来进行确权管理和数据存取服务。

The three sub-services perform their duties and jointly complete the data access service of the entire Hive network. The front-end SDK accesses user application data with the back-end Hive Node through the HTTP protocol. Each Hive Node provides a separate Vault service for each DID identity, and the Vault service is managed by the front-end users according to the DID and data access services.

## HTTP Service

Hive Node 服务通过HTTP协议向前端SDK提供以下组件服务：

Hive Node service provides the following component services to front-end SDK through HTTP protocol:

* Vault Service 的订阅创建和销毁；
* Backup Service 的订阅创建和销毁；
* 基于 Vault Service的应用数据存储，数据类型包括数据块文件数据，以及MongoDB类型兼容的非结构化数据；
* 基于自定义规则（Scripting）定义的对Vault的授权数据存储处理；
* Vault 数据通过Backup 服务进行备份/Restore，以及Vault迁移等；
* Subscription creation and destruction of Vault Service
* Subscription creation and destruction of Backup Service
* Application data memory based on Vault Service, with data types including block file data and unstructured data compatible with MongoDB type
* Authorized data memory processing for Vault based on the definition of self-defined rules (Scripting)
* Utilized Backup service to deal with Vault data, such as Backup/Restore and Vault migration

前端应用使用Hive SDKs，基于HTTP request/response与后端Http Service直接数据交互，并将需要进一步处理的数据请求交由右端侧的IPFS Node 和MongoDB service进行处理。

The front-end application uses Hive SDKs, directly interacts with the back-end Http Service based on HTTP request/response and sends the data requests that need further processing to the right-end IPFS Node and MongoDB service for processing.

## IPFS Node

IPFS Node子系统负责接收从左侧 HTTP Service转发的文件存储请求，负责将对应的文件数据存储到该IPFS Node节点Pin保存。 当进行Vault 数据备份时，IPFS Node 也会接收并备份从 Vault中导出的数据，同时将该备份数据副本通过IPFS Network同步到对应的Backup Service中，保证vault数据的冗余性和可恢复性。

IPFS Node subsystem is responsible for receiving the file memory request forwarded from the left HTTP Service and saving the corresponding file data to the IPFS Node node Pin for saving. When the Vault data is backed up, the IPFS Node will also receive and back up the data exported from the Vault, and at the same time synchronize the backup data copy to the corresponding Backup Service through the IPFS Network to ensure the redundancy and recoverability of the vault data.

## MongoDB Service

MongoDB 子服务接收从左侧HTTP Service转发的非结构化数据请求（即mongodb 兼容的数据库数据），将其数据存储到mongdb 内部数据库中进行持久化保存。

The MongoDB subservice receives the unstructured data request (that is, MongoDB compatible database data) forwarded from the left HTTP Service and saves its data in MongoDB internal database for persistent memory.
