# DatabaseService

Hive Node 的 Database Service 也是 Hive Node 的基础服务，Database Service 基于 mongodb 的 NoSQL 数据库，Database Service 提供了基础的 CRUD 功能。

插入数据的示例如下（ context 和 vault provider 的设置参见[Developer Guide](README.md) ）：

```java
Vault vault = new Vault(context, getVaultProviderAddress());
DatabaseServic databaseServic = vault.getDatabaseServic();

ObjectNode docNode = JsonNodeFactory.instance.objectNode();
docNode.put("author", "john doe1");
docNode.put("title", "Eve for Dummies1");
databaseService.insertOne(COLLECTION_NAME, docNode,
        new InsertOptions().bypassDocumentValidation(false))
    .thenAcceptAsync(result -> {
        System.out.println("Successfully get the result.");
    }).exceptionally(ex -> {
        ex.printStackTrace();
        return null;
    });
```

插入数据需要先构造 Json 数据，然后插入到指定的数据表（ COLLECTION_NAME ）中，同时可以指定插入选项 InsertOptions ，这些选项源自mongodb数据库。

## Create Collection

mongodb 中，collection 代表数据表，此处创建指定名称的数据表；创建之后，便可以插入用户数据。

```java
CompletableFuture<Void> createCollection(String name);
```

## Delete Collection

删除 mongodb 数据表，表内的数据一并删除。

```java
CompletableFuture<Void> deleteCollection(String name);
```

## Insert

往数据表中插入数据，可以是单条数据，也可以是多条数据。如上面的示例所示，插入选项 InsertOptions 源自于 mongodb 插入 document 的选项，具体支持哪些选项，可以参考 InsertOptions 的定义。

```java
CompletableFuture<InsertResult> insertOne(String collection, JsonNode doc, InsertOptions options);
CompletableFuture<InsertResult> insertMany(String collection, List<JsonNode> docs, InsertOptions options);
```

## Update

与插入数据一样，更新数据也提供了两个版本的接口：更新满足条件的第一条数据和多条数据，更新数据同样支持更新选项。

```java
CompletableFuture<UpdateResult> updateOne(String collection, JsonNode filter, JsonNode update, UpdateOptions options);
CompletableFuture<UpdateResult> updateMany(String collection, JsonNode filter, JsonNode update, UpdateOptions options);
```

## Delete

删除数据的方法也一样，提供了2个版本，仅需要提供表明和删除条件，满足条件的数据才会被删除。

```java
CompletableFuture<Void> deleteOne(String collection, JsonNode filter);
CompletableFuture<Void> deleteMany(String collection, JsonNode filter);
```

## Count

计算表中数据的条数可以使用以下的方法；需要指定表名、查询条件和参数。

```java
CompletableFuture<Long> countDocuments(String collection, JsonNode query, CountOptions options);
```

## Find and Query

查找数据支持以下三种方式：查询满足条件的第一条数据、查询满足条件的多条数据、以及带多个查询条件的多条数据返回的版本。三种方式都需要提供：表名、查询条件和查询选项。

```java
CompletableFuture<JsonNode> findOne(String collection, JsonNode query, FindOptions options);
CompletableFuture<List<JsonNode>> findMany(String collection, JsonNode query, FindOptions options);
CompletableFuture<List<JsonNode>> query(String collection, JsonNode query, QueryOptions options);
```
