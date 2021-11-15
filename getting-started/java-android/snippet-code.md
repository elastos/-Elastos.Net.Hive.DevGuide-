# Snippet Code

关于如何使用Hive Java SDK，可以参考SDK的测试用例，以下仅列出测试用例的一些代码片段。

## Vault

为了能使用Hive Node的服务，首先必须先订阅（subscribe）Vault。订阅Vault意味者在Hive Node端有文件存储的空间，数据库的使用权限等。
为了订阅Vault，首先要创建VaultSubscription对象。

```java
public VaultSubscription newVaultSubscription() throws HiveException {
    return new VaultSubscription(context, getVaultProviderAddress());
}
```

有了VaultSubscription对象，便可以通过它来订阅Vault。

```java
@Test @Order(3) void testSubscribe() {
    Assertions.assertDoesNotThrow(()->subscription.subscribe().get());
}
```

## Database

订阅了Vault之后，便可以使用Vault下面的服务，Database服务便是其中之一。
为了使用Database服务，需要先创建Vault对象。

```java
public Vault newVault() {
    return new Vault(context, getVaultProviderAddress());
}
```

然后，通过Vault对象，获取Database服务。

```java
@BeforeAll public static void setUp() {
    trySubscribeVault();
    Assertions.assertDoesNotThrow(()->databaseService = TestData.getInstance().newVault().getDatabaseService());
}
```

最后就是使用Database服务，比如插入一条数据：

```java
@Test @Order(2) void testInsertOne() {
    Assertions.assertDoesNotThrow(()->{
        ObjectNode docNode = JsonNodeFactory.instance.objectNode();
        docNode.put("author", "john doe1");
        docNode.put("title", "Eve for Dummies1");
        Assertions.assertNotNull(databaseService.insertOne(COLLECTION_NAME, docNode,
                new InsertOptions().bypassDocumentValidation(false)).get());
    });
}
```

## Files

文件服务和Database服务使用流程是一样的，也是需要先创建Vault对象：

```java
public Vault newVault() {
    return new Vault(context, getVaultProviderAddress());
}
```

再获取文件服务：

```java
@BeforeAll public static void setUp() {
    trySubscribeVault();
    Assertions.assertDoesNotThrow(()->filesService = TestData.getInstance().newVault().getFilesService());
}
```

最后使用文件服务，比如上传文件：

```java
@Test @Order(1) void testUploadText() {
    Assertions.assertDoesNotThrow(this::uploadTextReally);
    verifyRemoteFileExists(remoteTxtFilePath);
}

private void uploadTextReally() throws IOException, ExecutionException, InterruptedException {
    try (Writer writer = filesService.getUploadWriter(remoteTxtFilePath).get();
         FileReader fileReader = new FileReader(localTxtFilePath)) {
        Assertions.assertNotNull(writer);
        char[] buffer = new char[1];
        while (fileReader.read(buffer) != -1) {
            writer.write(buffer);
        }
    }
}
```

## Scripting

Scripting服务的设置目标，是希望Vault的拥有者，能将其拥有的数据和文件分享给指定的人或群体，甚至公开访问权限。
其使用流程和Database服务也类似，不同的是服务的使用者分为数据的拥有者和数据的访问者。

数据的拥有者需要先创建Vault对象：

```java
public Vault newVault() {
    return new Vault(context, getVaultProviderAddress());
}
```

然后获取到Scripting服务：

```java
scriptingService = testData.newVault().getScriptingService();
```

然后注册脚本给数据的访问者调用。Scripting服务内部设置了多种脚本类型，数据的拥有者需要先设置脚本，并指定权限：

