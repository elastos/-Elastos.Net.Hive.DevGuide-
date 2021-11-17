# Configuration

Hive Node服务的执行依赖于它的配置文件，配置文件中包含了以下几个部分：

- 运行参数。例如：did resolver链接，Hive Node DID信息等
- 功能开关。是否启用CORS功能

下面以开发环境的配置参数为例，介绍下配置文件中可能用到的参数：

- DID Resolver和ELA Resolver

```
DID_RESOLVER=https://api.trinity-tech.cn/eid
ELA_RESOLVER=https://api-testnet.elastos.io/ela
```

- Hive Node Instance DID

```
DID_MNEMONIC="advance duty suspect finish space matter squeeze elephant twenty over stick shield"
DID_PASSPHRASE=secret
DID_STOREPASS=password
```

- 本地数据目录位置，用于保存DID等信息

```
HIVE_DATA = ./data
```

- 支付配置文件和Hive Node拥有者的钱包地址

```
HIVE_PAYMENT_CONFIG=./payment_config.json
HIVE_PAYMENT_ADDRESS=EN9YK69ScA6WFgVQW3UZcmSRLSCStaU2pQ
```

- mongodb的配置

```
MONGO_TYPE=
MONGO_HOST=localhost
MONGO_PORT=27020
```

- Hive Node管理用的数据库名称

```
DID_INFO_DB_NAME=hive_manage_info
```

- Hive Node基本信息：版本号及提交的hash值

```
HIVE_VERSION=v2.3.6
HIVE_COMMIT_HASH=cec7d255e758bfddea3cff0a36b117bae0770b13
```

- 是否支持CORS开关

```
ENABLE_CORS=True
```

- IPFS服务配置信息

IPFS_NODE_URL = http://localhost:5002
IPFS_PROXY_URL = http://localhost:8080
