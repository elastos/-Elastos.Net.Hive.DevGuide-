# VaultSubscription

Hive SDK needs to interact with the remote Vault service through the Vault instance. Users subscribe to the trusted Hive Node to create Vault service in advance; then they can access the application data to the corresponding Vault.

The VaultSubscription class is used to create a Vault Service by subscription, and the Hive Node service creates a new Vault Service according to the DID identity of the requester. If the Vault Service based on the DID identity already exists, the existing Vault Service metadata will be returned.

## Example

Use the vaultSubscription instance to create a new Vault service in the trusted Hive Node subscription and return a CompletableFuture object containing the metadata information of the subscribed remote Vault Service. The actual sample code is as follows:

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

Once the subscription is successful, the instantiation object vault can be used to access the application data.

## Subscription Management

### Check Subscription

After subscribing to the trusted Hive Node, users can use the vault instance to access the data in the application or view the information of the corresponding Vault Service by using the following interface.

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

### Unsubscribe

Users can use the Vault service for a period of time and migrate the already used Vault service to the new Hive Node as needed. After the migration, it's important to stop the Vault service on the original Hive Node and destroy the data within it.

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

**Attention:** After calling this interface to cancel the Vault service, the data held in the vault will be permanently deleted from the corresponding Hive Node, and the Vault service will stop serving as well. Users must ensure that a new Vault service has been migrated before calling this interface to cancel the Vault service that originally needed to be abandoned.
