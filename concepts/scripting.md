# Scripting service

Scripting 是一套由开发者在应用开发时，根据具体应用中的数据交互场景，预定义的一套用于访问指定范围内数据集的供其他用户访问者进行数据存储和访问规则。开发者可以通过Scripting Service 接口来实现复杂的社交应用或者交互式游戏应用，在应用内可以进行数据局部范围内分享和交互。

Scripting is a set of data memory and access rules predefined by developers to access data sets within a specified range for other user visitors according to data interaction scenarios in specific applications during application development. Developers can realize complex social applications or interactive game applications through the Scripting Service interface, in which data can be shared and interacted locally.

Scripting Service 由 Condition 和 Executalbe 等规则组合使用。

Scripting Service is used by a combination of rules such as Condition and Executable.

## Executable

Executable 是一组对指定数据集的操作规则。开发者在开发应用时，根据具体应用场景，对指定数据集（例如 Vault 中的文件数据和数据库中条目数据）预定义规则。只有对于满足 Condition 条件中的用户DID和应用 DID，可以执行预定义规则脚本。

Executable is a set of operation rules for a specified data set. When developers develop applications, according to specific application scenarios, predefined rules for specified data sets (such as file data in Vault and item data in database). Only the user DID and application DID that meet the conditions in Condition can execute the predefined rule script.

开发者定义规则应该明确数据的访问对象范围，力求做到允许访问数据范围的最小集。比如：特定的数据库字段或指定文件的路径，允许用户在脚本里设计自定义参数。

During the rule definition, developers should define the scope of data access objects, and strive to achieve the minimum set of allowed access data ranges. For example, a specific database field or the path of a specified file, which allows users to design customized parameters in scripts.

脚本的参数和类型被包装在脚本的 Executable（执行体）里面，作为脚本的组成部分之一。当然一个脚本可以包含多个执行体，它们用名称分开，这样可以在执行脚本的时候获取对应的返回值。

The parameters and types of the script are wrapped in the Executable of the script as one of the components of the script. A script can contain multiple executors separated by names, so that the corresponding return values can be obtained when the script is executed.

## Condition

Condition 是一组对访问者集合的过滤规则。开发者在开发应用时，根据具体应用场景，除了对限定对指定数据的预定义访问和存储规则，也需要对访问者进限定过滤。只有满足 Condition 条件中的用户 DID和应用 DID，才允许执行预定义规则，操作规则范围的 vault 数据。

Condition is a set of filtering rules for visitor collection. When developers are developing applications, according to the specific application scenarios, apart from the predefined access and memory rules that restrict the specified data, they also need to restrict the visitors. Only when the user DID and the application DID in the conditions of Condition are met are the predefined rules allowed to be executed and the vault data within the scope of operation rules allowed.

当其他用户在访问某个Owner DID的 vault 数据时，会提供对应其对应的用户 DID 和应用 DID。而后端脚本通过Condition规则 验证该DID 和应用DID是否有权限访问限制数据。如果通过Condition规则，则开始执行 Executable 规则执行数据访问和存储，否则，则禁止执行 Exceutable 脚本规则。

When other users access the vault data of an Owner DID, they will provide the corresponding user DID and application DID. And the back-end script verifies whether the DID and the application DID have the right to access the restricted data through the Condition rule. If the Condition rule is passed, the Executable rule will be executed to perform data access and memory; otherwise, the executable script rule will be prohibited.

## Anonymous

除了支持定义 Condition 规则来限制数据访问者，Hive Node 的scripting 机制还支持匿名访问，包括用户 DID 匿名访问和应用 DID 匿名访问。 用户 DID 匿名访问就是不限制用户的 DID，任何 DID 用户都可以访问指定规则范围内的数据。而应用 DID 匿名访问就是不限制应用的 DID，任何应用 DID 应用上下文中都可以访问指定规则范围的数据。

In addition to supporting the definition of Condition rules to restrict data visitors, Hive Node’s scripting mechanism also supports anonymous access, including user DID anonymous access and application DID anonymous access. User DID anonymous access means that the user’s DID is not restricted, and any DID user can access the data within the specified rules. Anonymous access of application DID means unlimited application DID, and the data within the specified rule range can be accessed in the context of any application DID.
