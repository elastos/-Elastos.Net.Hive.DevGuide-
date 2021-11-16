# Subscribe to vault service

订阅Vault服务是使用Vault数据空间的第一步。Vault是一个概念，表示了Hive Node的一块独立的区域，在这个区域内，用户可以使用Vault的相关服务。作为一个Hive Node的使用者，必然是通过应用来进行的，若想存储属于自己的数据（比如：文件和数据库数据），必须先订阅Vault。

SDK里面，VaultSubscription用于实现Vault的订阅功能。创建它的语句如下：

```java
public VaultSubscription newVaultSubscription() throws HiveException {
    return new VaultSubscription(context, getVaultProviderAddress());
}
```

创建VaultSubscription对象，必须提供2个参数：

- AppContext：用户定义的应用上下文，需要由用户提供三块信息：本地数据存储的位置、App Instance DID、登录时用到的授权信息。
- Vault Provider Address: Hive Node的访问地址。

有了VaultSubscription对象，就可以订阅Vault了。

```java
void testSubscribe() {
    Assertions.assertDoesNotThrow(()->subscription.subscribe().get());
}
```

一旦订阅成功，就可以使用Vault类下面的服务了。

- 文件服务
- 数据库服务
- Scripting服务
- Backup服务
