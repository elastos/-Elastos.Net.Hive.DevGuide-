# Scripting mechanism

用户选择可信 Hive Node 上订阅 Vault Service 来存储个人应用数据。原则上 Vault 中的数据与其他DID 用户访问完全隔离，仅允许 Vault Owner 通过DID才允许存储和访问。但是在实际的场景中，用户不仅需要存储个人私有数据，也需要存储局部范围内的公开数据。比如在类似Twitter等社交应用中，用户允许指定自己发布的blog内容分享给自己的 follower 用户浏览，也允许 follower 用户对 blog 内容贡献自己的评论，或者点赞等。

Users subscribe to Vault Service on trusted Hive Node to save personal application data. In principle, the data in the Vault is completely isolated from other DID users’ access, and only the Vault Owner is allowed to save and access through DID. However, in the actual scene, users not only need to save personal private data, but also need to save local public data. For example, in social applications like Twitter, users are allowed to designate their own blog content to share with their followers, and followers are also allowed to contribute their own comments or likes to blog content.

为了支持类似的交互应用场景，增加用户数据的交互性，Hive Node 提供了 Scripting 机制，帮助用户开发用户交互的社交，Game应用，但同时又保证用户对数据的ownership 所有权。Scripting 机制还处于雏形阶段，后面会参考更多的实际应用场景，以及开发者用户的反馈，不断完善和增加新的Features。

In order to support similar interactive application scenarios and increase the interactivity of user data, Hive Node provides a Scripting mechanism to help users develop social and Game applications for user interaction, while at the same time ensuring the ownership of data by users. The Scripting mechanism is still in the embryonic stage, and we will refer to more practical application scenarios as well as feedback from developers and users to continuously improve and add new Features.

通俗来讲，Scripting机制就是开发者在应用开发过程中，预先使用脚本来定义其他用户对指定范围内的 vault 数据的存储和访问权限。整个规则对所有者用户来说是透明。在这个规则下，Vault Owner 对该数据的存储有完全的控制权，预定义规则范围内的用户有指定规则内的数据访问权，而无关用户则完全被禁止该范围内的数据。

Generally speaking, the Scripting mechanism is that developers use scripts in advance to define the memory and access rights of other users to vault data within the specified range during the application development process. The whole rule is transparent to the user. Under this rule, the Vault Owner has complete control over the memory of this data, and users within the predefined rules have access to the data within the specified rules, while irrelevant users are completely prohibited from data within this range.

Scripting 机制通过两部分 Executable 和 Condition 来组织对应数据访问规则。通过 Condition 对象来限制数据访问者范围，通过 Executable 来限制存储数据和访问权限操作。

Scripting mechanism organizes the corresponding data access rules through two parts: Executable and Condition. The Condition object is used to limit the scope of data visitors, and the Executable object is used to limit the operation of data saving and access rights.

## Executable

开发者通过设计不同脚本规则来定义不同的数据交互类型和数据对象。比如通过上传脚本定义允许用户上传数据文件，或者指定数据库内条目的查询或插入等。开发者可以根据应用数据交互类型，来定义符合应用场景的脚本规则。

Developers define different data interaction types and data objects by designing different script rules. For example, users are allowed to upload data files through the definition of upload scripts; and the query or insertion of items in the database is specified. Developers can define script rules according to the application data interaction types.

开发者定义规则应该明确数据的访问对象范围，力求做到允许访问数据范围的最小集。比如：特定的数据库字段或指定文件的路径，允许用户在脚本里设计自定义参数。

The developers should define the scope of data access objects and strive to achieve the minimum set of allowed access data ranges when defining rules. For example, users are allowed to design customized parameters in scripts through a specific database field or the path of a specified file.

脚本的参数和类型被包装在脚本的 Executable（执行体）里面，作为脚本的组成部分之一。当然一个脚本可以包含多个执行体，它们用名称分开，这样可以在执行脚本的时候获取对应的返回值。

The parameters and types of the script are wrapped in the Executable of the script as one of the components of the script. Certainly, a script can contain multiple executables separated by names, so that the corresponding return values can be obtained when the script is executed.

## Condition

开发者通过定义 Condition 来定义数据访问者集合。对于 Vault 中的私有数据，数据访问者只能是 Owner DID。而对于访问脚本定义下的半公开数据，则其数据访问者通过 Condition 对象来定义。从另外角度来说，Condition 就类似于数据访问者的脚本规则。

A data visitor set can be defined by the developers through defining a Condition. For private data in the Vault, the data visitor can only be Owner DID. For the semi-public data defined by the access script, the data visitor is defined by the Condition object. From another point of view, Condition is similar to the script rules of data visitors.

当其他用户在访问某个Owner DID的 vault 数据时，会提供对应其对应的用户 DID 和应用 DID。而后端脚本通过Condition规则 验证该DID 和应用DID是否有权限访问限制数据。如果通过Condition规则，则开始执行 Executable 规则执行数据访问和存储，否则，则禁止执行 Exceutable 脚本规则。

When other users access the vault data of an Owner DID, they will provide the corresponding user DID and application DID. And the back-end script verifies whether the DID and the application DID have the right to access the restricted data through the Condition rule. If they pass, the Executable rule will be executed to perform data access and memory; otherwise, the executable script rule will be prohibited.

## Anonymous

除了支持定义 Condition 规则来限制数据访问者，Hive Node 的scripting 机制还支持匿名访问，包括用户 DID 匿名访问和应用 DID 匿名访问。 用户 DID 匿名访问就是不限制用户的 DID，任何 DID 用户都可以访问指定规则范围内的数据。而应用 DID 匿名访问就是不限制应用的 DID，任何应用 DID 应用上下文中都可以访问指定规则范围的数据。若用户和应用都设置了匿名，那么用户脚本对应的数据就完全公开。

In addition to supporting the definition of Condition rules to restrict data visitors, Hive Node’s scripting mechanism also supports anonymous access, including user DID anonymous access and application DID anonymous access. User DID anonymous access means that the user’s DID is not restricted, and any DID user can access the data within the specified rules. Anonymous access of application DID means unlimited application DID, and the data within the specified rule range can be accessed in the context of any application DID. If both the user and the application are anonymous, the data corresponding to the user script will be fully disclosed.

一般情况，用户可以通过匿名访问规则来设置avatar数据，供其他用户和应用上下都可以访问，这样达到 avatar 数据在各应用之间的无缝连通。后续匿名访问数据可以分享到IPFS 网络，供任何用户直接通过CID 从IPFS Gateway中访问获取。

Users can set avatar data through anonymous access rules, so that other users and applications can access it from top to bottom, to achieve seamless connection of avatar data among applications. Subsequent anonymous access data can be shared to the IPFS network for any user to access and obtain directly from the IPFS Gateway through CID.
