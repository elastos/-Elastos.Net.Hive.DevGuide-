# Integrate with Connectivity SDK

The web application using Hive JS SDK also needs integrate with Connectivity JS SDK which will connect with Essentials App.
Hive JS SDK provides [a helper class](https://github.com/elastos/Elastos.Hive.JS.SDK/blob/master/samples/ionic/src/app/hivejs/browser_helper.ts) for easy using.
Here will explain how it works.

## Application Context

To create some service, it is required AppContext object. For example, create Vault object.

```
  public async getVaultServices(userDid: string, providerAddress: string = null, onAuthError?: (e: Error) => void): Promise<Vault> {
    let appContext = await this.getAppContext(userDid, onAuthError);
    // if (!providerAddress)
    //   providerAddress = await AppContext.build(appContextProvider, userDid).getProviderAddress(userDid); // TODO: cache, don't resolve every time
    return new Vault(appContext, providerAddress);
  }
```

To create the AppContext, the process is this:

```
  public async getAppContext(userDid: string, onAuthError?: (e: Error) => void): Promise<AppContext> {
    let appInstanceDIDInfo = await this.didAccess.getOrCreateAppInstanceDID();

    console.log("hiveauthhelper", "Getting app instance DID document");
    let didDocument = await appInstanceDIDInfo.didStore.loadDid(appInstanceDIDInfo.did.toString());
    //appInstanceDIDInfo.didStore.loadDidDocument(appInstanceDIDInfo.did.getDIDString(), async (didDocument) => {
    console.log("hiveauthhelper", "Got app instance DID document. Now creating the Hive client", didDocument.toJSON());

    let appContextProvider: AppContextProvider = {
      getLocalDataDir: (): string => {
        return "/";
      },
      getAppInstanceDocument: (): DIDDocument => {
        return didDocument;
      },
      getAuthorization: (authenticationChallengeJWtCode: string): Promise<string> => {
        /**
         * Called by the Hive plugin when a hive backend needs to authenticate the user and app.
         * The returned data must be a verifiable presentation, signed by the app instance DID, and
         * including a appid certification credential provided by the identity application.
         */
        console.log("hiveauthhelper", "Hive client authentication challenge callback is being called with token:", authenticationChallengeJWtCode);
        try {
          return this.handleVaultAuthenticationChallenge(authenticationChallengeJWtCode);
        }
        catch (e) {
          console.error("hiveauthhelper", "Exception in authentication handler:", e);
          if (onAuthError)
            onAuthError(e);
          return null;
        }
      }
    }

    return AppContext.build(appContextProvider, userDid, AppDID.APP_DID);
  }
```

## Backup Credential

The backup credential is for backup from the vault node to the backup node. Here is how to get it.

```
  async getBackupCredential(srcDid: string, targetDid: string, targetHost: string): Promise<string> {
      const credential = await this.didAccess.generateHiveBackupCredential(srcDid, targetDid, targetHost);
      console.log(`backup credential: ${credential.toString(true)}`);
      return credential.toString(true);
  }
```
