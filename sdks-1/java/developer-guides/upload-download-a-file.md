# Upload-download-a-file

文件数据是 Vault 数据的重要组成部分，而 File Service 是 Vault Service 的基础。File Service 在 Hive Node 端是基于 IPFS 分布式文件存储服务，文件内容在 IPFS 服务上，其元数据保存与 Hive Node 的数据表中。

上传文件的示例如下（ context 和 vault provider 的设置参见[Developer Guide](./) ）：

```java
Vault vault = new Vault(context, getVaultProviderAddress());
FilesServic filesService = vault.getFilesService();
filesService.getUploadWriter(REMOTE_FILE_PATH)
.thenCompose(this::writeFileContent)
.thenAcceptAsync(result -> {
    System.out.println("upload the file by writer successfully");
}).exceptionally(ex -> {
    System.out.println("failed to upload the file by writer");
    ex.printStackTrace();
    return null;
});
```

文件上传的时候，首先通过指定路径（REMOTE\_FILE\_PATH）获取文件流的 Writer ，然后将文件内容写入 Writer 中 （this::writeFileContent），整个操作都在异步线程中进行的。

## Upload

如示例所示，上传文件提供的接口方式，是通过上传文件的 Hive Node 路径得到对应的写入流对象，SDK 中提供了两种方式的：Writer 和 OutputStream ，用户仅需往这两个对象中写入待上传的文件流即可。

```java
filesService.getUploadStream(REMOTE_FILE_PATH)
.thenCompose(this::writeFileContent)
.thenAcceptAsync(result -> {
    System.out.println("upload the file by output stream successfully");
}).exceptionally(ex -> {
    System.out.println("failed to upload the file by output stream");
    ex.printStackTrace();
    return null;
});
```

## Download

下载文件也采用和上传文件类似的方法，通过指定远程文件的位置，获取 InputStream 和 Reader：

```java
filesService.getDownloadStream(REMOTE_FILE_PATH)
.thenCompose(this::readFileContent)
.thenAcceptAsync(result -> {
    System.out.println("download the file by input stream successfully");
}).exceptionally(ex -> {
    System.out.println("failed to download the file by input stream");
    ex.printStackTrace();
    return null;
});
```

下面是使用 Reader 的示例，和 InputStream 的类似：

```java
filesService.getDownloadReader(REMOTE_FILE_PATH)
.thenCompose(this::readFileContent)
.thenAcceptAsync(result -> {
    System.out.println("download the file by input stream successfully");
}).exceptionally(ex -> {
    System.out.println("failed to download the file by input stream");
    ex.printStackTrace();
    return null;
});
```

## List

列出文件目录下的文件，FileInfo 为单个文件信息，path 为文件目录。

```java
filesService.list(REMOTE_DIR_PATH)
.thenAcceptAsync(list -> {
    System.out.println("list folder files successfully");
    System.out.println("List<FileInfo> =>");
}).exceptionally(ex -> {
    System.out.println("failed to list folder files");
    ex.printStackTrace();
    return null;
});
```

## Stat

获取单个文件的信息。

```java
filesService.stat(REMOTE_FILE_PATH)
.thenAcceptAsync(fileInfo -> {
    System.out.println("get the state of the file successfully");
    System.out.println("FileInfo =>");
}).exceptionally(ex -> {
    System.out.println("failed to get the state of the file");
    ex.printStackTrace();
    return null;
});
```

## Hash

获取单个文件的 hash 值，hash 算法为 SHA256。

```java
filesService.hash(REMOTE_FILE_PATH)
.thenAcceptAsync(hash -> {
    System.out.println("get the hash of the file successfully");
    System.out.println("hash string =>");
}).exceptionally(ex -> {
    System.out.println("failed to get the hash of the file");
    ex.printStackTrace();
    return null;
});
```

## Move

移动单个文件，从 source 位置移动到 target 位置。

```java
filesService.move(remoteTxtFilePath, remoteBackupTxtFilePath)
.thenAcceptAsync(result -> {
    System.out.println("move the file successfully");
    System.out.println(" => ");
}).exceptionally(ex -> {
    System.out.println("failed to move the file");
    ex.printStackTrace();
    return null;
});
```

## Copy

拷贝单个文件，从 source 位置移动到 target 位置。

```java
filesService.copy(remoteTxtFilePath, remoteBackupTxtFilePath)
.thenAcceptAsync(result -> {
    System.out.println("copy the file successfully");
    System.out.println(" => ");
}).exceptionally(ex -> {
    System.out.println("failed to copy the file");
    ex.printStackTrace();
    return null;
});
```

## Delete

删除文件，文件位置为 path 。

```java
filesService.delete(REMOTE_FILE_PATH)
.thenAcceptAsync(result -> {
    System.out.println("delete the file successfully");
    System.out.println(" =>");
}).exceptionally(ex -> {
    System.out.println("failed to delete of the file");
    ex.printStackTrace();
    return null;
});
```
