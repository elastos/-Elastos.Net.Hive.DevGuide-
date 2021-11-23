# Sample Code

Hello Application 不同于测试用例，它是真实应用场景下如何使用 Hive Java SDK 的用例。本文将介绍 Hello Application 内上传文件的示例：

## Upload File

上传文件的功能是用于展示Files Service的用法，通过 Files Service 可以对 Vault 内的文件进行操作。订阅 Vault Service 之后，利用 Files Service 上传文件，大致分为如下几步：

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
