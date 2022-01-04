# DatabaseService

Hive SDK 通过 DatabaseService 类将非结构化数据上传到对应的 Vault 中。非结构化数据库类型数据是 Hive SDK 支持的数据类型之一。 DatabaseService 类是 Vault Service 中衍生子服务之一，用于支持对非结构化数据类型的操作，比如插入/更新/查询等CRUD功能。非结构化数据一旦存储到Hive Node，就会被持久化道对应的 MongoDB service中。

## Insert a document

插入数据的示例如下：

```java
Vault vault = new Vault(context, getVaultProviderAddress());
DatabaseServic dbService = vault.getDatabaseServic();

ObjectNode doc = JsonNodeFactory.instance.objectNode();
doc.put("author", "john doe1");
doc.put("title", "Eve for Dummies1");
dbService.insertOne(YOUR_COLLECT_NAME, doc,
        new InsertOptions().bypassDocumentValidation(false))
.thenAcceptAsync(insertResult -> {
    System.out.println("Inserted a document to database in success.");
}).exceptionally(ex -> {
    System.out.println("Failed to insert a document.");
    ex.printStackTrace();
    return null;
});
```

插入数据需要先构造 Json 数据，然后插入到指定的数据表（ COLLECTION_NAME ）中，同时可以指定插入选项 InsertOptions ，InsertOptions 选项源自mongodb数据库相对应的功能。

## Create Collection

mongodb 中，collection 代表数据表，此处创建指定名称的数据表；创建之后，便可以插入用户数据。

```java
databaseService.createCollection("your_collection")
.thenAcceptAsync(result -> {
    System.out.println("Created a collection successfully");
}).exceptionally(ex -> {
    System.out.println("Failed to create the collection");
    ex.printStackTrace();
    return null;
});
```

## Delete Collection

删除 mongodb 数据表，表内的数据会一并删除。

```java
databaseService.deleteCollection("your_collection")
.thenAcceptAsync(result -> {
    System.out.println("Deleted the collection successfully");
}).exceptionally(ex -> {
    System.out.println("Failed to delete the collection");
    ex.printStackTrace();
    return null;
});
```

## Insert

往数据表中插入数据，可以是单条数据，也可以是多条数据。如上面的示例所示，插入选项 InsertOptions 源自于 mongodb 插入 document 的选项，具体支持哪些选项，可以参考 InsertOptions 的定义。此处展示插入多条数据的例子。

```java
List<JsonNode> nodes = new ArrayList<>();

ObjectNode doc = JsonNodeFactory.instance.objectNode();
doc.put("author", "john doe2");
doc.put("title", "Eve for Dummies2");
nodes.add(doc);

doc.removeAll();

doc = JsonNodeFactory.instance.objectNode();
doc.put("author", "john doe3");
doc.put("title", "Eve for Dummies3");
nodes.add(doc);
dbService.insertMany("your_collection", nodes,
		new InsertOptions().bypassDocumentValidation(false).ordered(true))
.thenAcceptAsync(insertResult -> {
    System.out.println("Inserted two documents successfully.");
}).exceptionally(ex -> {
    System.out.println("Failed to insert documents.");
    ex.printStackTrace();
    return null;
});
```

## Update

与插入数据一样，更新数据也提供了两个版本的接口：更新满足条件的第一条数据或多条数据，更新数据同样支持提供更新选项。更新单条数据如下：

```java
ObjectNode filter = JsonNodeFactory.instance.objectNode();
filter.put("author", "john doe1");
ObjectNode doc = JsonNodeFactory.instance.objectNode();
doc.put("author", "john doe1");
doc.put("title", "Eve for Dummies1_1");
ObjectNode update = JsonNodeFactory.instance.objectNode();
update.put("$set", doc);
dbService.updateOne("your_collection", filter, update,
        new UpdateOptions().setBypassDocumentValidation(false).setUpsert(true))
.thenAcceptAsync(updateResult -> {
    System.out.println("updated the document successfully.");
    System.out.println("UpdateResult =>");
}).exceptionally(ex -> {
    System.out.println("Failed to update the document.");
    ex.printStackTrace();
    return null;
});
```

更新多条数据的例子：

