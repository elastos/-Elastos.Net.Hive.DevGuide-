# Developer guides

Hive Swift SDK 封装了 Hive Node 的 HTTP 接口，同时提供了包装好的类，供应用使用。从整体而言，Swift SDK 的服务分为以下几个模块：

* 隐式的登陆机制：对于需要 Token 的接口，SDK 在调用的时候，会去检查缓存是否有 Token，没有会去执行登陆，获取到接口访问需要的 Token。
* Vault Service：基于 Vault 的服务，也是整个 Hive Node 服务的核心。
* Payment Service：支付服务，用于升级 Vault 的功能，比如：存储空间。
* Backup Service：用于备份 Vault 数据到其它的 Hive Node 上，或者从其它 Hive Node 上恢复备份好的数据。
* Promotion Service：用于将从其它 Hive Node 备份的 Vault 数据升级为一个新的 Vault，相当于进行了 Vault 迁移。

基于Vault的服务，又被分为以下几块：

* File Service：文件服务属于 Vault 服务的一部分，它通过结合 IPFS 分布式文件存储服务向用户提供文件存储空间，用户可以保存应用中用到的文件数据。
* Database Service：基于 mongodb 的数据库存储。可以保存应用的数据，比如配置信息、应用数据等。
* Scripting Service：提供了一种让访问者访问数据拥有者数据的机制。

## Login

Hive Node 数据服务相关的接口是需要登陆才能访问的，Swift SDK 为了方便用户使用 Hive Node 的 HTTP 接口，对于需要登陆才能使用的接口，进行了隐式的登陆操作。用户所要做的就是在构建服务访问 Hive Node 接口之前，准备好相关的 DID 信息并提供 AppContext 给相关服务。定义 AppContext 的例子如下：

```
public class TestAppContextProvider: AppContextProvider {
    public func getAuthorization(_ authenticationChallengeJWTcode: String) -> Promise<String>? {
        return Promise<String> { resolver in
            resolver.fulfill(try signAuthorization(authenticationChallengeJWTcode)!)
        }
    }
    
    public var appInstanceDid: AppDID
    public var userDid: UserDID
    private var localDataDir: String

    public func getLocalDataDir() -> String? {
        self.localDataDir
    }
    
    public func getAppInstanceDocument() -> DIDDocument? {
        return try! appInstanceDid.getDocument()
    }
    
    init(_ path: String, _ userDid: UserDID, _ appInstanceDid: AppDID) {
        self.localDataDir = path
        self.userDid = userDid
        self.appInstanceDid = appInstanceDid
    }
    
    public func signAuthorization(_ jwtToken: String) throws -> String? {
        
        var accessToken: String?
        let semaphore: DispatchSemaphore = DispatchSemaphore(value: 0)
        var err: Error?
        DispatchQueue.global().async { [self] in
            do {
                let claims = try JwtParserBuilder().build().parseClaimsJwt(jwtToken).claims
                let iss = claims.getIssuer()
                let nonce = claims.get(key: "nonce") as? String
                
                let vc = try userDid.issueDiplomaFor(appInstanceDid)
                let vp: VerifiablePresentation = try appInstanceDid.createPresentation(vc, iss!, nonce!)
                let token = try appInstanceDid.createToken(vp, iss!)
                accessToken = token
                semaphore.signal()
            } catch{
                err = error
                semaphore.signal()
            }
        }
        semaphore.wait()
        guard err == nil else {
            throw err!
        }
        return accessToken!
    }
}

let context = try AppContext.build(TestAppContextProvider(storePath, userDid, appInstanceDid), userDid.description)
```

除此之外若需要登陆 Hive Node，需要提供 Hive Node 的地址，提供地址仅需要通过获取配置文件的地址即可：

```
//	TODO
```

## Payment Service

订阅 Vault Service 使得用户可以使用 Hive Node 存放数据。若需要更大的 Vault 空间，Hive Node 提供了 Payment Service，通过 Payment Service 可以购买 Hive Node 的价格套餐，升级 Vault 的存储空间。

