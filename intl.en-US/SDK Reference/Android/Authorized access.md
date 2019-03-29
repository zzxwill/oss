# Authorized access {#concept_32046_zh .concept}

This topic describes how to perform access control.

## Access control {#section_yd2_ndg_lfb .section}

A mobile device is an untrusted environment. Therefore, the SDK provides two authentication modes based on your servers: `STS authentication mode` and `self-signed mode`.

## STS authentication mode { .section}

-   Introduction

    OSS can temporarily grant access authorizations using the Alibaba Cloud STS service. Alibaba Cloud STS \(Security Token Service\) is a web service providing temporary access tokens for cloud computing users. With the STS, you can assign a third-party application or federated user \(you can manage the user ID\) an access credential with a custom validity period and permissions. The access credential in the application is called FederationToken. Third-party applications or federated users can use these access credentials to directly call the Alibaba Cloud APIs or use the SDKs provided by Alibaba Cloud to access the APIs of cloud products.

    -   You do not need to disclose your long-term key \(AccessKey\) to a third-party application. You only need to generate an access token and send the access token to the third-party application. You can customize the access permissions and the validity of this token.
    -   You do not need to revoke permissions. The access token becomes invalid automatically when it expires.
    For example, the interaction process in an application is shown in the following figure:

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/22518/155384392113692_en-US.png)

    The solution is as follows:

    1.  Log on to the application. You can manage your application ID. You can customize the ID management system or use an external web account or OpenID. For each valid app user, the AppServer can precisely define the minimum access permission.
    2.  The AppServer requests a security token \(SecurityToken\) from the STS. Before the STS is called, the AppServer needs to determine the minimum access permission \(in the policy syntax\) of the application user and the expiration time of the authorization. Then, you can obtain the security token by calling the STS AssumeRole API. For more information about the roles and usage, see [Role Management](../../../../../reseller.en-US/User Guide/Identities/Roles.md#) in the RAM User Guide.
    3.  The STS returns a valid access credential \(FederationToken in the application\) to the AppServer. The access credential contains a SecurityToken, a temporary pair of access keys \(an AccessKeyID and an AccessKeySecret\), and the expiration time.
    4.  The AppServer returns the FederationToken to the ClientApp. The ClientApp can cache this credential. When the credential becomes invalid, the ClientApp needs to request a new valid access credential from the AppServer. For example, if the access credential is valid for one hour, the ClientApp can request the AppServer to update the access credential every 30 minutes.
    5.  The ClientApp uses the locally cached FederationToken to request Alibaba Cloud service APIs. The cloud service detects the STS access credential, verifies the credential with the STS service, and responds to the user requests accordingly.
    For more information about STS security tokens, see [Role Management](../../../../../reseller.en-US/User Guide/Identities/Roles.md#) in the RAM User Guide. The key is to call the STS’s [AssumeRole](../../../../../reseller.en-US/API Reference (STS)/Operation interfaces/AssumeRole.md#) API to obtain the valid access credential. You can also directly use STS SDKs to call this method. [Click to view details](../../../../../reseller.en-US/SDK Reference/SDK Reference - STS/Java SDK/Preface.md#).

    To use this authentication mode, you must first activate the Alibaba Cloud RAM service.

    STS User Manual: [Click to view details](../../../../../reseller.en-US/SDK Reference/SDK Reference - STS/Java SDK/Preface.md#).

    OSS authentication policy configuration: [Click to view details](../../../../../reseller.en-US/Developer Guide/Access and control/Overview.md#).

-   Set the StsToken directly

    You can get a pair of the StsToken in the application by a certain method \(such as network requests from your business server\) and then use it to initialize the SDK. If this method is adopted, you must follow the expiration time of the STSToken. When the STSToken is about to expire, you must take the initiative to update the new STSToken for the SDK.

    The initialization code is as follows:

    ```language-java
    String endpoint = "http://oss-cn-hangzhou.aliyuncs.com";
    
    OSSCredentialProvider credentialProvider = new OSSStsTokenCredentialProvider("<StsToken.AccessKeyId>", "<StsToken.SecretKeyId>", "<StsToken.SecurityToken>");
    
    OSS oss = new OSSClient(getApplicationContext(), endpoint, credentialProvider);
    
    ```

    When you notice that the token is about to expire, you can rebuild a new OSSClient or update the CredentialProvider by the following method:

    ```language-java
    oss.updateCredentialProvider(new OSSStsTokenCredentialProvider("<StsToken.AccessKeyId>", "<StsToken.SecretKeyId>", "<StsToken.SecurityToken>"));
    
    ```

-   Implement the callback to get the StsToken

    If you want the SDK to automatically help you manage Token updates, you must configure the SDK to get the token. In the application of the SDK, you must implement a callback, which gets a FederationToken \(STSToken\) and returns the token. The SDK uses the token for signing. When the token needs to be updated, the SDK calls the callback to get the token, as shown in the following figure:

    ```language-java
    String endpoint = "http://oss-cn-hangzhou.aliyuncs.com";
    
    OSSCredentialProvider credentialProvider = new OSSFederationCredentialProvider() {
    	
        @Override
    	public OSSFederationToken getFederationToken() {
    		// Here you must implement a method to get a FederationToken and construct it into the OSSFederationToken object to return it.
        	// If the token is not obtained, "nil" is returned.
    
        	OSSFederationToken * token;
        	// The following are some codes that can be used to obtain the token, for example from your server.
        	...
        	return token;
    	}
    };
    
    OSS oss = new OSSClient(getApplicationContext(), endpoint, credentialProvider);
    
    ```

    **Note:** Additionally, if you have obtained all the fields required by the token by other means, you can use the callback to directly return the token. In this case, you must manually update the token, and then reset the OSSCredentialProvider of the OSSClient instance.

    Example:

    Suppose the URL of the server you set up is http://localhost:8080/distribute-token.json. If you access the URL, the returned data is shown as follows:

    ```
    {
    	"StatusCode": 200,
    	"AccessKeyId":"STS.iA645eTOXEqP3cg3VeHf",
    	"AccessKeySecret":"rV3VQrpFQ4BsyHSAvi5NVLpPIVffDJv4LojUBZCf",
    	"Expiration":"2015-11-03T09:52:59Z",
    	"SecurityToken":"CAES7QIIARKAAZPlqaN9ILiQZPS+JDkS/GSZN45RLx4YS/p3OgaUC+oJl3XSlbJ7StKpQ...."}
    
    ```

    You can use the following codes to implement the `OSSFederationCredentialProvider` instance:

    ```language-Java
    OSSCredentialProvider credetialProvider = new OSSFederationCredentialProvider() {
    	@Override
    	public OSSFederationToken getFederationToken() {
    		try {
    			URL stsUrl = new URL("http://localhost:8080/distribute-token.json");
    			HttpURLConnection conn = (HttpURLConnection) stsUrl.openConnection();
    			InputStream input = conn.getInputStream();
    			String jsonText = IOUtils.readStreamAsString(input, OSSConstants.DEFAULT_CHARSET_NAME);
    			JSONObject jsonObjs = new JSONObject(jsonText);
    			String ak = jsonObjs.getString("AccessKeyId");
    			String sk = jsonObjs.getString("AccessKeySecret");
    			String token = jsonObjs.getString("SecurityToken");
    			String expiration = jsonObjs.getString("Expiration");
    			return new OSSFederationToken(ak, sk, token, expiration);
    		} catch (Exception e) {
    			e.printStackTrace();
    		}
    		return null;
    	}
    };
    
    ```

-   Self-signed mode

    You can store the AccessKeyID and AccessKeySecret to your business server and then obtain them by implementing the callback in the SDK. Post the merged signature string to be signed to the server, sign the string using the OSS-specified signature algorithm on the business server, and transfer the signed string to the callback, which returns the string.

    Signature algorithm reference: [Click to view details](../../../../../reseller.en-US/API Reference/Access control/Add a signature to the header.md#).

    The content is a string merged according to various request parameters. The algorithm is:

    ```
    signature = "OSS " + AccessKeyId + ":" + base64(hmac-sha1(AccessKeySecret, content))
    
    ```

    The code is as follows:

    ```language-java
    String endpoint = "http://oss-cn-hangzhou.aliyuncs.com";
    
    credentialProvider = new OSSCustomSignerCredentialProvider() {
    	@Override
    	public String signContent(String content) {
    		// Here you must sign a string using the OSS-specified signature algorithm, merge the AccessKeyID with the signed string, and return the string.
        	// Generally, post the string to your business server and then return the signature.
        	// If the signing fails, "nil" is returned after the error is described.
        	
        	// The following is a demo using a local algorithm for signing.
        	return "OSS " + AccessKeyId + ":" + base64(hmac-sha1(AccessKeySecret, content));
    	}
    };
    
    OSS oss = new OSSClient(getApplicationContext(), endpoint, credentialProvider);
    
    ```

    **Note:** For the STS authentication mode and self-signed mode, make sure the results are returned when you call the implemented callback function. Therefore, if you must obtain the token and signature from the business server with a network request, we recommend that you call the synchronous APIs of the network library. The callback is run in the subthread of the request from the SDKs, which does not block the main thread.


