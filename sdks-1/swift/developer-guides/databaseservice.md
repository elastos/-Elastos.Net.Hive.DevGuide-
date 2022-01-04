# DatabaseService

Hive SDK 通过 DatabaseService 类将非结构化数据上传到对应的 Vault 中。非结构化数据库类型数据是 Hive SDK 支持的数据类型之一。 DatabaseService 类是 Vault Service 中衍生子服务之一，用于支持对非结构化数据类型的操作，比如插入/更新/查询等CRUD功能。非结构化数据一旦存储到Hive Node，就会被持久化道对应的 MongoDB service中。

## Insert a document

插入数据的示例如下：

```swift
let vault = try Vault(context, providerAddress)
let databaseServic = vault.databaseService

let docNode = ["author" : "john doe1", "title" : "Eve for Dummies1"]

_databaseService.insertOne(YOUR_COLLECT_NAME, docNode, InsertOptions()
.bypassDocumentValidation(false))
.done{ insertResult in
    print("Inserted a document to database in success.")
 }
.catch{ error in
	print("Failed to insert a document.")
    print(error)
}
```

插入数据需要先构造 Json 数据，然后插入到指定的数据表（ COLLECTION\_NAME ）中，同时可以指定插入选项 InsertOptions ，InsertOptions 选项源自mongodb数据库相对应的功能。

## Create Collection

mongodb 中，collection 代表数据表，此处创建指定名称的数据表；创建之后，便可以插入用户数据。

```swift
databaseService!.createCollection("your_collection")
.done { success in
  print("Create the collection successfully")
}
.catch { error in
	print("Failed to create the collection")
   print(error)
}
```

## Delete Collection

删除 mongodb 数据表，表内的数据一并删除。

```swift
databaseService!.deleteCollection("your_collection")
.done { success in
   print("Delete the collection successfully")
}
.catch { error in
	print("Failed to delete the collection.")
   print(error)
}
```

## Insert

往数据表中插入数据，可以是单条数据，也可以是多条数据。如上面的示例所示，插入选项 InsertOptions 源自于 mongodb 插入 document 的选项，具体支持哪些选项，可以参考 InsertOptions 的定义。此处展示插入多条数据的例子。

```swift
 let nodes = [
                ["author" : "john doe2", "title" : "Eve for Dummies2"],
                ["author" : "john doe3", "title" : "Eve for Dummies3"],
            ]
databaseService.insertMany("your_collection", nodes, InsertOptions().bypassDocumentValidation(false).ordered(true))
.done { insertResult in
   print("Inserted two documents successfully.")
}
.catch { error in
  print("Failed to insert documents.")
  print(error)
}
```

## Update

与插入数据一样，更新数据也提供了两个版本的接口：更新满足条件的第一条数据或多条数据，更新数据同样支持提供更新选项。更新单条数据如下：

```swift
let filter = ["author" : "john doe1"]
let doc = ["author" : "john doe1", "title" : "Eve for Dummies1_1"]
let update = ["$set" : doc]
databaseService!.updateOne("your_collection", filter, update, UpdateOptions()
.setBypassDocumentValidation(false).setUpsert(true))
.done { updateResult in
   print("Update the document successfully.")
}
.catch { error in
	print("Failed to update the document.")
   print(error)
}
```

更新多条数据的例子：

```swift
let filter = ["author" : "john doe1"]
let doc = ["author" : "john doe1", "title" : "Eve for Dummies1_2"]
let update = ["$set" : doc]

databaseService!.updateMany("your_collection", filter, update, UpdateOptions()
.setBypassDocumentValidation(true))
.done { updateResult in
   print("Update the documents successfully.")
}
.catch { error in
   print("Failed to update the documents.")
   print(error)
}
```

## Delete

删除数据的方法也一样，提供了两个版本，仅需要提供表明和删除条件，满足条件的数据才会被删除。以下是删除满足条件的第一条数据的例子：

```swift
let filter = ["author" : "john doe2"]
databaseService!.deleteOne("your_collection", filter)
.done { success in
  print("Delete the document successfully.")
}
.catch { error in
  print("Failed to delete the document.")
  print(error)
}
```

删除满足条件的多条数据的示例如下：

```swift
let filter = ["author" : "john doe2"]
databaseService!.deleteMany("your_collection", filter)
.done { success in
   print("Delete the documents successfully.")
}
.catch { error in
  print("Failed to delete the documents.")
  print(error)
}
```

## Count

计算表中数据的条数可以使用以下的方法；需要指定表名、查询条件和参数。

```swift
let filter = ["author" : "john doe1"]
databaseService!.countDocuments("your_collection", filter, CountOptions().setLimit(1).setSkip(0).setMaxTimeMS(1000000000))
.done { count in
   print("Count the documents successfully.")
   print("count => \(count)")
}
.catch { error in
	print("Failed to count the documents.")
   print(error)
}
```

## Find and Query

查找数据支持以下三种方式：查询满足条件的第一条数据、查询满足条件的多条数据、以及带多个查询条件的多条数据返回的版本。三种方式都需要提供：表名、查询条件和查询选项。

```swift
let query = ["author" : "john doe1"]
databaseService!.findOne("your_collection", query, FindOptions().setSkip(0).setLimit(0))
.done { document in
   print("Find the document successfully.")
   print("author => \(document["author"])")
   print("title => \(document["title"])")
}
.catch { error in
   print("Failed to find the document.")
   print(error)
}
```

查找多个结果的示例如下：

```swift
let query = ["author" : "john doe1"]
databaseService!.findMany("your_collection", query, FindOptions().setSkip(0).setLimit(0))
.done { list in
   print("Find the documents successfully.")
   print("author => \(document[author])")
   print("title => \(document["title"])")
}.catch { error in
   print("Failed to find the documents.")
   print(error)
}
```

查找多个结果时可以使用更多的查询选项的示例如下：

```swift
let query = ["author" : "john doe1"]
databaseService!.query("your_collection", query, nil)
.done { list in
   print("Find the documents successfully.")
   print("author => \(document["author"])")
   print("title => \(document["title"])")
}
.catch { error in
   print("Failed to find the documents.")
   print(error)
}
```
