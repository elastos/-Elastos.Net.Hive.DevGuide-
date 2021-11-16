# Upload file

上传文件功能源自Vault的文件服务，文件服务是Vault的基础服务。它能为应用上传下载文件、获取文件信息等。文件服务基于IPFS分布式文件存储服务，文件的内容位于IPFS端，文件的元数据位于Vault的数据表内。本节主要介绍如何上传文件到用户的Vault。

Vault相关的服务，都通过Vault类来进行。首先，需要创建Vault对象，需要指定AppContext和Hive Node的服务地址。

```java
public Vault newVault() {
    return new Vault(context, getVaultProviderAddress());
}
```

然后获取文件服务。

```java
@BeforeAll public static void setUp() {
    // ...
    Assertions.assertDoesNotThrow(()->filesService = TestData.getInstance().newVault().getFilesService());
}
```

最后通过文件服务上传文件，上传文件接口提供了不同的方式：Writer和OutputStream。获取到其中之一，直接往里写入文件内容即可。下面的测试用例中，上传完之后，会检查上传的文件是否存在，确保上传成功。

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

```java
@Test @Order(2) void testUploadBin() {
    Assertions.assertDoesNotThrow(this::uploadBinReally);
    verifyRemoteFileExists(remoteImgFilePath);
}

private void uploadBinReally() throws ExecutionException, InterruptedException, IOException {
    try (OutputStream out = filesService.getUploadStream(remoteImgFilePath).get()) {
        Assertions.assertNotNull(out);
        out.write(Utils.readImage(localImgFilePath));
        out.flush();
    }
}
```
