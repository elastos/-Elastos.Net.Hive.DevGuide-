# FilesService

文件数据是 Vault 数据的重要组成部分，而 File Service 是 Vault Service 的基础。File Service 在 Hive Node 端是基于 IPFS 分布式文件存储服务，文件内容在 IPFS 服务上，其元数据保存与 Hive Node 的数据表中。

上传文件的示例如下（ context 和 vault provider 的设置参见[Developer Guide](README.md) ）：

```
let vault = try Vault(appContext, providerAddress)
let filesService = try vault.filesService
filesService!.getUploadWriter(filePath)
.then { fileWriter in
   return try fileWriter.write(data: data)
}
.done { success in
   print("upload the file by writer successfully")
}
.catch { error in
   print(error)
}
```

文件上传的时候，首先通过指定路径（REMOTE_FILE_PATH）获取文件流的 Writer ，然后将文件内容写入 Writer 中 （this::writeFileContent），整个操作都在异步线程中进行的。

## Upload

如示例所示，上传文件提供的接口方式，是通过上传文件的 Hive Node 路径得到对应的写入流对象，SDK 中提供了FileWriter的方式 ，用户仅需往这两个对象中写入待上传的文件路径，拿到FileWriter，用FileWriter写入数据即可。

```
filesService!.getUploadWriter(filePath)
.then { fileWriter in
   return try fileWriter.write(data: data)
}
.done { success in
   print("upload the file by writer successfully")
}
.catch { error in
   print(error)
}
```

## Download

下载文件也采用和上传文件类似的方法，通过指定远程文件的位置，获取 FileReader：

```
filesService.getDownloadReader(REMOTE_FILE_PATH)
.then { fileReader in
   return fileReader.read(localCachePath)
}
.done { result in
   print("download the file by input stream successfully")
}
.catch { error in
   print(error)
}
```

## List

列出文件目录下的文件，FileInfo 为单个文件信息，path 为文件目录。

```
filesService!.list(REMOTE_DIR_PATH)
.done { list in
   print("list folder files successfully.")
}
.catch { error in
   print(error)
}
```

## Stat

获取单个文件的信息。

```
filesService!.stat(REMOTE_FILE_PATH)
.done { fileInfo in
   print("get the state of the file successfully.")
}
.catch { error in
   print(error)
}
```

## Hash

获取单个文件的 hash 值，hash 算法为 SHA256。

```
filesService!.hash(REMOTE_FILE_PATH)
.done { hash in
   print("get the hash of the file successfully")
}
.catch { error in
   print(error)
}
```

## Move

移动单个文件，从 source 位置移动到 target 位置。

```
filesService!.move(remoteTxtFilePath, remoteBackupTxtFilePath)
.done { success in
   print("move the file successfully")
}
.catch { error in
   print(error)
}
```

## Copy

拷贝单个文件，从 source 位置移动到 target 位置。

```
filesService!.copy(remoteTxtFilePath, remoteBackupTxtFilePath)
.done { success in
   print("copy the file successfully")
}
.catch { error in
   print(error)
}
```

## Delete

删除文件，文件位置为 path 。

```
filesService!.delete(REMOTE_FILE_PATH)
.done { success in
   print("delete the file successfully")
}
.catch { error in
   print(error)
}
```