### Example

使用 Payment Service 升级 Vault 空间的第一步是生成订单，此时需要指定价格计划的名称。获取 Hive Node 支持的价格计划列表，参见 [Vault Subscription](vault-subscription.md)。 生成订单操作返回的是订单详情，使用 Payment Service 下单的示例如下：

```
let paymentService = try VaultSubscription(appContext, providerAddress)
paymentService!.placeOrder(PRICING_PLAN_NAME)
.done { order in
   print("Place the order successfully")
}
.catch { error in
   print(error)
}
```

### Pay Order

用户生成订单之后，需要执行真正的支付操作，此时会获取到支付的事务 ID，加上生成的订单号，就可以通过支付订单接口，将订单转为支付状态。支付订单接口返回的是支付凭证 Receipt，有了支付凭证可以作为升级 Vault 的证明。

```
paymentService!.payOrder(ORDER_ID, TRANS_ID)
.done { receipt in
   print("Pay the order successfully")
}
.catch { error in
   print(error)
}
```

### Get Orders

Payment Service 还提供了接口，可以获取到用户生成的所有订单详情。

```
paymentService!.getOrderList()
.done { list in
   print("Get the orders successfully")
}
.catch { error in
   print(error)
}
```

### Get Order

如果已经知道了订单 ID，可以直接通过订单 ID 获取订单详情。

```
paymentService!.getOrder(ORDER_ID)
.done { order in
   print("Get the order successfully")
}
.catch { error in
   print(error)
}
```

## Backup Service

用户在选择 Hive Node 时，会选择安全可靠的节点订阅 Vault Service，保存数据到 Vault 中。Hive Node 为了确保数据的安全性，支持通过 Backup Service 备份 Vault 数据到其它 Hive Node。

### Example

备份 Vault 数据到另外的 Hive Node，可以执行如下的操作：

```
let vault = try Vault(context, providerAddress)
let backService = try newVault().backupService
backupService!.startBackup()
.done { success in
   print("Start backup successfully")
}
.catch { error in
   print(error)
}
```

备份操作仅仅通知 Backup Service 执行备份操作，整个备份流程需要在独立的线程中执行，执行的结果会记录到相关的表中。备份的流程大致为：

* 将 Vault 的数据库数据打包，然后保存到 IPFS 节点上
* 将文件在 IPFS 节点上的索引汇总到一起
* 将文件和数据表在 IPFS 上的信息汇总成元信息，提交到 IPFS 节点上
* 将备份数据的元信息，提交给备份的 Hive Node
* 备份的 Hive Node 将拿到的备份元数据保存到备份表中，同时将所有备份文件的索引记录到备份 Hive Node上

### Restore

恢复（Restore）过程就是备份的逆过程，是将备份 Hive Node 上的 Vault 打包信息恢复到当前的 Hive Node 上，现有 Hive Node 上的 Vault 数据将会全部被重置为备份包的内容。

```
backupService!.restoreFrom()
.done { success in
   print("Start restore successfully")
}
.catch { error in
   print(error)
}
```

真正的恢复 Vault 数据的过程也同样在独立的线程中进行，执行的结果会保存在备份相关的表中。

### Check Result

Backup Service 提供了检查备份&恢复操作的结果状态，返回的状态信息包括：是否执行完成、是否执行成功等。

```
backupService!.checkResult()
.done { backupResult in
   print("Check result successfully")
}
.catch { error in
   print(error)
}
```

## Promotion Service

通过 Hive Node 的 Backup Service 备份到备份 Hive Node 上的 Vault 数据，可以在备份 Hive Node 上直接升级为普通的 Vault。

### Example

执行promotion操作的例子如下：

```
let backup = try Backup(context, providerAddress)
let promotionService = backup.promotionService
promotionService!.promote().done { success in
   print("Do promotion successfully")
}
.catch { error in
   print(error)
}
```

Promotion Service 连接的是 Backup Hive Node，其它操作和 Vault 的 Hive Node 类似。