```java
ObjectNode filter = JsonNodeFactory.instance.objectNode();
filter.put("author", "john doe1");
ObjectNode doc = JsonNodeFactory.instance.objectNode();
doc.put("author", "john doe1");
doc.put("title", "Eve for Dummies1_1");
ObjectNode update = JsonNodeFactory.instance.objectNode();
update.put("$set", doc);
databaseService.updateMany("your_collection", filter, update,
		new UpdateOptions().setBypassDocumentValidation(false).setUpsert(true))
.thenAcceptAsync(updateResult -> {
    System.out.println("Updated the documents successfully.");
}).exceptionally(ex -> {
    System.out.println("Failed to update the documents.");
    ex.printStackTrace();
    return null;
});
```

## Delete

删除数据的方法也一样，提供了两个版本，仅需要提供表明和删除条件，满足条件的数据才会被删除。以下是删除满足条件的第一条数据的例子：

```java
ObjectNode filter = JsonNodeFactory.instance.objectNode();
filter.put("author", "john doe2");
databaseService.deleteOne("your_collection", filter)
.thenAcceptAsync(deleteResult -> {
    System.out.println("delete the document successfully.");
    System.out.println("DeleteResult =>");
}).exceptionally(ex -> {
    System.out.println("failed to delete the document.");
    ex.printStackTrace();
    return null;
});
```

删除满足条件的多条数据的示例如下：

```java
ObjectNode filter = JsonNodeFactory.instance.objectNode();
filter.put("author", "john doe2");
databaseService.deleteMany("your_collection", filter)
.thenAcceptAsync(deleteResult -> {
    System.out.println("delete the documents successfully.");
    System.out.println("DeleteResult =>");
}).exceptionally(ex -> {
    System.out.println("failed to delete the documents.");
    ex.printStackTrace();
    return null;
});
```

## Count

计算表中数据的条数可以使用以下的方法，需要指定表名、查询条件和参数。

```java
ObjectNode filter = JsonNodeFactory.instance.objectNode();
filter.put("author", "john doe1");
databaseService.countDocuments("your_collection", filter,
        new CountOptions().setLimit(1L).setSkip(0L).setMaxTimeMS(1000000000L))
.thenAcceptAsync(count -> {
    System.out.println("Count documents successfully.");
    System.out.println("count =>");
}).exceptionally(ex -> {
    System.out.println("Failed to count the documents.");
    ex.printStackTrace();
    return null;
});
```

## Find and Query

查找数据支持以下三种方式：查询满足条件的第一条数据、查询满足条件的多条数据、以及带多个查询条件的多条数据返回的版本。三种方式都需要提供：表名、查询条件和查询选项。

```java
ObjectNode query = JsonNodeFactory.instance.objectNode();
query.put("author", "john doe1");
databaseService.findOne("test_collection", query, new FindOptions().setSkip(0).setLimit(0))
.thenAcceptAsync(document -> {
    System.out.println("find the document successfully.");
    System.out.println("author => " + document.get("author").textValue());
    System.out.println("title => " + document.get("title").textValue());
}).exceptionally(ex -> {
    System.out.println("failed to find the document.");
    ex.printStackTrace();
    return null;
});
```

查找多个结果的示例如下：

```java
ObjectNode query = JsonNodeFactory.instance.objectNode();
query.put("author", "john doe1");
databaseService.findMany("test_collection", query, new FindOptions().setSkip(0).setLimit(0))
.thenAcceptAsync(list -> {
    System.out.println("find the documents successfully.");
    System.out.println("author => " + document.get("author").textValue());
    System.out.println("title => " + document.get("title").textValue());
}).exceptionally(ex -> {
    System.out.println("failed to find the documents.");
    ex.printStackTrace();
    return null;
});
```

查找多个结果时可以使用更多的查询选项的示例如下：

```java
ObjectNode query = JsonNodeFactory.instance.objectNode();
query.put("author", "john doe1");
Assertions.assertNotNull(databaseService.query(COLLECTION_NAME, query, null)
.thenAcceptAsync(list -> {
    System.out.println("find the documents successfully.");
    System.out.println("author => " + document.get("author").textValue());
    System.out.println("title => " + document.get("title").textValue());
}).exceptionally(ex -> {
    System.out.println("failed to find the documents.");
    ex.printStackTrace();
    return null;
});
```
