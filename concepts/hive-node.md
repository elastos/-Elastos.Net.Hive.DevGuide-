# Hive Node

Hive Node是指Hive去中心化存储网络的一个服务节点，可由社区人员或者组织根据安装指导进行部署运行，并需要24*7方式常驻运行的节点服务。所有运行的Hive节点组成整个Hive存储网络。

Hive Node部署运行后，会有三个子服务系统常驻作为一体运行：
* Hive HTTP service
* IPFS Node
* MongoDB服务

三个子服务各司其职，共同完成整个Hive 网络的数据存取服务。用户可以通过HTTP接口协议与后端Hive Node进行数据存取。每个Hive Node会承载会承载多个Vault服务，每个Vault服务由前端用户根据DID来进行确权管理和数据存取服务。

## HTTP Service

Hive Node 服务通过HTTP协议向前端用户提供以下组件服务：
* Vault Service的生命周期管理，比如订阅/取消订阅；
* Backup Service的生命周期管理，比如订阅/取消订阅订阅；
* Vault数据存取，包括数据块文件数据和MongoDB类型的非结构化数据；
* 通过规则脚本自定义Vault中的数据存取权限；
* Vault 数据的备份/Restore，以及迁移等

前端用户使用Hive SDKs，基于HTTP request/response与后端Http Service直接数据交互，并将需要进一步处理的数据请求交由右端侧的IPFS Node 和MongoDB service进行处理。

## IPFS Node

IPFS Node子系统负责接收从左侧 HTTP Service发送的文件存储请求，同时将对应的文件数据存储到IPFS Node节点Pin保存。
IPFS Node也会接收从Vault Service导出的备份数据到IPFS Node节点，同时将该备份数据副本保存到Vault对应的Backup service中，已保证vault数据的冗余性和可恢复性。

## MongoDB&#x20;

MongoDB 子服务接收从左侧HTTP Service发送的非结构化数据请求（即mongodb 兼容的数据库数据），将其数据存储到mongdb 内部数据库中保存。
