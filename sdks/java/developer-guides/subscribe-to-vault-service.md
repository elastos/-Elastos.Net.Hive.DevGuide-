# VaultSubscription

Hive SDK 需要通过 Vault 对象与 Hive Node 交互。用户需要预先在指定的 Hive Node 上订阅创建 Vault Service，然后才可以将应用数据存放到对应的 Vault 中，VaultSubscription 则是用于订阅 Vault Service。

使用 VaultSubscription 订阅 Vault Service 的例子如下：

```java
VaultSbuscription vault = new VaultSubscription(context, getVaultProvider());
vault.subscribe().thenAccept(result -> {
    System.out.println("Successfully get the result.");
}).exceptionally(ex -> {
    ex.printStackTrace();
    return null;
});
```

以上的例子，调用的是订阅接口，创建 VaultSubscription 对象时，需要提供如下两个参数：

- context：用户定义的应用上下文，基于接口 AppContextProvider。具体由用户提供三块信息：本地数据存储的位置、App Instance DID、登录时用到的授权信息。
- Vault Provider Address: Hive Node的访问地址。

执行完订阅 Vault Service 之后，就可以使用 Vault 相关的服务了。

## Subscribe

订阅操作的原型如下，返回的是一个 CompletableFuture 对象，方便调用者在异步线程中执行订阅操作。此处的 T 代表的类型 VaultInfo （ Vault Service 的信息）。

```java
CompletableFuture<T> subscribe();
```

## Unsubscribe

取消订阅是订阅方式的逆向，它会销毁 Vault 内的所有数据，取消订阅 Vault 之后，Vault 内的服务将不在可用。

```java
CompletableFuture<Void> unsubscribe();
```

## Check Subscription

若是想知道当前的订阅信息，可以使用如下方法，它会返回 VaultInfo （ Vault 的信息）。

```java
CompletableFuture<T> checkSubscription();
```
