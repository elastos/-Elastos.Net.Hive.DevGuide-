# Sample Code

Hello App不同于测试用例，它是真实App场景下如何使用Hive Java SDK的用例。本文将全面介绍Hello App和SDK相关内容。

## Update User DID

用户DID被用作作为用户的唯一凭证，它由类UserDID负责管理，使用该类仅需提供几个参数：名称、mnemonic、pass phrase、storepass, 名称用于DID分类，其它几个参数和Essentials App里面导入DID用到的一样。

```java
userDid = new UserDID(userConfig.name(), 
                      getMnemonicStr(userConfig), 
                      getPassPhraseStr(userConfig), 
                      userConfig.storepass());
```

App DID用作标识应用的唯一凭证，它保存于类AppDID的appId字段。App Instance DID用于标识应用安装在某个机器上的一个实例，它与App DID一样，会用作Hive Node登陆。App Instance DID由类AppDID负责管理，它用到的参数和类UserDID一样：

```java
appInstanceDid = new AppDID(applicationConfig.name(),
                            applicationConfig.mnemonic(),
                            applicationConfig.passPhrase(),
                            applicationConfig.storepass());
```

使用SDK时，仅需要设置用户DID、App DID和App Instance DID。用户调用SDK接口，会根据接口的类型，隐式的调用登陆接口，获取功能接口访问的Token。

Hello App中，更新User DID实际上是更新用户DID的mnemonic、pass phrase设置。具体的代码实现中，这2个设置是保存在Android应用的设置里面的，若有设置则取该设置，否则取配置文件内的默认User DID配置。

## Vault Subscribe

使用Hive Node上服务之前，需要先订阅Vault，获取到存储的空间和权限。欲实现Vault的订阅，只需要执行如下几步：

- 创建VaultSubscription对象

```java
public VaultSubscription newVaultSubscription() throws HiveException {
    return new VaultSubscription(context, getVaultProviderAddress());
}
```

- 在执行线程里面调用订阅Vault功能，此处会UI上显示转圈，提示订阅执行中

```java
public void subscribeVault() {
    this.showLoading();
    this.vaultSubscription.subscribe().whenCompleteAsync((result, e)-> hideLoadingWithMessage(e));
}
```

## Upload File

上传文件的功能是用于展示文件服务的用法。在订阅了Vault之后，文件服务边可以用来上传文件了，大致分为如下几步：

- 创建Vault对象

```java
public Vault newVault() {
    return new Vault(context, getVaultProviderAddress());
}
```

- 获取FilesService对象

```java
this.filesService = mainActivity.getSdkContext().newVault().getFilesService();
```

- 上传文件，此处串联CompletableFuture，并将上传操作放在异步线程里执行

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

脚本机制是让Vault数据的拥有者（Owner）分享数据给使用者（Caller）。在Hello App里面，构建了一个使用场景：

- 两张数据表，一张用于存储调用者权限，另一张用于调用者存储消息
- 拥有者为调用者设置权限，指定某个调用者可以存储消息
- 拥有者设置脚本，为调用者开放数据操作接口
- 调用者调用设置的脚本，向数据拥有者的表内存储消息

下面是具体的实现流程：

- 连续创建2个数据表

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

作为调用者，首先需要获取ScriptRunner对象，然后直接调用脚本即可。

- 创建ScriptRunner对象

```java
public ScriptRunner newCallerScriptRunner() {
    return new ScriptRunner(contextCaller, getVaultProviderAddress());
}
```

- 通过脚本名称，调用脚本

```
public CompletableFuture<JsonNode> runScript() {
    ObjectNode params = JsonNodeFactory.instance.objectNode();
    params.put("author", "John");
    params.put("content", "message");
    return scriptRunner.callScript(ScriptConst.SCRIPT_NAME,
            params, userDid, appDid, JsonNode.class);
}
```
