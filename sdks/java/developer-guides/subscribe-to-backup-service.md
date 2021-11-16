# Subscribe to backup service

Backup服务用于将Vault上的文件和数据库数据备份到另一个Hive Node，确保数据因为某种原因丢失后可以恢复。Backup服务和Vault服务位于不同的节点，若需要备份Vault数据到另一个Hive Node节点上，必须在另一个Hive Node节点上订阅Backup服务。Backup服务订阅功能位于类BackupSubscription中，创建BackupSubscription对象的语句如下。创建BackupSubscription对象同样需要参数AppContext和Hive Node的URL。

```java
public BackupSubscription newBackupSubscription() throws HiveException {
    return new BackupSubscription(context, getBackupProviderAddress());
}
```

创建好BackupSubscription对象之后，就可以直接调用订阅接口订阅了，不需要任何参数。

```java
void testSubscribe() {
    Assertions.assertDoesNotThrow(()->subscription.subscribe().get());
}
```

在备份的Hive Node端订阅完之后，就可以在Vault端通过Backup服务备份Vault数据了。
