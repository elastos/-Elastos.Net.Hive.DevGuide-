# FilesService

文件数据是 Vault 数据的重要组成部分，而 File Service 是 Vault Service 的基础。File Service 在 Hive Node 端是基于 IPFS 分布式文件存储服务，文件内容在 IPFS 服务上，其元数据保存与 Hive Node 的数据表中。

上传文件的示例如下：

```java
Vault vault = new Vault(context, getVaultProviderAddress());
FilesServic filesService = vault.getFilesService();
filesService.getUploadWriter(REMOTE_FILE_PATH)
        .thenCompose(this::writeFileContent)
        .thenAcceptAsync(result -> {
            System.out.println("Successfully get the result.");
        }).exceptionally(ex -> {
            ex.printStackTrace();
            return null;
        });
```

创建 Vault 对象依赖的参数，参见 [VaultSubscription](subscribe-to-vault-service.md) 的说明。文件上传的时候，首先通过指定路径（REMOTE_FILE_PATH）获取文件流的 Writer ，然后将文件内容写入 Writer 中 （this::writeFileContent），整个操作都在异步线程中进行的。

## Upload

如示例所示，上传文件提供的接口方式，是通过上传文件的 Hive Node 路径得到对应的写入流对象，SDK 中提供了两种方式的：Writer 和 OutputStream ，用户仅需往这两个对象中写入待上传的文件流即可。

```java
CompletableFuture<OutputStream> getUploadStream(String path);
CompletableFuture<Writer> getUploadWriter(String path);
```

## Download

下载文件也采用和上传文件类似的方法，通过指定远程文件的位置，获取 InputStream 和 Reader：

```java
CompletableFuture<InputStream> getDownloadStream(String path);
CompletableFuture<Reader> getDownloadReader(String path);
```

## List

列出文件目录下的文件，FileInfo 为单个文件信息，path 为文件目录。

```java
CompletableFuture<List<FileInfo>> list(String path);
```

## Stat

获取单个文件的信息。

```java
CompletableFuture<FileInfo> stat(String path);
```

## Hash

获取单个文件的 hash 值，hash 算法为 SHA256。

```java
CompletableFuture<String> hash(String path);
```

## Move

移动单个文件，从 source 位置移动到 target 位置。

```java
CompletableFuture<Void> move(String source, String target);
```

## Copy

拷贝单个文件，从 source 位置移动到 target 位置。

```java
CompletableFuture<Void> copy(String source, String target);
```

## Delete

删除文件，文件位置为 path 。

```java
CompletableFuture<Void> delete(String path);
```
