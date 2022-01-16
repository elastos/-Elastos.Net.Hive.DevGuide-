# BackupSubscription

考虑到Hive Node上数据的冗余性，需要保持Vault中数据的多地备份. 以防对应 Hive Node如果失效后，可选择在其他可信的 Hive Node恢复生成可继续服务的 Vault Service。 BackupSubscription 类就是用于 在可信Hive Node上订阅创建Backup Service。创建完成后，就可用于将制定 Vault Service中数据备份到该Backup Service中来。

Considering the redundancy of data on the Hive Node, it is necessary to keep multiple backups of data in the Vault. In case that the corresponding Hive Node fails, other trusted hive nodes can be selected to restore and generate the Vault Service that can continue to serve. BackupSubscription class is used to subscribe to the trusted Hive Node to create a Backup Service. After creation, it can be used to back up the data in the established Vault Service to the Backup Service.

BackupSubscription 类就是用来定订阅创建 Backup Service，Hive Node 服务根据请求者的DID身份创建新的 Backup Service。如果基于该DID身份的 Backup Service 已经存在，则返回已存在的 Backup Service 元数据信息。

BackupSubscription class is used to create a Backup Service by subscription, and Hive Node service creates a new Backup Service according to the DID identity of the requester. If the Backup Service based on the DID identity already exists, the metadata information of the existing Backup Service is returned.

## Examples

使用BackupSubscription实例在可信的Hive Node订阅创建新的 Backup Service，返回一个 CompletableFuture 对象，包含该订阅的远端 Backup Service的元数据信息。实际样例代码如下：

Use BackupSubscription instance to create a new Backup Service in the trusted Hive Node subscription and return a CompletableFuture object containing metadata information of the remote Backup Service of the subscription. The actual sample code is as follows:

```java
BackupSubscritpion subscription = new BackupSubscritpion(context, getVaultProvider());
subscription.subscribe().thenAccept(backup -> {
    System.out.println("Registered backup service successfully");
    System.out.println("service DID ==> " + backup.getServiceDid());
    System.out.println("storage quota ==> " + backup.getStorageQuota());
    System.out.println("price plan ==> " + backup.getPricePlan());
}).exceptionally(ex -> {
    System.out.println("Failed to register backup service")
    ex.printStackTrace();
    return null;
});
```

创建订阅完 Backup Service 成功后，就可以通过 Backup service 将该DID身份持有的 Vault 数据备份到该 Hive nodo 节点中。

After successfully subscribing to the Backup Service, the Vault data held by the DID identity can be backed up to the Hive Node nodes through the Backup service.

注意： 创建订阅 Backup Service 时必须使用与 Vault Service 用一个DID身份创建，这样才可以将对应该 DID 身份对应的vault 数据备份到 Backup service中来。 为防止单点失效，务必使用与待备份的 Vault Service 不再同一个Hive Node 创建订阅 Backup service。

Attention: when you create a subscription Backup Service, you must use the same DID identity as the Vault Service, so that the vault data corresponding to the DID identity can be backed up into the Backup service. To avoid single point of failure, it is important to create a subscription Backup service with the Hive Node that is no longer the same as the Vault Service to be backed up.

## Subscription management

### Check Subscription

用户在可信 Hive Node 上订阅创建后，可以使用 backup 对象来存取应用中的数据，也可以在使用以下接口查看对应 Backup Service 的信息。

After subscribing to the trusted Hive Node, users can use the backup object to access the data in the application, or view the information of the corresponding Backup Service by using the following interface.

```java
subscription.checkSubscription().thenAccept(backup -> {
    System.out.println("Dump remote backup service:");
    System.out.println("service DID ==> " + backup.getServiceDid());
    System.out.println("storage quota ==> " + backup.getStorageQuota());
    System.out.println("price plan ==> " + backup.getPricePlan());
}).exceptinally(ex -> {
    System.out.println("Failed to check backup subscription");
    ex.printStackTrace();
    return null;
});
```

### Unsubscription

用户在使用Backup Service 一段时间，如果发现体验不好，发现另外有可信的 Hive Node 能提供更好的 Backup Service, 则可以取消订阅并销毁该 Backup中的数据，转向另外可信的 Hive Node 节点订阅新的 Backup Service。

After using the Backup Service for a period, if the user finds that the experience is not good and another trusted Hive Node can provide a better Backup Service, the data in the Backup can be unsubscribed and destroyed, then another trusted Hive Node can be chosen to subscribe for a new Backup Service.

```java
subscription.unsbuscribe().thenAccept(() -> {
    System.out.println("Unsubscribed from the backup service");
    System.out.println("Cleared all data in this backup");
}).exceptinally(ex -> {
    System.out.println("Failed to unsubscribe to backup service");
    ex.printStackTrace();
    return null;
});
```

注意：调用此接口销毁 Backup Service 后，该backup 内持有数据从对应 Hive Node 上被永久删除，同时该 Backup Serivce 也停止服务。

Attetion: after calling this interface to destroy the Backup Service, the data held in the backup will be permanently deleted from the corresponding Hive Node, and the Backup Service will stop serving as well.
