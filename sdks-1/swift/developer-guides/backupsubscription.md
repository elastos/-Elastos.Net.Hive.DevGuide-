# BackupSubscription

Considering the redundancy of data on the Hive Node, it's important to keep multiple backups of data in the Vault. In case the corresponding Hive Node fails, other trusted hive nodes can be selected to restore and generate the Vault Service that can continue to serve. BackupSubscription class is used to subscribe to the trusted Hive Node to create a Backup Service. After creation, it can be used to back up the data in the established Vault Service to the Backup Service.

BackupSubscription class is used to create Backup Service by subscription, and Hive Node service creates a new Backup Service according to the DID identity of the requester. If the Backup Service based on the DID identity already exists, the metadata information of the existing Backup Service is returned.

## Examples

Use BackupSubscription instance to create new Backup Service in the trusted Hive Node subscription, and return a Promise object containing metadata information of the remote Backup Service of the subscription. The actual sample code is as follows:

```swift
let subscription = try BackupSubscription(appContext, providerAddress)
subscription!.subscribe()
.done { backup in
   print("Registered backup service successfully.")
   print("service DID ==> \(backup.serviceDid)")
   print("storage quota ==> \(backup.storageQuota)")
   print("price plan ==> \(backup.pricePlan)")
}
.catch { error in
   print("Failed to register backup service")
   print(error)
}
```

After successfully subscribing to the Backup Service, the Vault data held by the DID identity can be backed up to the Hive Node nodes through it.

**Attention:** When you create a subscription Backup Service, you must use the same DID as the Vault Service so that the vault data corresponding to the DID identity can be backed up into the Backup service. To avoid a single point of failure, it's important to create a subscription Backup service with the Hive Node that is no longer the same as the Vault Service to be backed up.

## Subscription management

### Check Subscription

After subscribing to the trusted Hive Node, the user can use the backup object to access the data in the application or view the information of the corresponding Backup Service by using the following interface.

```swift
subscription!.checkSubscription()
.done { backup in
   print("Dump remote backup service:")
   print("service DID ==> \(backup.serviceDid)")
   print("storage quota ==> \(backup.storageQuota)")
   print("price plan ==> \(backup.pricePlan)")
}
.catch { error in
   print("Failed to check backup subscription")
   print(error)
}
```

### Unsubscription

After using the Backup Service for a period, if the user finds that the experience is not good and another trusted Hive Node can provide a better Backup Service, the data in the Backup can be unsubscribed and destroyed; then, another trusted Hive Node can be chosen to subscribe for a new Backup Service.

```swift
subscription!.unsubscribe()
.done { success in
   print("Unsubscribed from the backup service")
   print("Cleared all data in this backup")
}
.catch { error in
   print(error)
}
```

**Attention:** After calling this interface to destroy the Backup Service, the data held in the backup will be permanently deleted from the corresponding Hive Node, and the Backup Service will stop serving as well.
