# Sample Code

Hello App 不同于测试用例，它是真实 App 场景下如何使用 Hive Java SDK 的用例。本文将全面介绍 Hello App 和 SDK 相关内容。

## Update User DID

用户 DID 被用作作为用户的唯一凭证，它由类 UserDID 负责管理，使用该类仅需提供几个参数：名称、mnemonic、pass phrase、storepass, 名称用于 DID 分类，其它几个参数和 Essentials App 里面导入DID用到的一样。

```java
UserDID userDid = new UserDID(userConfig.name(), 
        getMnemonicStr(userConfig),
        getPassPhraseStr(userConfig),
        userConfig.storepass());
```

App DID 用作标识应用的唯一凭证，它保存于类 AppDID 的appId字段。App Instance DID 用于标识应用安装在某个机器上的一个实例，它与 App DID 一样，会用作 Hive Node 登陆。App Instance DID 由类 AppDID 负责管理，它用到的参数和类 UserDID 一样：

```java
AppDID appInstanceDid = new AppDID(applicationConfig.name(),
        applicationConfig.mnemonic(),
        applicationConfig.passPhrase(),
        applicationConfig.storepass());
```

使用SDK时，仅需要设置用户 DID、App DID 和 App Instance DID。用户调用 SDK 接口，会根据接口的类型，隐式的调用登陆接口，获取功能接口访问的 Token。

Hello App 中，更新 User DID 实际上是更新用户 DID 的 mnemonic、pass phrase 设置。具体的代码实现中，这2个设置是保存在 Android 应用的设置里面的，若有设置则取该设置，否则取配置文件内的默认 User DID 配置。

## Vault Subscribe

使用 Hive Node 上的 Vault Service 之前，需要先订阅 Vault Service，获取到存储空间和权限。欲实现 Vault Service 的订阅，只需要执行如下几步：

- 创建VaultSubscription对象

```java
public VaultSubscription newVaultSubscription() throws HiveException {
    return new VaultSubscription(context, getVaultProviderAddress());
}
```

- 在异步线程里面调用订阅 Vault Service 功能，此处会界面上显示转圈，提示订阅执行中

```java
public void subscribeVault() {
    this.showLoading();
    this.vaultSubscription.subscribe().whenCompleteAsync((result, e)-> hideLoadingWithMessage(e));
}
```

## Upload File

上传文件的功能是用于展示文件服务的用法。在订阅了 Vault Service 之后，文件服务边可以用来上传文件了，大致分为如下几步：

- 创建 Vault 对象

```java
public Vault newVault() {
    return new Vault(context, getVaultProviderAddress());
}
```

- 获取 FilesService 对象

```java
this.filesService = mainActivity.getSdkContext().newVault().getFilesService();
```

- 上传文件，此处串联 CompletableFuture，并将上传操作放在异步线程里执行

```java
public CompletableFuture<Void> writeFileContent(Writer writer) {
    return CompletableFuture.runAsync(() -> {
        try {
            writer.write(REMOTE_FILE_CONTENT);
            writer.flush();
            writer.close();
        } catch (IOException e) {
            throw new CompletionException(e);
        }
    });
}

public void uploadFile() {
    this.showLoading();
    filesService.getUploadWriter(REMOTE_FILE_PATH)
            .thenCompose(this::writeFileContent)
            .whenComplete((result, e)-> hideLoadingWithMessage(e));
}
```

## Register Script

脚本机制是让 Vault Service 数据的拥有者（Owner）分享数据给使用者（Caller）。在 Hello App 里面，构建了一个使用场景：

- 两张数据表，一张用于存储调用者权限，另一张用于调用者存储消息
- 拥有者为调用者设置权限，指定某个调用者可以存储消息
- 拥有者设置脚本，为调用者开放数据操作接口
- 调用者调用设置的脚本，向数据拥有者的表内存储消息

下面是具体的实现流程：

- 连续创建两个数据表

```java
databaseService = this.sdkContext.newVault().getDatabaseService();

private CompletableFuture<Void> createTwoCollections() {
    return databaseService.createCollection(ScriptConst.COLLECTION_GROUP)
            .thenCompose(result->databaseService.createCollection(ScriptConst.COLLECTION_GROUP_MESSAGE));
}
```

- 为调用者设置另一张表的权限

```java
private CompletableFuture<InsertResult> addPermission2Caller() {
    // The document to save the permission of the user DID.
    ObjectNode conditionDoc = JsonNodeFactory.instance.objectNode();
    conditionDoc.put("collection", ScriptConst.COLLECTION_GROUP_MESSAGE);
    conditionDoc.put("did", callDid);
    // Insert the permission to COLLECTION_GROUP
    return databaseService.insertOne(
            ScriptConst.COLLECTION_GROUP,
            conditionDoc,
            new InsertOptions().bypassDocumentValidation(false));
}
```

- 拥有者为调用者设置脚本

```java
private CompletableFuture<Void> doSetScript() {
    // The condition to restrict the user DID.
    ObjectNode filter = JsonNodeFactory.instance.objectNode();
    filter.put("collection",ScriptConst.COLLECTION_GROUP_MESSAGE);
    filter.put("did","$caller_did");
    // The message is for inserting to the COLLECTION_GROUP_MESSAGE.
    ObjectNode msgDoc = JsonNodeFactory.instance.objectNode();
    msgDoc.put("author", "$params.author");
    msgDoc.put("content", "$params.content");
    ObjectNode options = JsonNodeFactory.instance.objectNode();
    options.put("bypass_document_validation", false);
    options.put("ordered", true);
    // register the script for caller to insert message to COLLECTION_GROUP_MESSAGE
    return scriptingService.registerScript(ScriptConst.SCRIPT_NAME,
            new QueryHasResultCondition("verify_user_permission", ScriptConst.COLLECTION_GROUP, filter),
            new InsertExecutable(ScriptConst.SCRIPT_NAME, ScriptConst.COLLECTION_GROUP_MESSAGE, msgDoc, options),
            false, false);
}
```

- 前面三步连起来，按顺序执行

```java
public CompletableFuture<Void> setScript() {
    return cleanTwoCollections()
            .thenCompose(result->createTwoCollections())
            .thenCompose(result->addPermission2Caller())
            .thenCompose(result->doSetScript());
}
```

## Run Script

作为调用者，首先需要获取 ScriptRunner 对象，然后直接调用脚本即可。

- 创建 ScriptRunner 对象

```java
public ScriptRunner newCallerScriptRunner() {
    return new ScriptRunner(contextCaller, getVaultProviderAddress());
}
```

- 通过脚本名称，执行脚本

```
public CompletableFuture<JsonNode> runScript() {
    ObjectNode params = JsonNodeFactory.instance.objectNode();
    params.put("author", "John");
    params.put("content", "message");
    return scriptRunner.callScript(ScriptConst.SCRIPT_NAME,
            params, userDid, appDid, JsonNode.class);
}
```
