# Authorized access {#concept_32059_zh .concept}

The iOS SDK provides STS authentication and self-signed modes to secure mobile devices.

## STS authentication mode {#section_tpr_krj_lfb .section}

You can use Alibaba Cloud STS to authorize temporary access to OSS. STS is a Web service that provides a temporary access token to a cloud computing user. You can use STS to assign a third-party application or a federated user \(you can manage the user ID\) an access credential with a custom validity period and permissions. The access credential in the application is called a federation token. The third-party application or federated user can use the access credential to directly call the Alibaba Cloud service APIs or use the Alibaba Cloud service SDKs to access the cloud service APIs.

-   STS benefits:
    -   You do not need to expose your long-term key \(AccessKey\) to a third-party application. You need only to generate an access token and send the access token to the third-party application. You can customize the access permissions and validity period of this token.
    -   The access token automatically becomes invalid after expiration.
-   How to use STS
    1.  Log on to AppServer as an application user.

        You can manage your application ID. You can customize the ID management system or use an external Web account or open ID. For each valid application user, AppServer can precisely define their minimum access permissions.

    2.  AppServer requests a security token from STS.
        1.  Before calling STS, AppServer needs to determine the minimum access permissions \(described in the policy syntax\) of the application user and the expiration time of the authorization.
        2.  Then, you can call the [AssumeRole](../../../../reseller.en-US/API Reference (STS)/Operation interfaces/AssumeRole.md#) of STS to obtain the security token. For more information about how to manage and use roles, see [Role management](../../../../reseller.en-US//Identities/Roles.md#) in RAM User Guide.
    3.  STS returns a valid access credential to AppServer. The access credential contains a security token, a temporary AccessKey pair \(AccessKey ID and AccessKey Secret\), and the expiration time.
    4.  AppServer returns the access credential to ClientApp.

        ClientApp caches this credential. When the credential becomes invalid, ClientApp must request a new valid access credential from AppServer. For example, if the access credential is valid for one hour, ClientApp can request AppServer to update the access credential every 30 minutes.

    5.  ClientApp uses the locally cached access credential to request Alibaba Cloud service APIs. Cloud services detect the STS access credential. STS verifies the credential, and correctly responds to user requests.
    -   Set an STS token directly

        Before you log on to ClientApp, you can use a certain method \(such as sending network requests from your AppServer\) to obtain an STS token. Then, use the token to initialize the SDK. If this method is adopted, you must take note of the validity period of the STS token. When the STS token is about to expire, you must manually update the STS token for the SDK.

        The initialization code is as follows:

        ```
        id<OSSCredentialProvider> credential = [[OSSStsTokenCredentialProvider alloc] initWithAccessKeyId:@"<StsToken.AccessKeyId>" secretKeyId:@"<StsToken.SecretKeyId>" securityToken:@"<StsToken.SecurityToken>"];
        client = [[OSSClient alloc] initWithEndpoint:endpoint credentialProvider:credential];
        ```

        When you notice that the token is about to expire, you can rebuild a new OSSClient or use the following method to update the CredentialProvider field value:

        ```
        id<OSSCredentialProvider> credential = [[OSSStsTokenCredentialProvider alloc] initWithAccessKeyId:@"<StsToken.AccessKeyId>" secretKeyId:@"<StsToken.SecretKeyId>" securityToken:@"<StsToken.SecurityToken>"];
        client = [[OSSClient alloc] initWithEndpoint:endpoint credentialProvider:credential];
        ```

    -   Implement a callback to obtain and update an STS token

        If you want the SDK to automatically help you manage token updates, you must configure the SDK to obtain the token. To implement a callback, you must obtain a federation token \(STS token\) and send the token to the SDK. The SDK signs the token. When the token needs to be updated, the SDK calls the callback to obtain the token, as shown in the following figure.

        ```
        id<OSSCredentialProvider> credential = [[OSSFederationCredentialProvider alloc] initWithFederationTokenGetter:^OSSFederationToken * {
            // Here you need to obtain a federation token, construct it into the OSSFederationToken object, and send the object to the SDK.
            // If the token fails to be obtained for some reason, nil is returned.
              OSSFederationToken * token;
            // The following code is used to obtain the token from AppServer:
            ...
            return token;
        }];
        client = [[OSSClient alloc] initWithEndpoint:endpoint credentialProvider:credential];
        ```

        **Note:** Additionally, if you use other means to obtain all fields required by the token, you can directly send the token to the SDK and implement the callback. In this case, you need to manually update the token and then reset the value of OSSCredentialProvider of the OSSClient instance.

        Example:

        Assume that your server address is http://localhost:8080/distribute-token.json. The following data is returned after the address is accessed:

        ```
        {
        	"StatusCode": 200,
        	"AccessKeyId":"STS.iA645eTOXEqP3cg3VeHf",
        	"AccessKeySecret":"rV3VQrpFQ4BsyHSAvi5NVLpPIVffDJv4LojUBZCf",
        	"Expiration":"2015-11-03T09:52:59Z",
        	"SecurityToken":"CAES7QIIARKAAZPlqaN9ILiQZPS+JDkS/GSZN45RLx4YS/p3OgaUC+oJl3XSlbJ7StKpQ...."}
        
        ```

        You can use the following code to implement the `OSSFederationCredentialProvider` instance:

        ```
        id<OSSCredentialProvider> credential2 = [[OSSFederationCredentialProvider alloc] initWithFederationTokenGetter:^OSSFederationToken * {
            // Construct a request to access your AppServer.
            NSURL * url = [NSURL URLWithString:@"http://localhost:8080/distribute-token.json"];
            NSURLRequest * request = [NSURLRequest requestWithURL:url];
            OSSTaskCompletionSource * tcs = [OSSTaskCompletionSource taskCompletionSource];
            NSURLSession * session = [NSURLSession sharedSession];
            //Send the request.
            NSURLSessionTask * sessionTask = [session dataTaskWithRequest:request
                                                        completionHandler:^(NSData *data, NSURLResponse *response, NSError *error) {
                                                            if (error) {
                                                                [tcs setError:error];
                                                                return;
                                                            }
                                                            [tcs setResult:data];
                                                        }];
            [sessionTask resume];
            // Wait until the response is returned.
            [tcs.task waitUntilFinished];
            // Parse the result.
            if (tcs.task.error) {
                NSLog(@"get token error: %@", tcs.task.error);
                return nil;
            } else {
                // The returned data is in the JSON format and you must parse it to obtain all fields of the token.
                NSDictionary * object = [NSJSONSerialization JSONObjectWithData:tcs.task.result
                                                                        options:kNilOptions
                                                                          error:nil];
                OSSFederationToken * token = [OSSFederationToken new];
                token.tAccessKey = [object objectForKey:@"AccessKeyId"];
                token.tSecretKey = [object objectForKey:@"AccessKeySecret"];
                token.tToken = [object objectForKey:@"SecurityToken"];
                token.expirationTimeInGMTFormat = [object objectForKey:@"Expiration"];
                NSLog(@"get token: %@", token);
                return token;
            }
        }];
        ```


## Self-signed mode { .section}

```language-objc
id<OSSCredentialProvider> credential = [[OSSCustomSignerCredentialProvider alloc] initWithImplementedSigner:^NSString *(NSString *contentToSign, NSError *__autoreleasing *error) {
    // Here you need to use the signature algorithm specified by OSS to sign a string, add the AccessKey ID to the signed string, and send the signed string to AppServer.
    // Typically, you can upload the string to AppServer. AppServer returns the signature.
    // If the string fails to be signed, error information is recorded, and nil is returned.
NSString *signature = [OSSUtil calBase64Sha1WithData:contentToSign withSecret:@"<your accessKeySecret>"]; // The ossutil tool in the SDK is used to complete the local signature. We recommend that you use AppServer to implement the remote signature.
    if (signature ! = nil) {
        *error = nil;
    } else {
        *error = [NSError errorWithDomain:@"<your domain>" code:-1001 userInfo:@"<your error info>"];
        return nil;
    }
    return [NSString stringWithFormat:@"OSS %@:%@", @"<your accessKeyId>", signature];
}];

client = [[OSSClient alloc] initWithEndpoint:endpoint credentialProvider:credential];

```

**Note:** For STS authentication and self-singed modes, you must ensure that the response is correct after you implement the callback. Therefore, if you send a network request to AppServer to obtain the token and signature, we recommend that you call the synchronous APIs of the network library or perform the conversion from async to sync. The callback is run in the subthread of the request from the SDK, which does not block the main thread.

