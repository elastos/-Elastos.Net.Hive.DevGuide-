# Scripting Mechanism

Users subscribe to Vault Service on a trusted Hive Node to save personal application data. In principle, the data in the Vault is completely isolated from other DID users’ access, and only the Vault owner is allowed to save and access through DID. However, in practice, users not only need to save personal private data but also local public data. For example, in social applications like Twitter, users are allowed to designate their own blog content to share with their followers, and followers are also allowed to contribute their own comments or likes to blog content.

In order to support similar interactive application scenarios and increase the interactivity of user data, Hive Node provides a scripting mechanism to help users develop social and game applications for user interaction, while at the same time ensuring the ownership of data by users. The scripting mechanism is still in the embryonic stage, and we will refer to more practical application scenarios as well as feedback from developers and users to continuously improve and add new features.

Generally speaking, the scripting mechanism is where developers use scripts in advance to define the memory and access rights of other users to vault data within the specified range during the application development process - the whole rule is transparent to the user. Under this rule, the Vault Owner has complete control over the memory of this data, and users within the predefined rules have access to the data within the specified rules, while irrelevant users are completely prohibited from data within this range.

Scripting mechanism organizes the corresponding data access rules through two parts: Executable and Condition. The Condition object is used to limit the scope of data visitors, and the Executable object is used to limit the operation of data saving and access rights.

## Executable

Developers define different data interaction types and data objects by designing different script rules. For example, users are allowed to upload data files through the definition of upload scripts, and the query or insertion of items in the database is specified. Developers can define script rules according to the application data interaction types.

The developers should define the scope of data access objects and strive to achieve the minimum set of allowed access data ranges when defining rules. For example, users are allowed to design customized parameters in scripts through a specific database field or the path of a specified file.

The parameters and types of the script are wrapped in the Executable of the script as one of the components of the script. A script can contain multiple executables separated by names so that the corresponding return values can be obtained when the script is executed.

## Condition

A data visitor set can be defined by the developers through defining a Condition. For private data in the Vault, the data visitor can only be Owner DID. For the semi-public data defined by the access script, the data visitor is defined by the Condition object. From another point of view, Condition is similar to the script rules of data visitors.

When other users access the vault data of an Owner DID, they will provide the corresponding user and application DID. And the back-end script verifies whether the DID and the application DID have the right to access the restricted data through the Condition rule. If they pass, the Executable rule will be executed to perform data access and memory; otherwise, the executable script rule will be prohibited.

## Anonymous

In addition to supporting the definition of Condition rules to restrict data visitors, Hive Node’s scripting mechanism also supports anonymous access, including user DID anonymous access and application DID anonymous access. User DID anonymous access means that the users DID is not restricted, and any DID user can access the data within the specified rules. Anonymous access of application DID means unlimited application DID, and the data within the specified rule range can be accessed in the context of any application DID. If both the user and the application are anonymous, the data corresponding to the user script will be fully disclosed.

Users can set avatar data through anonymous access rules so that other users and applications can access it from top to bottom, achieving the seamless connection of avatar data among applications. Subsequent anonymous access data can be shared to the IPFS network for any user to access and obtain directly from the IPFS Gateway through CID.