```java
private void register_script_for_caller() {
    Assertions.assertDoesNotThrow(()->{
        ObjectNode filter = JsonNodeFactory.instance.objectNode();
        filter.put("collection", COLLECTION_GROUP_MESSAGE);
        filter.put("did", "$caller_did");
        ObjectNode doc = JsonNodeFactory.instance.objectNode();
        doc.put("author", "$params.author");
        doc.put("content", "$params.content");
        ObjectNode options = JsonNodeFactory.instance.objectNode();
        options.put("bypass_document_validation",false);
        options.put("ordered",true);
        scriptingService.registerScript(SCRIPT_NAME,
                new QueryHasResultCondition("verify_user_permission", COLLECTION_GROUP, filter),
                new InsertExecutable(SCRIPT_NAME, COLLECTION_GROUP_MESSAGE, doc, options),
                false, false).get();
    });
}
```

数据的访问者可以通过脚本的名称调用脚本，但使用流程和数据的拥有者不同，他需要先获取ScriptRunner对象：

```java
public ScriptRunner newCallerScriptRunner() {
    return new ScriptRunner(callerContext, getVaultProviderAddress());
}
```

然后执行脚本：

```java
private void run_script_without_group_permission() {
    Assertions.assertDoesNotThrow(()->{
        ObjectNode params = JsonNodeFactory.instance.objectNode();
        params.put("author", "John");
        params.put("content", "message");
        JsonNode result = scriptRunner.callScript(SCRIPT_NAME, params, targetDid, appDid, JsonNode.class).get();
        Assertions.assertNotNull(result);
        Assertions.assertTrue(result.has(SCRIPT_NAME));
        Assertions.assertTrue(result.get(SCRIPT_NAME).has("inserted_id"));
    });
}
```

## Backup

当前SDK支持的备份功能，是从Hive Node A将用户Vault里的数据备份到Hive Node B。
备份的流程是在Hive Node A完成的，同样也是先获取Vault对象内部的备份服务：

```java
public Vault newVault() {
    return new Vault(context, getVaultProviderAddress());
}

Assertions.assertDoesNotThrow(()->backupService = TestData.getInstance().getBackupService());
```

为了能成功执行备份操作，需要在Hive Node B端订阅备份服务：

```java
public BackupSubscription newBackupSubscription() throws HiveException {
    return new BackupSubscription(context, getBackupProviderAddress());
}

private static void trySubscribeBackup() {
    Assertions.assertDoesNotThrow(()->backupSubscription = TestData.getInstance().newBackupSubscription());
    try {
        backupSubscription.subscribe();
    } catch (NotFoundException e) {}
}
```

最后在Hive Node A端执行备份操作：

```java
@Test @Order(1) void testStartBackup() {
    Assertions.assertDoesNotThrow(()->backupService.startBackup().get());
}
```

若是要将备份的vault数据在Hive Node B端升级为一个Vault，此时相当于将Hive Node A的Vault迁移到Hive Node B。
需要Backup对象，并获取Promotion服务。

```java
public Backup newBackup() {
    return new Backup(context, getBackupProviderAddress());
}

@BeforeAll public static void setup() {
    Assertions.assertDoesNotThrow(()->promotionService = TestData.getInstance().newBackup().getPromotionService());
}
```

然后就是执行升级操作：

```java
@Test void testPromote() {
    Assertions.assertDoesNotThrow(()->promotionService.promote().get());
}
```

## Payment

支付服务用于升级Vault或Backup的存储空间，支付的金额将由Hive Node的拥有者收取。
支付服务通过VaultSubscription对象获取，

```java
public VaultSubscription newVaultSubscription() throws HiveException {
    return new VaultSubscription(context, getVaultProviderAddress());
}

Assertions.assertDoesNotThrow(()->paymentService = TestData.getInstance().newVaultSubscription());
```

然后就可以使用支付服务下的功能了，比如下支付订单：

```java
void testPlaceOrder() {
    Assertions.assertDoesNotThrow(()->{
        Order order = paymentService.placeOrder(PRICING_PLAN_NAME).get();
        Assertions.assertNotNull(order);
        Assertions.assertNotNull(order.getOrderId());
    });
}
```
