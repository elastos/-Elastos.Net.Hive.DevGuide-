# ScriptingService

## ScriptingService

Hive Node 上，为了支持 Vault 数据分享给指定或者所有的访问者，提供了Scripting Service。Scripting Service 通过提供可执行脚本来将 Vault 中的数据（文件或数据库数据）分享给其他人。数据的拥有者设计、注册脚本，数据的使用者执行脚本操作数据。

注册脚本的例子如下：

```shell
Vault vault = new Vault(context, getVaultProviderAddress());
ScriptingService scriptingService = vault.getScriptingService();

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
scriptingService.registerScript(ScriptConst.SCRIPT_NAME,
        new QueryHasResultCondition("get_group_message", ScriptConst.COLLECTION_GROUP, filter),
        new InsertExecutable(ScriptConst.SCRIPT_NAME, ScriptConst.COLLECTION_GROUP_MESSAGE, msgDoc, options),
        false, false)
    .thenAcceptAsync(result -> {
        System.out.println("Successfully get the result.");
    }).exceptionally(ex -> {
        ex.printStackTrace();
        return null;
    });
```

该例子演示了如何注册一个脚本，注册脚本的时候需要设置条件（ condition ），用于限制数据的使用者。另外就是脚本的内容（ executable ）为一个模版，允许调用脚本的用户可以往数据表里插入内容（类型为 InsertExecutable ）。同时脚本里面限制了数据的访问者能访问的数据（$params定义的参数）。注册完脚本后，满足条件的访问者可以通过执行脚本来插入数据。此处的使用场景为，往数据拥有者的 Vault 里面发送消息。

### Register Script

按照Scripting服务的设计，脚本的拥有者可以注册8种不同类型的脚本，分享文件内容和执行数据库操作。

* 插入数据库数据
* 更新数据库数据
* 删除数据库数据
* 查询数据库数据
* 上传文件
* 下载文件
* 获取文件属性信息
* 获取文件Hash值

涉及到具体类型的脚本数据包含两块内容：

* condition：对访问者设置的条件，访问者执行脚本的时候，满足条件才能得到正确的执行效果
* executable: 脚本的定义区域，定义了指定类型脚本的类型、参数模版等

下面是一个注册脚本的方法原型。注册脚本的参数有：脚本的名称、执行脚本的条件、脚本内容、是否允许匿名用户、是否允许匿名应用

```java
CompletableFuture<Void> registerScript(String name, Executable executable);
CompletableFuture<Void> registerScript(String name, Condition condition, Executable executable);
CompletableFuture<Void> registerScript(String name, Executable executable,
        boolean allowAnonymousUser, boolean allowAnonymousApp);
CompletableFuture<Void> registerScript(String name, Condition condition, Executable executable,
        boolean allowAnonymousUser, boolean allowAnonymousApp);
```

### Unregister Script

移除指定名称的脚本，移除之后脚本将不在可执行。

```java
CompletableFuture<Void> unregisterScript(String name);
```

## ScriptRunner

按照 Scripting Service的说明，数据的访问者若是需要执行脚本，只能通过 Script Runner来执行。与上面注册脚本相对应的执行脚本的例子如下：

```java
ScriptRunner scriptRunner = new ScriptRunner(callerContext, getVaultProviderAddress());
ObjectNode params = JsonNodeFactory.instance.objectNode();
params.put("author", "John");
params.put("content", "message");
scriptRunner.callScript(ScriptConst.SCRIPT_NAME, params, userDid, appDid, JsonNode.class)
    .thenAcceptAsync(result -> {
            System.out.println("Successfully get the result.");
        }).exceptionally(ex -> {
            ex.printStackTrace();
            return null;
        });
```

Script Runner 用到的参数和创建 Vault 的时候一样，只是提供的 user DID 属于数据的访问者。执行脚本的时候，就是设置好脚本模版的参数、指定脚本的拥有者，然后通过名称执行脚本，操作数据。

### Run Script

执行脚本操作就如上面的示例一样，除了提供必要的参数值之外，还需要指定返回值类型（resultType）

```java
<T> CompletableFuture<T> callScript(String name, JsonNode params,
        String targetDid, String targetAppDid, Class<T> resultType);
```

### Upload File

注册脚本时候的八种脚本类型，有两种情况是需要多执行一步：上传、下载文件。执行上传文件脚本的时候，会先返回事务 ID，然后再调用上传接口将文件上传到脚本对应的 Vault 中，此时 resultType 才是上传类型脚本执行的返回值。

```java
<T> CompletableFuture<T> uploadFile(String transactionId, Class<T> resultType);
```

### Download File

下载文件和上传文件的操作流程类似，通过事务 ID，下载到指定文件的内容。

```java
<T> CompletableFuture<T> downloadFile(String transactionId, Class<T> resultType);
```
