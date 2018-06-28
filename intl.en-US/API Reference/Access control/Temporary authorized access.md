# Temporary authorized access {#concept_m3j_pff_xdb .concept}

## Introduction of STS {#section_r51_qff_xdb .section}

OSS can temporarily perform authorized access through the Alibaba Cloud STS \(Security Token Service\). Alibaba Cloud STS is a web service that provides a temporary access token to a cloud computing user. Using STS, you can grant access credentials to a third-party application or federated user \(you can manage the user IDs\) with customized permissions and validity periods. Third-party applications or federated users can use these access credentials to directly call the Alibaba Cloud product APIs or use the SDKs provided by Alibaba Cloud products to access the cloud product APIs.

-   You do not need to expose your long-term key \(AccessKey\) to a third-party application and only need to generate an access token and send the access token to the third-party application.

-   You can customize the access permission and validity of this token.

-   You do not need to care about permission revocation issues. The access credential automatically becomes invalid when it expires.


Using an app as an example, the interaction process is shown as follows:

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4670/3535_en-US.png)

The solution is described in detail as follows:

1.  Log on as the app user. 

    App user IDs are managed by the customer. Customers can customize the ID management system, or use an external web account or OpenID. For each valid app user, the AppServer can precisely define the minimum access permission.

2.  The AppServer requests a security token \(SecurityToken\) from the STS. 

    Before calling STS, the AppServer needs to determine the minimum access permission \(described in policy syntax\) of the app user and the expiration time of the authorization. Then the security token is obtained by calling the STS’ AssumeRole interface.

3.  The STS returns a valid access credential to the AppServer, including a security token, a temporary AccessKey \(AccessKeyID and AccessKeySecret\), and the expiry time.
4.  The AppServer returns the access credential to the ClientApp.

    The ClientApp can cache this credential. When the credential becomes invalid, the ClientApp needs to request a new valid access credential from the AppServer. For example, if the access credential is valid for one hour, the ClientApp can request the AppServer to update the access credential every 30 minutes.

5.  The ClientApp uses the access credential cached locally to request Alibaba Cloud Service APIs. The cloud services perceives the STS access credential, and relies on STS to verify the credential and correctly respond to the user request.

For more information on the STS Security token, refer to role management in the RAM usage guide.

Call [AssumeRole](https://www.alibabacloud.com/help/doc-detail/28763.htm) of the STS interface to obtain valid access credentials. You can also directly use STS SDK to call the this method. 

## Use STS credentials to construct signed requests {#section_ukd_wff_xdb .section}

After obtaining the STS temporary credential, the client of the user creates a signature using the security token \(SecurityToken\) and temporary AccessKey \(AccessKeyId and AccessKeySecret\) in the credential. The method for constructing an authorized access signature is basically the same as using the AccessKey of a root account to [add a signature to a header](intl.en-US/API Reference/Access control/Add a signature to the header.md#). Pay attention to the following two points:

-   The signature key used by the user is the temporary AccessKey \(AccessKeyId and AccessKeySecret\) provided by the STS.

-   The user needs to carry the security token \(security token\) in the request header or in the URI as a request parameter. These two manners are alternative. If both manners are selected, OSS returns an InvalidArgument error.

    -   The header `x-oss-security-token：SecurityToken` is carried in a request header. When CanonicalizedOSSHeaders of the signature is calculated, x-oss-security-token is taken into consideration.

    -   Parameter `security-token=SecurityToken` is carried in the URL. When CanonicalizedResource of the signature is calculated, security-token is taken into consideration and considered as a sub-resource.


