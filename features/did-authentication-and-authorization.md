---
description: >-
  Users keep the ownership of vault data with DID-based authentication and
  authorization mechanism
---

# DID Authentication and Authorization

## Traditional authentication and authorization

传统web2.0领域中，运营商或者组织一般采用以下两种机制来验证用户身份：

* 独立的用户认证授权系统；
* 基于OAuth2标准协议的认证授权系统

这些本质上都是中心化的认证授权系统，所有用户账号和凭证信息都被强制要求存储在特定中心服务器或者集群中。同时，这些服务器以及集群都由单一运营商或者组织所控制。从内部实现还是运营层面看，用户一旦将自己产生的数据存储到这些服务器时，也就被剥夺了对这些数据的所有权和控制权。基于自身利益的驱动，这些运营商和组织会以不公平的方式恶意审核甚至任意屏蔽用户继续使用服务，甚至于以用户隐私数据进行交易来追求利润。

## DID-based authentication and authorization

基于DID的用户授权和认证系统中，DID代表用户身份，所有跟DID相关的用户凭证信息由用户自己管理和存储，不再需要在远程Server上创建用户账号过程，也不会存储用户的私密凭证。用户直接通过前端DID管理应用（例如Essentials钱包应用），授权第三方应用使用DID和颁发凭证开始访问远程服务时验证用户身份以及授权数据访问权限。

## The whole flow of authentication and athorization on Hive

Hive 服务深度集成DID，实现基于DID的用户验证和授权机制，支持用户通过DID身份向Hive服务存取应用数据。这样，用户通过应用使用Hive服务的交互过程中，会涉及以下几个实体：

* 用户
* Essentials Wallet
* 第三方应用
* Hive 服务

用户在使用Hive服务之前，需要在前端应用Essentials Wallet创建或导入目标DID身份，同时保证该DID已发布到EID侧链中。后续所有的基于DID用户授权都在Essentials Wallet中完成。

用户登录第三方应用时，打开Essentials Wallet向用户请求授权使用目标DID登录，用户在Essentials wallet中点击确认授权，用户通过DID身份正式登陆第三方应用。

当第三方应用需要需要将数据存储到Hive中时，通过Essentials Wallet向用户请求授权访问Hive服务，用户点击确认后，同意授权并向第三方应用签发访问凭证。第三方应用会将用户DID以及访问凭证请求访问Hive中的数据存取服务。Hive Server验证DID以及访问凭证的真实性和有效性。一旦验证通过，Hive Server会根据请求用户的DID身份授权对应的数据访问权限。

## Conclusion

基于DID的用户认证和授权，将用户的授权验证过程真正完全地前置到用户的控制范围内，而后端Server仅是对具体已授权的DID身份和凭证进行真实性和有效性验证而已。通过基于DID的去中心化认证，后端Server就不再需要用户创建账号以及填充用户的私密信息，也就无法统一存储和管理用户信息，从而在机制上就避免服务端分析用户数据进而审核甚至屏蔽普通用户的合法访问权益。
