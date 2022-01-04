# DatabaseService

Hive Node 的 Database Service 也是 Hive Node 的基础服务，Database Service 基于 mongodb 的 NoSQL 数据库，Database Service 提供了基础的 CRUD 功能。

## Example

插入数据的示例如下（ context 和 vault provider 的设置参见[Developer Guide](README.md) ）：

```
let vault = try Vault(context, providerAddress)
let databaseServic = vault.databaseService

let docNode = ["author" : "john doe1", "title" : "Eve for Dummies1"]

_databaseService!.insertOne(COLLECTION_NAME, docNode, InsertOptions()
.bypassDocumentValidation(false))
.done{ insertResult in
    print("insert the document successfully.")
 }
.catch{ error in
    print(error)
}
```

插入数据需要先构造 Dictionary 数据，然后插入到指定的数据表（ COLLECTION_NAME ）中，同时可以指定插入选项 InsertOptions ，这些选项源自mongodb数据库。

## Create Collection

mongodb 中，collection 代表数据表，此处创建指定名称的数据表；创建之后，便可以插入用户数据。

```
databaseService!.createCollection("test_collection")
.done { success in
  print("create the collection successfully")
}
.catch { error in
   print(error)
}
```

## Delete Collection

删除 mongodb 数据表，表内的数据一并删除。

```
databaseService!.deleteCollection("test_collection")
.done { success in
   print("delete the collection successfully")
}
.catch { error in
   print(error)
}
```

## Insert

往数据表中插入数据，可以是单条数据，也可以是多条数据。如上面的示例所示，插入选项 InsertOptions 源自于 mongodb 插入 document 的选项，具体支持哪些选项，可以参考 InsertOptions 的定义。此处展示插入单条数据的例子。

```
 let nodes = [
                ["author" : "john doe2", "title" : "Eve for Dummies2"],
                ["author" : "john doe3", "title" : "Eve for Dummies3"],
            ]
databaseService!.insertMany("test_collection", nodes, InsertOptions().bypassDocumentValidation(false).ordered(true))
.done { insertResult in
   print("insert the documents successfully.")
}
.catch { error in
  print(error)
}
```

## Update

与插入数据一样，更新数据也提供了两个版本的接口：更新满足条件的第一条数据和多条数据，更新数据同样支持更新选项。更新单条数据如下：

```
let filter = ["author" : "john doe1"]
let doc = ["author" : "john doe1", "title" : "Eve for Dummies1_1"]
let update = ["$set" : doc]
databaseService!.updateOne("test_collection", filter, update, UpdateOptions()
.setBypassDocumentValidation(false).setUpsert(true))
.done { updateResult in
   print("update the document successfully.")
}
.catch { error in
   print(error)
}
```

更新多条数据的例子：

```
let filter = ["author" : "john doe1"]
let doc = ["author" : "john doe1", "title" : "Eve for Dummies1_2"]
let update = ["$set" : doc]

databaseService!.updateMany("test_collection", filter, update, UpdateOptions()
.setBypassDocumentValidation(true))
.done { updateResult in
   print("update the documents successfully.")
}
.catch { error in
   print(error)
}
```

## Delete

删除数据的方法也一样，提供了2个版本，仅需要提供表明和删除条件，满足条件的数据才会被删除。

```
let filter = ["author" : "john doe2"]
databaseService!.deleteOne("test_collection", filter)
.done { success in
  print("delete the document successfully.")
}
.catch { error in
  print(error)
}
```

删除满足条件的多条数据的示例如下：

```
let filter = ["author" : "john doe2"]
databaseService!.deleteMany("test_collection", filter)
.done { success in
   print("delete the documents successfully.")
}
.catch { error in
  print(error)
}
```

## Count

计算表中数据的条数可以使用以下的方法；需要指定表名、查询条件和参数。

```
let filter = ["author" : "john doe1"]
databaseService!.countDocuments("test_collection", filter, CountOptions().setLimit(1).setSkip(0).setMaxTimeMS(1000000000))
.done { count in
   print("count the documents successfully.")
}
.catch { error in
   print(error)
}
```

## Find and Query

查找数据支持以下三种方式：查询满足条件的第一条数据、查询满足条件的多条数据、以及带多个查询条件的多条数据返回的版本。三种方式都需要提供：表名、查询条件和查询选项。

```
let query = ["author" : "john doe1"]
databaseService!.findOne("test_collection", query, FindOptions().setSkip(0).setLimit(0))
.done { document in
   print("find the document successfully.")
}
.catch { error in
   print(error)
}
```

查找多个结果的示例如下：

```
let query = ["author" : "john doe1"]
databaseService!.findMany("test_collection", query, FindOptions().setSkip(0).setLimit(0))
.done { list in
   print("find the documents successfully.")
}.catch { error in
   print(error)
}
```

查找多个结果时可以使用更多的查询选项的示例如下：

```
let query = ["author" : "john doe1"]
databaseService!.query("test_collection", query, nil)
.done { list in
   print("find the documents successfully.")
}
.catch { error in
   print(error)
}
```
