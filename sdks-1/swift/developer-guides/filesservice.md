# FilesService

Hive SDK 通过 FileService 类将文件上传到对应的 Vault 中。文件数据是 Hive SDK 支持的数据类型之一。 FileService 类是 Vault Service 中衍生子服务之一，用于支持对文件类型的操作，比如上传，下载，删除等。文件数据一旦上传到 Hive Node 后，其文件块数据被托管存储到对应的IPFS Node 中，而其元数据信息则被托管到 Vault 内部数据库中。

## 上传文件

### 使用写文件流接口 Writer 方式上传文件数据

上传文件时，需要先从 Vault 实例中获取 FileSerivce 接口实例，然后设置目标路径（REMOTE\_FILE\_PATH)来获取远程写文件流接口实例 Writer，同时将文件内容写入到该文件流的。写入文件数据的过程，也就是上传文件数据的过程。

使用FileService 实例上传文件整个过程通过 Promise 模式串联起来，最终返回一个 Promise 对象，表示文件上传成功还是上传失败跑出异常。

通过使用写文件流接口方式上传文件的示例如下：

```
let vault = try Vault(appContext, providerAddress)
let filesService = try vault.filesService
filesService.getUploadWriter(YOUR_REMOTE_PATH)
.then { fileWriter in
   return try fileWriter.write(data: REMOTE_FILE_CONTENT_DATA)
}
.done { success in
   print("Upload the file by writer successfully")
}
.catch { error in
	print("Failed to upload a file.")
   print(error)
}
```

## 下载文件

与上传文件类似，下载文件时通过 FileSerice 实例通过获取 Reader 或者 InputStream 实例，再基于Reader 实例或 InputStream 实例下载在 Vault 中的文件数据。

```
filesService.getDownloadReader(YOUR_REMOTE_PATH)
.then { fileReader in
   return fileReader.read(localCachePath)
}
.done { result in
   print("Download the file by input stream successfully")
}
.catch { error in
	print("Failed to download the file by input stream")
   print(error)
}
```

## List

列出文件目录下的文件，FileInfo 为单个文件信息，path 为文件目录。

```
filesService!.list(YOUR_REMOTE_PATH)
.done { list in
   print("List folder files successfully.")
   list.forEach { info in
     print("file name => \(info.updated)")
     print("file size => \(info.size)")
     }
}
.catch { error in
	print("Failed to list folder files")
   print(error)
}
```

## Stat

获取单个文件的信息。

```
filesService!.stat(YOUR_REMOTE_PATH)
.done { fileInfo in
   print("get the state of the file successfully.")
   print("file name => \(fileInfo.name)")
   print("file size => \(fileInfo.size)")
}
.catch { error in
   print(error)
}
```

## Move

移动单个文件，从 source 位置移动到 target 位置。

```
filesService!.move(YOUR_REMOTE_PATH, YOUR_REMOTE_NEW_PATH)
.done { success in
   print("Moved the file to new path in success.")
}
.catch { error in
   print("Failed to move the file")
   print(error)
}
```

## Delete

删除文件，文件位置为 path 。

```
filesService!.delete(YOUR_REMOTE_PATH)
.done { success in
   print("Delete the file successfully")
}
.catch { error in
   print("Failed to delete of the file")
   print(error)
}
```
