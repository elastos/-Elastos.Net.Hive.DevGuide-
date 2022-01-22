# ScriptingService

On Hive Node, in order to support Vault data sharing to designated or all visitors, a Scripting Service is provided. Scripting Service provides executable scripts to share data (file or database data) in the Vault with others. The controller of the data designs and registers the script, and the user of the data executes the script to operate the data.

An example of registration scripts is as follows:

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

This example demonstrates how to register a script. When doing so, you need to set a condition to restrict the users of the data. In addition, the content of the script (executable) is a template, which allows the user who calls the script to insert the content (type is InsertExecutable) into the data table. At the same time, the script limits the data that data visitors can access (parameters defined by $params). After registering the script, visitors who meet the conditions can insert data by executing the script. The usage scenario here is to send messages to the Vault of the data controller.

### Register Script

According to the design of Scripting service, the controller of scripts can register 8 different types of scripts, share file contents, and perform database operations.

* Insert database data
* Update database data
* Delete database data
* Search database data
* Upload file
* Download file
* Get file attribute information
* Get file Hash value

Script data related to specific types contains two pieces of content:

* condition: the conditions set for the visitor. When the visitor executes the script, the correct execution effect can be obtained only if the conditions are met.
* executable: the definition area of the script, which defines the type and parameter template of the specified type of script.

The following is a method prototype of the registration script. The parameters for registering the script include: the name of the script, conditions for executing the script, content of the script, whether anonymous users are allowed or not, and whether anonymous applications are allowed or not.

```swift
func registerScript(_ name: String, _ executable: Executable) -> Promise<Void>
func registerScript(_ name: String, _ condition: Condition, _ executable: Executable) -> Promise<Void>
func registerScript(_ name: String, _ executable: Executable, _ allowAnonymousUser: Bool, _ allowAnonymousApp: Bool) -> Promise<Void>
func registerScript(_ name: String, _ condition: Condition?, _ executable: Executable, _ allowAnonymousUser: Bool, _ allowAnonymousApp: Bool) -> Promise<Void>
    
```

### Unregister Script

Remove the script with the specified name, after which the script will no longer be executable.

```
func unregisterScript(_ name: String) -> Promise<Void>
```

## ScriptRunner

According to the description of Scripting Service, if the data visitor needs to execute scripts, they can only be done so through Script Runner. An example of execution scripts corresponding to the above registered scripts is as follows:

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

The parameters of Script Runner are the same as the ones used when creating the Vault, and the user DID that is provided belongs to the visitor of the data. During the execution of the script, the parameters of the script template need to be set, and the controller of the script needs to be specified. Then the script is executed by name and the data is manipulated.

### Run Script

Executing script operation is just like the above example; besides providing the necessary parameter values, you also need to specify the return value type (resultType).

```swift
func callScript<T>(_ name: String, _ params: [String : Any]?, _ targetDid: String, _ targetAppDid: String, _ resultType: T.Type) -> Promise<T>
```

### Upload File

There are eight script types when registering scripts. One more step is needed for two situations: uploading and downloading files. When executing the uploading file script, the transaction ID will be returned first, and then the upload interface will be called to upload the file to the Vault corresponding to the script. At this point, the resultType is the return value of the upload type script execution.

```swift
func uploadFile(_ transactionId: String) -> Promise<FileWriter>
```

### Download File

The operation process of downloading a file is similar to that of uploading a file, and the content of the specified file is downloaded through the transaction ID.

```swift
func downloadFile(_ transactionId: String) -> Promise<FileReader>
```
