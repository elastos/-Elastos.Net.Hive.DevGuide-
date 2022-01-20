# FilesService

Hive SDK 通过 FileService 类将文件上传到对应的 Vault 中。文件数据是 Hive SDK 支持的数据类型之一。 FileService 类是 Vault Service 中衍生子服务之一，用于支持对文件类型的操作，比如上传，下载，删除等。文件数据一旦上传到 Hive Node 后，其文件块数据被托管存储到对应的IPFS Node 中，而其元数据信息则被托管到 Vault 内部数据库中。

Hive SDK uploads files to the corresponding Vault through the FileService class. File is one of the data types supported by Hive SDK. The FileService class is one of the derived sub-services in Vault Service, which is used to support the operation of file types, such as uploading, downloading, and deleting. Once the data is uploaded to Hive Node, its file block data is hosted and saved in the corresponding IPFS Node, while its metadata information is hosted in the Vault internal database.

## 上传文件

Upload file

### 使用写文件流接口 Writer 方式上传文件数据

Upload file data by writing file stream interface Writer.

上传文件时，需要先从 Vault 实例中获取 FileSerivce 接口实例，然后设置目标路径（REMOTE\_FILE\_PATH)来获取远程写文件流接口实例 Writer，同时将文件内容写入到该文件流的。写入文件数据的过程，也就是上传文件数据的过程。

When uploading a file, you need to get the FileSerivce interface instance from the Vault instance, and then set the target path (REMOTE\_FILE\_PATH) to get the remote write file stream interface instance Writer, and at the same time, write the file content into the file stream. The process of writing file data is the process of uploading file data.

使用FileService 实例上传文件整个过程通过 CompletableFuture 模式串联起来，最终返回一个 CompletableFuture 对象，表示文件上传成功还是上传失败跑出异常。

The whole process of uploading files of using FileService instance is connected in series through the CompletableFuture mode, and finally a CompletableFuture object is returned, which indicates whether the file upload succeeds, or the upload fails and an exception occurs.

通过使用写文件流接口方式上传文件的示例如下：

Example of uploading files by using the write file stream interface is as follows:

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

Vault vault = new Vault(context, getVaultProviderAddress());
FilesService filesService = vault.getFilesService();
filesService.getUploadWriter(YOUR_REMOTE_PATH)
    .thenCompose(this::writeFileContent)
    .thenAcceptAsync(result -> {
        System.out.println("Upload a file using writer.");
    }).exceptionally(ex -> {
        System.out.println("Failed to upload a file");
        ex.printStackTrace();
        return null;
    }
);
```

### 使用输出流 OutputStream 方式上传文件数据

Upload file data by the method of output stream OutputStream.

上传文件时，也可以通过 FileService 接口实例设置目录路径 (REMOTE\_FILE\_PATH)来获取远程输出流 OutputStream，同时将文件内容写入该输入流 OutputStream 实例中。

When uploading a file, you can also set the directory path (REMOTE\_FILE\_PATH) through the FileService interface instance to obtain the remote output stream OutputStream, and write the file content into the input stream OutputStream instance.

使用FileService 实例上传文件整个过程通过 CompletableFuture 模式串联起来，最终返回一个 CompletableFuture 对象，表示文件上传成功还是上传失败跑出异常。

The whole process of uploading files of using FileService instance is connected in series through the CompletableFuture mode, and finally a CompletableFuture object is returned, which indicates whether the file upload succeeds, or the upload fails and an exception occurs.

通过使用输出流接口方式上传文件的示例如下：

Example of uploading files by using the output stream interface is as follows:

```java
public CompletableFuture<Void> writeFileContent(OutputStream out) {
    return CompletableFuture.runAsync(() -> {
        try {
            out.write(REMOTE_FILE_CONTENT.getBytes());
            out.flush();
            out.close();
        } catch (IOException e) {
            throw new CompletionException(e);
        }
    });
}

Vault vault = new Vault(context, getVaultProviderAddress());
FilesService filesService = vault.getFilesService();
filesService.getUploadStream(YOUR_REMOTE_PATH)
.thenCompose(this::writeFileContent)
.thenAcceptAsync(result -> {
    System.out.println("Upload a file using OutputStream.");
}).exceptionally(ex -> {
    System.out.println("Failed to upload a file");
    ex.printStackTrace();
    return null;
});
```

## 下载文件

Download file

与上传文件类似，下载文件时通过 FileSerice 实例通过获取 Reader 或者 InputStream 实例，再基于Reader 实例或 InputStream 实例下载在 Vault 中的文件数据。

Similar to uploading files, when downloading files, you can get the Reader or InputStream instance through the FileSerice instance, and then download the file data in the Vault based on the Reader instance or InputStream instance.

```java
Vault vault = new Vault(context, getVaultProviderAddress());
FilesService filesService = vault.getFilesService();

filesService.getDownloadStream(YOUR_REMOTE_PATH)
.thenCompose(this::readFileContent)
.thenAcceptAsync(result -> {
    System.out.println("download the file by input stream successfully");
}).exceptionally(ex -> {
    System.out.println("failed to download the file by input stream");
    ex.printStackTrace();
    return null;
});

filesService.getDownloadReader(YOUR_REMOTE_PATH)
.thenCompose(this::readFileContent)
.thenAcceptAsync(result -> {
    System.out.println("download the file by input stream successfully");
}).exceptionally(ex -> {
    System.out.println("failed to download the file by input stream");
    ex.printStackTrace();
    return null;
});
```

## List files

列出文件目录下的文件，FileInfo 为单个文件信息，path 为文件目录。

List the files in the file directory. FileInfo is single file information, and path is the file directory.

```java
filesService.list(YOUR_REMOTE_PATH)
.thenAcceptAsync(list -> {
    System.out.println("list folder files successfully");
    for (FileInfo file in list) {
        System.out.println("file name => " + file.getName());
        System.out.println("file size => " + file.getSize());
    }
}).exceptionally(ex -> {
    System.out.println("failed to list folder files");
    ex.printStackTrace();
    return null;
});
```

## Stat

获取单个文件的信息。

Get the information of a single file.

```java
filesService.stat(YOUR_REMOTE_PATH)
.thenAcceptAsync(fileInfo -> {
    System.out.println("get the state of the file successfully");
    System.out.println("file name => " + fileInfo.getName());
    System.out.println("file size => " + fileInfo.getSize());
}).exceptionally(ex -> {
    System.out.println("failed to get the state of the file");
    ex.printStackTrace();
    return null;
});
```

## Move

移动单个文件，从 source 位置移动到 target 位置。

Move a single file from source to target.

```java
filesService.move(YOUR_REMOTE_PATH, YOUR_REMOTE_NEW_PATH)
.thenAcceptAsync(result -> {
    System.out.println("Moved the file to new path in success.");
}).exceptionally(ex -> {
    System.out.println("failed to move the file");
    ex.printStackTrace();
    return null;
});
```

## Delete

删除文件，文件位置为 path 。

Delete the file, the file is in path.

```java
filesService.delete(YOUR_REMOTE_PATH)
.thenAcceptAsync(result -> {
    System.out.println("delete the file successfully");
}).exceptionally(ex -> {
    System.out.println("failed to delete of the file");
    ex.printStackTrace();
    return null;
});
```
