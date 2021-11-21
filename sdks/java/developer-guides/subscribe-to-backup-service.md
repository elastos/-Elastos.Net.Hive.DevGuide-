# BackupSubscription

Hive Node 支持备份 Vault Service 的数据，主要是考虑到个人数据的安全性，SDK 支持将 Hive Node 的 Vault 数据备份到另一个 Hive Node 节点上。使用 Backup Service 和 Vault Service 一样，都需要订阅，只是两个服务位于不同的 Hive Node节点上。

以下是订阅 Backup Service 的示例：

```java
BackupSubscription backup = new BackupSubscription(context, getVaultProvider());
backup.subscribe().thenAccept(result -> {
        System.out.println("Successfully get the result.");
    }).exceptionally(ex -> {
        ex.printStackTrace();
        return null;
    });
```

创建 BackupSubscription 对象用到的参数和创建 VaultSubscription 对象一样，具体参见 [VaultSubscription](subscribe-to-vault-service.md) 的说明。

订阅完 Backup Service 之后，就可以通过 Vault 侧的备份服务备份数据。

## Subscribe

订阅操作的原型如下，返回的是一个 CompletableFuture 对象，方便调用者在异步线程中执行订阅操作。此处的 T 代表的类型 BackupInfo （ Backup Service 的信息）。

```java
CompletableFuture<T> subscribe();
```

## Unsubscribe

取消订阅是取消 Backup Service，取消订阅操作会删除备份的 Hive Node 上用户的Vault备份数据。

```java
CompletableFuture<Void> unsubscribe();
```

## Check Subscription

若是想知道当前的 Backup Service 的订阅信息，可以使用如下方法，它会返回 BackupInfo （ Backup Service 的信息）。

```java
CompletableFuture<T> checkSubscription();
```
