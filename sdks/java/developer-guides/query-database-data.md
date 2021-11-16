# Query database data

查询数据库数据功能位于Vault的数据库服务，数据库服务基于mongodb，是NoSQL的数据存储方式，SDK封装了数据操作的接口，通过DatabaseService来提供功能。若要获取到DatabaseService服务，仍然是先要创建Vault对象。

```java
public Vault newVault() {
    return new Vault(context, getVaultProviderAddress());
}

@BeforeAll public static void setUp() {
    // ...
    Assertions.assertDoesNotThrow(()->databaseService = TestData.getInstance().newVault().getDatabaseService());
}
```

获取到了DatabaseService服务，便可以通过查询数据的接口来查询。查询接口分为连个，一个是简单查询，另一个是复杂查询，不同点在于查询的参数会有差异。

简单查询的示例如下。需要指定数据表的名称、查询条件、以及简单的查询选项（指定查询结果的范围）。

```java
@Test @Order(5) void testFindMany() {
    Assertions.assertDoesNotThrow(()->{
        ObjectNode query = JsonNodeFactory.instance.objectNode();
        query.put("author", "john doe1");
        List<JsonNode> docs = databaseService.findMany(COLLECTION_NAME, query,
                new FindOptions().setSkip(0).setLimit(0)).get();
    });
}
```

复杂的查询示例如下。同样需要指定数据表的名称、查询条件，但是查询选项会比较多，具体参见QueryOptions类。

```java
@Test @Order(6) void testQuery() {
    Assertions.assertDoesNotThrow(()->{
        ObjectNode query = JsonNodeFactory.instance.objectNode();
        query.put("author", "john doe1");
        Assertions.assertNotNull(databaseService.query(COLLECTION_NAME, query, null).get());
    });
}
```
