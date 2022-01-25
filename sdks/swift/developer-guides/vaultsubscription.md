# VaultSubscription

Hive SDK needs to interact with the remote Vault Service through the Vault instance. Users subscribe to the trusted Hive Node to create Vault Service in advance, and then they can access the application data to the corresponding Vault.

VaultSubscription class is used to create a Vault Service by subscription, and the Hive Node service creates a new Vault Service according to the DID identity of the requester. If the Vault Service based on the DID identity already exists, the existing Vault Service metadata will be returned.

## Example

Use the vaultSubscription instance to create a new Vault Service in the trusted Hive Node subscription and return a CompletableFuture object containing the metadata information of the subscribed remote Vault Service. The actual sample code is as follows:

```swift
let subscription = try VaultSubscription(testData.appContext, testData.providerAddress)
subscription.subscribe()
.done { vault in
   print("Registered vault service successfully.")
   
   print("service DID ==> \(vaultInfo.serviceDid)")
   print("storage quota ==> " + vaultInfo.getStorageQuota());
   print("price plan ==> \(vaultInfo.pricePlan)")
}
.catch { error in
   print("Failed to register vault service")
   print(error)
}
```

Once the subscription is successful, the instantiation object vault can be used to access the application data.

## Subscription Management

### Check Subscription

After subscribing to the trusted Hive Node, the user can use the vault instance to access the data in the application, or view the information of the corresponding Vault Service by using the following interface.

```swift
subscription.checkSubscription()
.done { vault in
   print("Dump remote vault service:")
   print("service DID ==> \(vaultInfo.serviceDid)")
   print("storage quota ==> \(vaultInfo.storageQuota)")
   print("price plan ==> \(vaultInfo.pricePlan)")
}
.catch { error in
   print("Failed to check vault subscription")
   print(error)
}
```

### Unsubscription

Users can use the Vault Service for a period and migrate the already used Vault Service to the new Hive Node as needed. After the migration, it is necessary to stop the Vault Service on the original Hive Node and destroy the data within.

```swift
subscription.unsubscribe()
.done { success in
   print("Unsubscribed from the vault service")
   print("Cleared all data in this vault.")
}
.catch { error in
   print("Failed to unsubscribe to vault service")
   print(error)
}
```

**Attention:** after calling this interface to cancel the vault Service, the data held in the vault will be permanently deleted from the corresponding Hive Node, and the Vault Service will stop serving as well. The user must ensure that a new Vault service has been migrated before calling this interface to cancel the vault service.
