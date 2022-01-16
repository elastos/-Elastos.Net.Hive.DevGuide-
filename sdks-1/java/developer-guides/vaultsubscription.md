# VaultSubscription

Hive SDK需要通过Vault实例与远程的Vault Service进行交互。用户预先在可信Hive Node上订阅创建Vault Service，然后才能将应用数据存取到对应的 Vault中.

Hive SDK needs to interact with the remote Vault service through the vault instance. Users subscribe to the trusted Hive Node to create Vault Service in advance, and then they can access the application data to the corresponding Vault.

VaultSubscription类就是用来定订阅创建Vault Service，Hive Node服务根据请求者的DID身份创建新的Vault Service。如果基于该DID身份的Vault Service已经存在，则返回已存在的 Vault Service元数据.

The VaultSubscription class is used to create a Vault Service by subscription, and the Hive Node service creates a new Vault Service according to the DID identity of the requester. If the Vault Service based on the DID identity already exists, the existing Vault Service metadata will be returned.

## Example

使用 vaultSubscription实例在可信Hive Node订阅创建新的Vault Service，返回一个 CompletableFuture 对象，包含已订阅的远端Vault Service的元数据信息。实际样例代码如下：

Use the vaultSubscription instance to create a new Vault Service in the trusted Hive Node subscription and return a CompletableFuture object containing the metadata information of the subscribed remote Vault Service. The actual sample code is as follows:

```java
VaultSbuscription subscription = new VaultSubscription(context, getVaultProvider());
subscription.subscribe().thenAccept(vaultInfo -> {
    System.out.println("Registered vault service successfully");
    System.out.println("service DID ==> " + vaultInfo.getServiceDid());
    System.out.println("storage quota ==> " + vaultInfo.getStorageQuota());
    System.out.println("price plan ==> " + vaultInfo.getPricePlan());
}).exceptionally(ex -> {
    System.out.println("Failed to register vault service")
    ex.printStackTrace();
    return null;
});
```

一旦订阅成功，就可以使用实例化对象vault来存取应用数据。

Once the subscription is successful, the instantiation object vault can be used to access the application data.

## Subscription management

### Check Subscription

用户在可信Hive Node上订阅创建后，可以使用vault实例来存取应用中的数据，也可以在使用以下接口查看对应Vault Service的信息。

After subscribing to the trusted Hive Node, users can use the vault instance to access the data in the application, or view the information of the corresponding Vault Service by using the following interface.

```java
subscription.checkSubscription().thenAccept(vaultInfo -> {
    System.out.println("Dump remote vault service:");
    System.out.println("service DID ==> " + vaultInfo.getServiceDid());
    System.out.println("storage quota ==> " + vaultInfo.getStorageQuota());
    System.out.println("price plan ==> " + vaultInfo.getPricePlan());
}).exceptinally(ex -> {
    System.out.println("Failed to check vault subscription");
    ex.printStackTrace();
    return null;
});
```

### Unsubscription

用户在使用Vault Service一段时间，根据需要，可以将已使用中Vault Service迁移到新的 Hive Node中，迁移完成后需要停止原来 Hive Node上的 Vault Service, 同时销毁其中数据。

Users can use the Vault Service for a period and migrate the already used Vault Service to the new Hive Node as needed. After the migration, it is necessary to stop the Vault Service on the original Hive Node and destroy the data in it.

```java
subscription.unsbuscribe().thenAccept(result -> {
    System.out.println("Unsubscribed from the vault service");
    System.out.println("Cleared all data in this vault");
}).exceptinally(ex -> {
    System.out.println("Failed to unsubscribe to vault service");
    ex.printStackTrace();
    return null;
});
```

注意：调用此接口取消vault Service 后，该vault内持有数据从对应 Hive Node 上被永久删除，同时该 Vault Serivce 也停止服务。用户必须先保证已有新的 Vault service 完成迁移后，再可调用该接口取消 原来需要废弃的vault service。

Attention: after calling this interface to cancel the vault Service, the data held in the vault will be permanently deleted from the corresponding Hive Node, and the Vault Service will stop serving as well. Users must ensure that a new Vault service has been migrated before calling this interface to cancel the vault service that originally needed to be abandoned.
