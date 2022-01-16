---
description: >-
  Users keep the ownership of vault data with DID-based authentication and
  authorization mechanism
---

# DID Authentication and Authorization

## Traditional authentication and authorization

传统web2.0领域中，中心机构或者组织一般采用以下两种机制来验证用户身份：

In the traditional field of web2.0, central institutions or organizations generally adopt the following two mechanisms to verify user identity:

* 独立的用户认证授权系统；
* 基于OAuth2标准协议的认证授权系统
* Independent user authentication and authorization system
* Authentication and authorization system based on OAuth2 standard protocol

这些本质上都是中心化的认证授权系统，所有用户账号和凭证信息都被强制要求存储在特定中心服务器中，而这些中心服务器明显由单一机构或组织所控制运营。从内部实现还是运营层面看，用户一旦将自己产生的数据存储到这些服务器时，也就被剥夺了对这些数据的所有权和控制权。基于自身利益驱动，这些中心机构或运营实体会以不公平的方式恶意审核甚至屏蔽用户继续正常使用服务，甚至于以用户隐私数据进行私下交易来谋求不当利润。

These are all centralized authentication and authorization systems in essence, and all user accounts and credential information are required to be saved in specific central servers, which are obviously controlled and operated by a single institution or organization. From the perspective of internal implementation or operation, once users save their own data in these servers, they are deprived of the ownership and control over these data. Driven by self-interest, these central institutions or operating entities will maliciously audit or even block users from continuing to use services normally in an unfair way, and even conduct private transactions with users’ private data to seek improper profits.

## DID-based authentication and authorization

基于DID的用户授权和认证系统中，DID代表用户身份，所有跟DID相关的用户凭证信息由用户自己管理和存储，不再需要在远程Server上创建用户账号，也不会存储用户的私密凭证。用户直接通过前端DID管理应用（例如Essentials钱包），授权第三方应用使用DID以及颁发访问凭证，在访问远程服务时验证用户身份和授权数据访问权限。

In the user authorization and authentication system based on DID, DID represents the user identity. All the credential information related to DID is managed and saved by the user, so it is no longer necessary to create a user account on the remote Server, nor to save the user’s private credentials. Users directly authorize third-party applications to use DID and issue access credentials through front-end DID management applications (such as Essentials wallet), and verify user identity and authorize data access rights when accessing remote services.

## The whole flow of authentication and athorization on Hive

Hive 服务深度集成DID，实现基于DID的用户验证和授权机制，支持用户通过DID身份向Hive服务存取应用数据。用户通过应用使用Hive服务的交互过程中，会涉及以下几个实体：

Hive service deeply integrates DID, realizes user authentication and authorization mechanism based on DID, and supports users to access application data from Hive service through DID identity. The following entities will be involved in the interaction process of users using Hive services through applications:

* Essentials Wallet
* 第三方应用
* Third-party application
* Hive 服务
* Hive service

用户在使用Hive服务之前，需要在前端应用Essentials Wallet创建或导入目标DID身份，同时保证该DID文档已公开发布到EID侧链中。后续所有的基于DID用户授权都在Essentials Wallet中完成。

Before users use Hive service, they need to apply Essentials Wallet in the front end to create or import the target DID identity, and at the same time ensure that the DID document has been publicly published to EID side chain. All subsequent DID-based user authorizations are completed in Essentials Wallet.

用户登录第三方应用时，打开Essentials Wallet向用户请求授权使用目标DID登录，用户在Essentials wallet中点击确认授权，用户通过DID身份正式登陆第三方应用。

When the user logs in to the third-party application, the Essentials wallet should be opened to send a request to the user for authorization of using the target DID. The user clicks on the Essentials Wallet to confirm authorization, and officially logs in to the third-party application through the DID identity.

当第三方应用需要需要将数据存储到Hive中时，通过Essentials Wallet向用户请求授权访问Hive服务，用户点击确认后，同意授权并向第三方应用签发访问凭证。第三方应用会基于用户DID以及访问凭证请求访问Hive中的数据存取服务。Hive Server验证DID以及访问凭证的真实性和有效性。一旦验证通过，Hive Server会根据会向第三方应用返回access token，后续第三应用就可以通过这个access token向Hive中存取应用数据。

When the third-party application needs to save data in Hive, it requests the user for authorization to access Hive service through Essentials Wallet. After the user confirms and clicks, an access credential will be authorized and issued to the third-party application. The third-party application will request access to data access services in Hive based on the user DID and access credentials. Hive Server verifies the authenticity and validity of DID and access credentials. Once the authentication is passed, the Hive Server will return to the access token, then the third-party application can access the application data in Hive through this access token.

## Conclusion

基于DID的用户认证和授权，将用户的授权验证过程真正完全地前置到用户的控制范围内，而后端Server仅是对具体已授权的DID身份和凭证进行真实性和有效性验证而已。通过基于DID的去中心化认证，后端Server就不再需要用户创建账号以及填充用户的私密信息，也就无法统一存储和管理用户信息，从而在机制上就避免服务端分析用户数据进而审核甚至屏蔽普通用户的合法访问权益。

Based on DID’s user authentication and authorization, the authorization verification process of the user is completely placed under the control of the user, while the back-end Server only verifies the authenticity and validity of the specific authorized DID identity and credential. Through the decentralized authentication based on DID, the back-end Server no longer needs user to create accounts and fill in users’ private information, so it can’t save and manage users’ information in a unified way, thus preventing the server from analyzing user data and then auditing or even shielding the legitimate access rights and interests of ordinary user.
