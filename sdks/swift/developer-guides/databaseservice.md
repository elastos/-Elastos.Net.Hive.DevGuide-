# DatabaseService

Hive SDK uploads unstructured data to the corresponding Vault through DatabaseService class. Unstructured database type data is one of the data types supported by Hive SDK. DatabaseService class is one of the derived sub-services in Vault Service, which is used to support operations on unstructured data types, such as insert/update/query of CRUD functions. Once the unstructured data is saved in Hive Node, it will be persisted in the corresponding MongoDB service.

## Insert a Document

An example of inserted data is as follows:

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

To insert data, you need to construct Json data first, and then insert it into the specified data table (COLLECTION\_NAME). At the same time, you can specify the InsertOptions, which are derived from the corresponding functions of MongoDB database.

## Create Collection

In MongoDB, collection represents a data table, where a data table with a specified name is created. After creation, user data can be inserted.

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

Delete MongoDB data table and all data in the table will be removed.

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

Insert data into the data table, which can be either single or multiple pieces of data. As shown in the example above, the insert option InsertOptions originates from MongoDB’s option to insert document. Please refer to the definition of InsertOptions for specific options. Here is an example of inserting multiple pieces of data:

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

Just like inserting data, updating data provides two versions of interfaces: updating the first piece of data or multiple pieces of data that meet the conditions, and updating data also supports providing update options. Update a single piece of data as follows:

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

Here's an example of updating multiple pieces of data:

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

The method of deleting data is the same - two versions are provided. Only by providing indication and deletion conditions, the data that meets the conditions will be deleted. The following is an example of deleting the first piece of data that meets the conditions:

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

Below is an example of deleting multiple pieces of data that meet the conditions:

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

The following methods can be used to calculate the number of data in the table: table name, query criteria, and parameters are required.

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

There are three ways to search data: search the first piece of data that meets the conditions, search multiple pieces of data that meet the conditions, and return version of multiple pieces of data with multiple query conditions. All three methods are required: table name, query conditions, and query options.

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

An example of multiple search results is as follows:

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

An example of more query options that can be used when searching multiple results is as follows:

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
