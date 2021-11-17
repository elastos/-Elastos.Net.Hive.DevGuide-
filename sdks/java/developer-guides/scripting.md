# Scripting

Scripting服务也是Vault服务的一部分，它通过提供脚本（Script）的形式，将拥有的数据分享给其它用户。数据拥有者注册脚本，数据使用者调用脚本获取拥有者的数据。

## Scripting服务和ScriptRunner服务

获取Scripting服务同样是通过Vault对象进行的，而且Scripting服务是为数据的拥有者使用。ScriptRunner服务则直接创建即可，为数据的使用者使用。创建Scripting服务和ScriptRunner服务的例子如下。其中调用者的AppContext和Vault的AppContext是一样的，只是用到的user DID为脚本调用者的DID。

```shell
public Vault newVault() {
    return new Vault(context, getVaultProviderAddress());
}

scriptingService = testData.newVault().getScriptingService();

public ScriptRunner newCallerScriptRunner() {
    return new ScriptRunner(callerContext, getVaultProviderAddress());
}
```

## 注册脚本

按照Scripting服务的设计，脚本的拥有者可以注册8种不同类型的脚本，分享文件内容和执行数据库操作。

- 插入数据库数据
- 更新数据库数据
- 删除数据库数据
- 查询数据库数据
- 上传文件
- 下载文件
- 获取文件属性信息
- 获取文件Hash值

涉及到具体类型的脚本数据包含两块内容：

- condition：对访问者设置的条件，访问者执行脚本的时候，满足条件才能得到正确的执行效果
- executable: 脚本的定义区域，定义了指定类型脚本的类型、参数模版等

下面是一个注册脚本的例子：

- 脚本的类型是插入数据库数据
- 设置的condition说明，调用者必须在表COLLECTION_GROUP中有权限
- 设置的2个参数说明，调用者只能插入2个字段的数据

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

## 调用脚本

调用者并非数据的拥有者，欲调用脚本必须使用ScriptRunner服务。下面是执行脚本的例子，脚本的执行会用到以下几个参数：

- 脚本的名称
- 数据的拥有者：user DID和app DID
- 指定脚本参数的值

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
