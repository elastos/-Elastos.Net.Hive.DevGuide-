# ScriptingService

## ScriptingService

Hive Node 上，为了支持 Vault 数据分享给指定或者所有的访问者，提供了Scripting Service。Scripting Service 通过提供可执行脚本来将 Vault 中的数据（文件或数据库数据）分享给其他人。数据的拥有者设计、注册脚本，数据的使用者执行脚本操作数据。

On Hive Node, in order to support Vault data sharing to designated or all visitors, a Scripting Service is provided. Scripting Service provides executable scripts to share data (file or database data) in the Vault with others. The controller of the data designs and registers the script, and the user of the data executes the script to operate the data.

注册脚本的例子如下：

Example of registration scripts is as follows:

```swift
let vault = try Vault(context, providerAddress)
let scriptingService = vault.scriptingService
// The condition to restrict the user DID.
let filter = ["collection" : COLLECTION_GROUP_MESSAGE, "did" : "$caller_did"]
// The message is for inserting to the COLLECTION_GROUP_MESSAGE.
let doc = ["author" : "$params.author", "content" : "$params.content"]
let options = ["bypass_document_validation" : false, "ordered" : true]
// register the script for caller to insert message to COLLECTION_GROUP_MESSAGE
scriptingService!.registerScript(SCRIPT_NAME, QueryHasResultCondition("get_group_message", COLLECTION_GROUP, filter), InsertExecutable(SCRIPT_NAME, COLLECTION_GROUP_MESSAGE, doc, options))
.done { success in
   print("Successfully get the result.")
}
.catch { error in
    print(error)
}
```

该例子演示了如何注册一个脚本，注册脚本的时候需要设置条件（ condition ），用于限制数据的使用者。另外就是脚本的内容（ executable ）为一个模版，允许调用脚本的用户可以往数据表里插入内容（类型为 InsertExecutable ）。同时脚本里面限制了数据的访问者能访问的数据（$params定义的参数）。注册完脚本后，满足条件的访问者可以通过执行脚本来插入数据。此处的使用场景为，往数据拥有者的 Vault 里面发送消息。

This example demonstrates how to register a script. When registering a script, you need to set condition (condition) to restrict the users of data. In addition, the content of the script (executable) is a template, which allows the user who calls the script to insert the content (type is InsertExecutable) into the data table. At the same time, the script limits the data that data visitors can access (parameters defined by $params). After registering the script, visitors who meet the conditions can insert data by executing the script. The usage scenario here is to send messages to the Vault of the data controller.

### Register Script

按照Scripting服务的设计，脚本的拥有者可以注册8种不同类型的脚本，分享文件内容和执行数据库操作。

According to the design of Scripting service, the controller of scripts can register 8 different types of scripts, share file contents and perform database operations.

* 插入数据库数据
* Insert database data
* 更新数据库数据
* Update database data
* 删除数据库数据
* Delete database data
* 查询数据库数据
* Search database data
* 上传文件
* Upload file
* 下载文件
* Download file
* 获取文件属性信息
* Get file attribute information
* 获取文件Hash值
* Get file Hash value

涉及到具体类型的脚本数据包含两块内容：

Script data related to specific types contains two pieces of content:

* condition：对访问者设置的条件，访问者执行脚本的时候，满足条件才能得到正确的执行效果
* condition: the conditions set for the visitor. When the visitor executes the script, the correct execution effect can be obtained only if the conditions are met.
* executable: 脚本的定义区域，定义了指定类型脚本的类型、参数模版等
* executable: the definition area of the script, which defines the type and parameter template of the specified type of script.

下面是一个注册脚本的方法原型。注册脚本的参数有：脚本的名称、执行脚本的条件、脚本内容、是否允许匿名用户、是否允许匿名应用

The following is a method prototype of the registration script. The parameters for registering the script include: the name of the script, the conditions for executing the script, the content of the script, whether anonymous users are allowed or not, and whether anonymous applications are allowed or not.

```swift
func registerScript(_ name: String, _ executable: Executable) -> Promise<Void>
func registerScript(_ name: String, _ condition: Condition, _ executable: Executable) -> Promise<Void>
func registerScript(_ name: String, _ executable: Executable, _ allowAnonymousUser: Bool, _ allowAnonymousApp: Bool) -> Promise<Void>
func registerScript(_ name: String, _ condition: Condition?, _ executable: Executable, _ allowAnonymousUser: Bool, _ allowAnonymousApp: Bool) -> Promise<Void>
    
```

### Unregister Script

移除指定名称的脚本，移除之后脚本将不在可执行。

Remove the script with the specified name, after which the script will no longer be executable.

```
func unregisterScript(_ name: String) -> Promise<Void>
```

## ScriptRunner

按照 Scripting Service的说明，数据的访问者若是需要执行脚本，只能通过 Script Runner来执行。与上面注册脚本相对应的执行脚本的例子如下：

According to the description of Scripting Service, if the data visitor needs to execute scripts, they can only be executed through Script Runner. Example of execution scripts corresponding to the above registered scripts is as follows:

```swift
let params = ["author" : "John", "content" : "message"]
let scriptRunner = try ScriptRunner(callerContext, providerAddress)
scriptRunner.callScript(SCRIPT_NAME, params, userDid, appDid, JSON.self)
.done { result in
   print("Successfully get the result.")
}
.catch { error in
   print(error)
}
```

Script Runner 用到的参数和创建 Vault 的时候一样，只是提供的 user DID 属于数据的访问者。执行脚本的时候，就是设置好脚本模版的参数、指定脚本的拥有者，然后通过名称执行脚本，操作数据。

The parameters of Script Runner are the same as the ones used when creating the Vault, and the user DID that is provided belongs to the visitor of the data. During the execution of the script, the parameters of the script template need to be set, and the controller of the script needs to be specified. Then the script is executed by name and the data is manipulated.

### Run Script

执行脚本操作就如上面的示例一样，除了提供必要的参数值之外，还需要指定返回值类型（resultType）

Executing script operation is just like the above example, besides providing the necessary parameter values, you also need to specify the return value type (resultType).

```swift
func callScript<T>(_ name: String, _ params: [String : Any]?, _ targetDid: String, _ targetAppDid: String, _ resultType: T.Type) -> Promise<T>
```

### Upload File

注册脚本时候的八种脚本类型，有两种情况是需要多执行一步：上传、下载文件。执行上传文件脚本的时候，会先返回事务 ID，然后再调用上传接口将文件上传到脚本对应的 Vault 中，此时 resultType 才是上传类型脚本执行的返回值。

There are eight script types when registering scripts. One more step is needed for two situations: uploading and downloading files. When executing the uploading file script, the transaction ID will be returned first, and then the upload interface will be called to upload the file to the Vault corresponding to the script. At this point, the resultType is the return value of the upload type script execution.

```swift
func uploadFile(_ transactionId: String) -> Promise<FileWriter>
```

### Download File

下载文件和上传文件的操作流程类似，通过事务 ID，下载到指定文件的内容。

The operation process of downloading a file is similar to that of uploading a file, and the content of the specified file is downloaded through the transaction ID.

```swift
func downloadFile(_ transactionId: String) -> Promise<FileReader>
```
