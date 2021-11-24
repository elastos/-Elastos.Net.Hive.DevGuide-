# VaultSubscription

Hive SDK 需要通过 Vault 对象与 远程的Vault Service 进行交互。用户需要预先在指定可信的 Hive Node 上订阅创建 Vault Service，然后才能将应用数据存放到对应的 Vault 中.

VaultSubscription 类就是用来定订阅创建 Vault Service，Hive Node 服务根据请求者的DID身份创建新的 Vault Service。如果基于该DID身份的 Vault Service 已经存在，则返回已存在的 Vault Service.

## Example

使用 vaultSubscription 对象在可信的Hive Node 订阅创建新的 Vault Service，返回一个 CompletableFuture 对象，包含该订阅的远端 Vault Service的元数据信息。实际样例代码如下（ context 和 vault provider 的设置参见[Developer Guide](README.md) ）：

```java
VaultSbuscription subscription = new VaultSubscription(context, getVaultProvider());
subscription.subscribe().thenAccept(vaultInfo -> {
    System.out.println("Registered vault service successfully");
    System.out.println("vaultinfo ==>");
}).exceptionally(ex -> {
    System.out.println("Failed to register vault service")
    ex.printStackTrace();
    return null;
});
```

一旦订阅成功，就可以使用实例化对象 vault 来存取应用数据。

## Subscription management

### Check Subscription

用户在可信 Hive Node 上订阅创建后，可以使用 vault 对象来存取应用中的数据，也可以在使用以下接口查看对应 Vault Service 的信息。

```java
subscription.checkSubscription().thenAccept(vaultInfo -> {
    System.out.println("Dump remote vault service:");
    System.out.println("...");
}).exceptinally(ex -> {
    System.out.println("Failed to check vault subscription");
    ex.printStackTrace();
    return null;
});
```

### Unsubscription

用户在使用Vault Service 一段时间，如果发现体验不好，发现另外有可信的 Hive Node 能提供更好的 Vault Service，或者用户自己搭建运行了Hive Node，需要将已使用中 Vault Service 迁移到新的 Hive Node中，迁移完成后需要停止原来 Hive Node上的 Vault Service, 同时销毁其中数据。

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

注意：调用此接口取消 vault Service 后，该vault内持有数据从对应 Hive Node 上被永久删除，同时该 Vault Serivce 也停止服务。用户必须先保证已有新的 Vault service 完成迁移后，再可调用该接口取消 原来需要废弃的vault service。

### Get Pricing Plans

为了能够升级 Vault 的存储空间，VaultSubscription 支持获取可以升级的计划套餐。获取到的列表是 Hive Node 在部署的时候，设置好的，同时也必须是 Hive Node 支持的套餐类型。

```java
subscription.getPricingPlanList().thenAccept(list -> {
    System.out.println("Get pricing plans successfully.");
    System.out.println("List<PricingPlan> =>");
}).exceptinally(ex -> {
    System.out.println("Failed to get pricing plans");
    ex.printStackTrace();
    return null;
});
```

### Get Pricing Plan

如果知道价格计划的名字，也可以通过它获取详情。

```java
subscription.getPricingPlan(PRICING_PLAN_NAME).thenAccept(pricingPlan -> {
    System.out.println("Get pricing plan successfully.");
    System.out.println("PricingPlan =>");
}).exceptinally(ex -> {
    System.out.println("Failed to get pricing plan");
    ex.printStackTrace();
    return null;
});
```
