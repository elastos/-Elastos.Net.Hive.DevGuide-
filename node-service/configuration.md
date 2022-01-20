# Configuration

The execution of Hive Node service depends on its configuration file, which includes the following parts:

* Running parameters (for example: did resolver link, Hive Node DID information, etc.)
* Function switch. Enable CORS function?

Taking the configuration parameters of the development environment as an example, the following parameters that may be used in the configuration file are introduced:

* DID Resolver and ELA Resolver

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

* The local data directory location is used to save DID and other information.

```
HIVE_DATA = ./data
```

* Payment profile and wallet address of the Hive Node controller

```
HIVE_PAYMENT_CONFIG=./payment_config.json
HIVE_PAYMENT_ADDRESS=EN9YK69ScA6WFgVQW3UZcmSRLSCStaU2pQ
```

* MongoDB configuration

```
MONGO_TYPE=
MONGO_HOST=localhost
MONGO_PORT=27020
```

* The name of the database used for management of the Hive Node.

```
DID_INFO_DB_NAME=hive_manage_info
```

* Basic information of Hive Node: version number and submitted hash value

```
HIVE_VERSION=v2.3.6
HIVE_COMMIT_HASH=cec7d255e758bfddea3cff0a36b117bae0770b13
```

* Is CORS switch supported?

```
ENABLE_CORS=True
```

* IPFS service configuration information

IPFS\_NODE\_URL = http://localhost:5002 IPFS\_PROXY\_URL = http://localhost:8080
