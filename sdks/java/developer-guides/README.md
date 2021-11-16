# Developer guides

Hive Java SDK封装了Hive Node的HTTP接口，同时提供了包装好的类，供应用使用。

应用通过SDK的相关类，可以做到如下几点：

- 隐式的登陆机制：对于需要Token的接口，SDK在调用的时候，会去检查缓存是否有Token，没有会去执行登陆，获取到接口访问需要的Token。
- Vault服务：每个用户都可以通过SDK，在Hive Node上订阅属于自己的Vault，用户用到的所有服务都是基于Vault展开的。Vault的中的数据主要为：文件和数据库数据。
- 文件服务：文件服务属于Vault服务的一部分，它通过结合IPFS分布式文件存储服务向用户提供文件存储空间，用户可以保存应用中用到的文件数据。
- 数据库服务：基于mongodb的数据库存储。可以保存应用的数据，比如配置信息、应用数据等。
- Scripting服务：提供了一种让访问者访问数据拥有者数据的机制。
- 备份服务：用于备份Vault数据到其它的Hive Node上，或者从其它Hive Node上恢复备份好的数据。
- Promotion服务：用于将从其它Hive Node备份的Vault数据升级为一个新的Vault，相当于进行了Vault迁移。
- Payment服务：用于升级Vault和备份服务的存储空间。
