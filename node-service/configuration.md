# Configuration

Hive Node服务的执行依赖于它的配置文件，配置文件中包含了以下几个部分：

The execution of Hive Node service depends on its configuration file, which includes the following parts:

* 运行参数。例如：did resolver链接，Hive Node DID信息等
* Running parameters. For example: did resolver link, Hive Node DID information, etc.
* 功能开关。是否启用CORS功能
* Function switch. Enable CORS function?

下面以开发环境的配置参数为例，介绍下配置文件中可能用到的参数：

Taking the configuration parameters of the development environment as an example, the following parameters that may be used in the configuration file are introduced:

* DID Resolver和ELA Resolver

```
DID_RESOLVER=https://api.trinity-tech.cn/eid
ELA_RESOLVER=https://api-testnet.elastos.io/ela
```

* Hive Node Instance DID

```
DID_MNEMONIC="advance duty suspect finish space matter squeeze elephant twenty over stick shield"
DID_PASSPHRASE=secret
DID_STOREPASS=password
```

* 本地数据目录位置，用于保存DID等信息
* Local data directory location, used to save DID and other information.

```
HIVE_DATA = ./data
```

* 支付配置文件和Hive Node拥有者的钱包地址
* Payment profile and wallet address of Hive Node controller

```
HIVE_PAYMENT_CONFIG=./payment_config.json
HIVE_PAYMENT_ADDRESS=EN9YK69ScA6WFgVQW3UZcmSRLSCStaU2pQ
```

* mongodb的配置
* MongoDB configuration

```
MONGO_TYPE=
MONGO_HOST=localhost
MONGO_PORT=27020
```

* Hive Node管理用的数据库名称
* The name of the database used for management of Hive Node.

```
DID_INFO_DB_NAME=hive_manage_info
```

* Hive Node基本信息：版本号及提交的hash值
* Basic information of Hive Node: version number and submitted hash value

```
HIVE_VERSION=v2.3.6
HIVE_COMMIT_HASH=cec7d255e758bfddea3cff0a36b117bae0770b13
```

* 是否支持CORS开关
* Is CORS switch supported?

```
ENABLE_CORS=True
```

* IPFS服务配置信息
* IPFS service configuration information

IPFS\_NODE\_URL = http://localhost:5002 IPFS\_PROXY\_URL = http://localhost:8080
