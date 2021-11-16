# Update database data

数据库服务除了查询功能之外，还有更新数据的功能，它们都需要通过Vault类获取DatabaseService，具体参见[Query Database Data](update-database-data.md)。获取到DatabaseService之后，就可以执行更新数据库数据的操作。

## Insert

插入数据功能，就是插入自定义的类Json格式数据，具体示例如下。插入数据需要指定：数据表名称、数据列表和插入数据选项。

```java
@Test @Order(3) void testInsertMany() {
    Assertions.assertDoesNotThrow(()->{
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

        Assertions.assertNotNull(databaseService.insertMany(COLLECTION_NAME, nodes,
                new InsertOptions().bypassDocumentValidation(false).ordered(true)).get());
    });
}
```

## Update

更新数据功能，就是按照查询条件，更新符合条件的已有数据。相对于插入数据而言，更新数据的参数多了一个查询条件。

```java
@Test @Order(9) void testUpdateMany() {
    Assertions.assertDoesNotThrow(()->{
        ObjectNode filter = JsonNodeFactory.instance.objectNode();
        filter.put("author", "john doe1");
        ObjectNode doc = JsonNodeFactory.instance.objectNode();
        doc.put("author", "john doe1");
        doc.put("title", "Eve for Dummies1_2");
        ObjectNode update = JsonNodeFactory.instance.objectNode();
        update.put("$set", doc);
        Assertions.assertNotNull(databaseService.updateMany(COLLECTION_NAME, filter, update,
                new UpdateOptions().setBypassDocumentValidation(false).setUpsert(true)).get());
    });
}
```

## Delete

删除数据功能只需要指定数据表的名称和查询条件，满足条件的数据都会被删除。

```java
@Test @Order(11) void testDeleteMany() {
    Assertions.assertDoesNotThrow(()->{
        ObjectNode filter = JsonNodeFactory.instance.objectNode();
        filter.put("author", "john doe2");
        databaseService.deleteMany(COLLECTION_NAME, filter).get();
    });
}
```
