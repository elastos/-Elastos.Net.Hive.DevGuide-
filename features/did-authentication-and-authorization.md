---
description: >-
  Users keep the ownership of vault data with DID-based authentication and
  authorization mechanism
---

# DID Authentication and Authorization

## Traditional Authentication and Authorization

In the traditional Web3 arena, central institutions or organizations generally adopt the following two mechanisms to verify user identities:

* Independent user authentication and authorization systems
* Authentication and authorization systems based on OAuth2 standard protocol

In essence, these are all centralized authentication and authorization systems - all user accounts and credential information are required to be saved on specific central servers, which are obviously controlled and operated by a single institution or organization. From the perspective of internal implementation or operations, once users save their own data on these servers, they're deprived of the ownership and control over the data. Driven by self-interest, these central institutions or operating entities will maliciously audit or even block users from continuing to use services in an unfair way, and even conduct private transactions with users’ private data to seek profits.

## DID-based Authentication and Authorization

In the user authorization and authentication systems based on DID, the DID itself represents the user identity. All the credential information related to DID is managed and saved by the user, so it's no longer necessary to create a user account on the remote server, nor required to save the user’s private credentials. Users directly authorize third-party applications to use DID and issue access credentials through front-end DID management applications (such as Essentials wallet) and verify user identity and authorize data access rights when accessing remote services.

## The Whole Flow of Authentication and Authorization on Hive

Hive deeply integrates DID while realizing the user authentication and authorization mechanisms based on DID. It also supports users by allowing them to access application data from Hive through DID identity. The following entities will be involved in the interaction process of users using Hive services through applications:

* Essentials Wallet
* Third-party applications
* Hive service

Before users begin to utilize Hive service, they need to apply Essentials Wallet in the front end to create or import the target DID identity, and at the same time ensure that the DID document has been published to the EID sidechain. All subsequent DID-based user authorizations are completed in the Essentials Wallet.

When the user logs in to the third-party application, the Essentials wallet should be opened to send a request to the user for authorization of using the target DID. The user clicks on the Essentials Wallet to confirm authorization, and officially logs in to the third-party application through the DID identity.

When the third-party application needs to save data in Hive, it requests authorization from the user to access Hive service through Essentials Wallet. After the user confirms and clicks, an access credential will be authorized and issued to the third-party application. The third-party application will then request access to data access services in Hive based on the user DID and access credentials. Hive server verifies the authenticity and validity of DID and access credentials. Once the authentication is passed, the Hive Server will return to the access token, then the third-party application can access the application data in Hive through this access token.

## Conclusion

Based on DID’s user authentication and authorization, the user authorization verification process is completely placed under their control, while the back-end server only verifies the authenticity and validity of the specific authorized DID identity and credentials. Through the decentralized authentication based on DID, the back-end server no longer needs the user to create accounts and fill in their private information, so it can’t save and manage users’ information in a unified way, thus preventing the server from analyzing user data and then auditing or even shielding the legitimate access rights and interests of ordinary user.
