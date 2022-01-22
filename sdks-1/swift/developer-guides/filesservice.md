# FilesService

Hive SDK 通过 FileService 类将文件上传到对应的 Vault 中。文件数据是 Hive SDK 支持的数据类型之一。 FileService 类是 Vault Service 中衍生子服务之一，用于支持对文件类型的操作，比如上传，下载，删除等。文件数据一旦上传到 Hive Node 后，其文件块数据被托管存储到对应的IPFS Node 中，而其元数据信息则被托管到 Vault 内部数据库中。

Hive SDK uploads files to the corresponding Vault through the FileService class. File is one of the data types supported by Hive SDK. The FileService class is one of the derived sub-services in Vault Service, which is used to support the operation of file types, such as uploading, downloading, and deleting. Once the data is uploaded to Hive Node, its file block data is hosted and saved in the corresponding IPFS Node, while its metadata information is hosted in the Vault internal database.

## 上传文件

Upload file

### 使用写文件流接口 Writer 方式上传文件数据

Upload file data by writing file stream interface Writer.

上传文件时，需要先从 Vault 实例中获取 FileSerivce 接口实例，然后设置目标路径（REMOTE\_FILE\_PATH)来获取远程写文件流接口实例 Writer，同时将文件内容写入到该文件流的。写入文件数据的过程，也就是上传文件数据的过程。

When uploading a file, you need to first get the FileSerivce interface instance from the Vault instance, and then set the target path (REMOTE\_FILE\_PATH) to get the remote write file stream interface instance Writer, and at the same time, write the file content into the file stream. The process of writing file data is the process of uploading file data.

使用FileService 实例上传文件整个过程通过 Promise 模式串联起来，最终返回一个 Promise 对象，表示文件上传成功还是上传失败跑出异常。

The whole process of uploading files by using the FileService instance is connected in series through the Promise mode, and finally a Promise object is returned, which indicates whether the file upload succeeds, or the upload fails and an exception occurs.

通过使用写文件流接口方式上传文件的示例如下：

Example of uploading files by using the write file stream interface is as follows:

```swift
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

Download file

与上传文件类似，下载文件时通过 FileSerice 实例通过获取 Reader 或者 InputStream 实例，再基于Reader 实例或 InputStream 实例下载在 Vault 中的文件数据。

Similar to uploading files, when downloading files, you can get the Reader or InputStream instance through the FileSerice instance, and then download the file data in the Vault based on the Reader instance or InputStream instance.

```swift
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

List the files in the file directory. FileInfo is single file information, and path is the file directory.

```swift
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

Get the information of a single file.

```swift
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

Move a single file from source to target.

```swift
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

Delete the file; the file is in path.

```swift
filesService!.delete(YOUR_REMOTE_PATH)
.done { success in
   print("Delete the file successfully")
}
.catch { error in
   print("Failed to delete of the file")
   print(error)
}
```
