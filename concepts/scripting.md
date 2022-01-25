# Scripting Service

Scripting is a set of data memory and access rules predefined by developers to access data sets within a specified range for other user visitors according to data interaction scenarios in specific applications during application development. Developers can realize complex social or interactive game applications through the Scripting service interface, in which data can be shared and interacted locally.

Scripting service is used by a combination of rules such as Condition and Executable.

## Executable

Executable is a set of operational rules for a specified data set. When developers create and build applications according to specific application scenarios, they need to deploy predefined rules for specified data sets (such as file data in Vault and item data in database). Only the user and application DIDs that meet the conditions in "Condition" can execute the predefined rule script.

During the rule definition, developers should define the scope of data access objects and strive to achieve the minimum set of allowed access data ranges. For example, a specific database field or the path of a specified file, allows users to design customized parameters in scripts.

The parameters and types of the script are wrapped in the Executable of the script as one of its components. A script can contain multiple executors separated by names so that the corresponding return values can be obtained when the script is executed.

## Condition

Condition is a set of filtering rules for visitor collection. When developers are building applications according to the specific application scenarios (apart from the predefined access and memory rules that restrict the specified data), they also need to restrict the visitors. Only when the user and application DID that meet the "Condition" are the predefined rules allowed to be executed and the Vault data within the scope of operation rules allowed.

When other users access the Vault data of an Owner DID, they will provide the corresponding user and application DID. The backend script verifies whether the DID and application DID have the right to access the restricted data through the Condition rule. If the Condition rule is passed, the Executable rule will be executed to perform data access and memory; otherwise, the executable script rule will be prohibited.

## Anonymous

In addition to supporting the definition of Condition rules to restrict data visitors, Hive Node’s scripting mechanism also supports anonymous access, including user and application DID anonymous access. User DID anonymous access means that the user's DID isn't restricted, so any DID user can access the data within the specified rules. Anonymous access of application DID means unlimited application DID, and the data within the specified rule range can be accessed in the context of any application DID.
