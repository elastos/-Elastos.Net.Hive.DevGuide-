# Snippet Code

如果要了解如何使用 Hive Java SDK（以下简称 SDK），其内的 Demo App 对于如何使用它有很好的展示。本文就以通过 SDK 上传文件到 Hive Node 为例，展示其完整的过程。

## 准备 DID

为了使用 SDK 的功能，你需要至少两个 DID : Application Instance DID 和 User DID 。DID 相关的操作，可以使用 Essentials 应用来完成，具体操作如下。

Application Instance DID 仅需要 mnemonic 和 passphrase，不需要上链，也无其它操作。

User DID 需要 mnemonic 和 passphrase，并且通过 Essentials 应用来上链。上链完成之后，需要在 Essentials 应用上使用 Hive 存储管理功能，选一个 Hive Node 节点进行绑定，绑定的同时会在 Hive Node 上创建 Vault Service。

## 创建 Vault

创建 Vault 之前需要准备好 AppContext 和 provider address，AppContext 可以通过如下方式创建。创建 AppContext 会用到 Application Instance DID 和 User DID 。

```java
AppContext appContext = AppContext.build(new AppContextProvider() {
    @Override
    public String getLocalDataDir() {
        // return local location for storing data.
        return getLocalStorePath();
    }

    @Override
    public DIDDocument getAppInstanceDocument() {
        // return the application instance DID document.
        try {
            return appInstanceDid.getDocument();
        } catch (DIDException e) {
            e.printStackTrace();
        }
        return null;
    }

    @Override
    public CompletableFuture<String> getAuthorization(String jwtToken) {
        // return the authorization string for auth API.
        return CompletableFuture.supplyAsync(() -> {
            try {
                Claims claims = new JwtParserBuilder().build().parseClaimsJws(jwtToken).getBody();
                if (claims == null)
                    throw new HiveException("Invalid jwt token as authorization.");
                return appInstanceDid.createToken(appInstanceDid.createPresentation(
                        userDid.issueDiplomaFor(appInstanceDid),
                        claims.getIssuer(), (String) claims.get("nonce")), claims.getIssuer());
            } catch (Exception e) {
                throw new CompletionException(new HiveException(e.getMessage()));
            }
        });
    }
}, userDid.toString());
```

provider address 就是之前绑定的 Hive Node 服务地址。通过 AppContext 和 provider address 就可以创建 Vault 了：

```java
Vault vault = new Vault(appContext, getVaultProviderAddress());
```

## 创建 FilesService

可以通过创建好的 Vault 来获取 FilesService：

```java
FilesService filesService = vault.getFilesService());
```

## 上传文件

有了 FilesService 便可以上传文件到 Hive Node 的文件存储内。上传文件时，先获取用于写的 Writer ，然后将文件内容写入即可完成整个上传文件的流程。

```java
public CompletableFuture<Void> writeFileContent(Writer writer) {
    return CompletableFuture.runAsync(() -> {
        try {
            writer.write(REMOTE_FILE_CONTENT);
            writer.flush();
            writer.close();
        } catch (IOException e) {
            throw new CompletionException(e);
        }
    });
}

filesService.getUploadWriter(REMOTE_FILE_PATH)
    .thenCompose(this::writeFileContent)
    .thenAcceptAsync(result -> hideLoadingWithMessage(null))
    .exceptionally(ex -> {
        hideLoadingWithMessage(ex);
        return null;
    });
```
