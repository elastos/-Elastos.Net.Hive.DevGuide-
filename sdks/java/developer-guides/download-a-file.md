# Download a file

下载文件也是文件服务的功能之一，如何获取文件服务，请参考[Upload File](upload-file.md)节。

与上传文件类似，下载文件也提供了2种方式：Reader和InputStream。下面就是代码示例：

```java
@Test @Order(3) void testDownloadText() {
    Assertions.assertDoesNotThrow(this::downloadTextReally);
}

private void downloadTextReally() throws ExecutionException, InterruptedException, IOException {
    try (Reader reader = filesService.getDownloadReader(remoteTxtFilePath).get()) {
        Assertions.assertNotNull(reader);
        Utils.cacheTextFile(reader, localCacheRootDir, FILE_NAME_TXT);
        Assertions.assertTrue(isFileContentEqual(localTxtFilePath, localCacheRootDir + FILE_NAME_TXT));
    }
}
```

```java
@Test @Order(4) void testDownloadBin() {
    Assertions.assertDoesNotThrow(this::downloadBinReally);
}

private void downloadBinReally() throws ExecutionException, InterruptedException, IOException {
    try (InputStream in = filesService.getDownloadStream(remoteImgFilePath).get()) {
        Assertions.assertNotNull(in);
        Utils.cacheBinFile(in, localCacheRootDir, FILE_NAME_IMG);
        Assertions.assertTrue(isFileContentEqual(localImgFilePath, localCacheRootDir + FILE_NAME_IMG));
    }
}
```
