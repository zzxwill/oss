# Access control {#concept_e4s_mhv_tdb .concept}

## Send an OSS access request {#section_p4c_phv_tdb .section}

You can access OSS directly by calling a RESTful API provided by OSS or using an API-encapsulated SDK.  Each request to access OSS requires identity verification or direct anonymous access based on the current bucket permission and operation. According to the role of the visitors,

-   the access to OSS resources is classified into owner access and third-party access. Here, the owner refers to the bucket owner, also known as "developer". Third-party users are the users who access resources in a bucket.
-   According to the identity of visitors, the access to OSS resources is classified into anonymous access and signature-based access. In the OSS, a request that does not contain any identification information is considered anonymous access. According to the rules in the OSS API documentation, signature-based access refers to the requests that contain signature information in the request header or URL.

## Types of AccessKeys {#section_en5_23v_tdb .section}

Currently, the three types of [AccessKey](intl.en-US/Developer Guide/Basic concepts.md#section_u3j_nmt_tdb) for OSS access are as follows:

-   Alibaba Cloud account AccessKeys

    These are the AccessKeys of the bucket owners. The AccessKey provided by each Alibaba Cloud account has full access to its own resources. Each Alibaba Cloud account can simultaneously have 0 to 5 active or inactive AccessKey pairs \(AccessKeyID and AccessKeySecret\).

    You can add or delete AccessKey pairs on [AccessKey console](https://ak-console.aliyun.com).

    Each AccessKey pair may be in two states: active and inactive.

    -   Active indicates that the user's AccessKey is in the active state and can be used for identity authentication.
    -   Inactive indicates that the user's AccessKey is in the inactive state and cannot be used for identity authentication.
    **Note:** The AccessKey of the Alibaba Cloud account must not be used directly unless it is required.

-   RAM account AccessKeys

    Resource Access Management \(RAM\) is a resource access control service provided by Alibaba Cloud. RAM account AKs are the AccessKeys granted by RAM. These AKs only allow access to resources in a bucket according to the rules defined by RAM. RAM helps you to collectively manage your users \(such as employees, systems, or applications\) and controls which resources your users can access. For example, you can allow your users to have only the read permission on a bucket. Sub-accounts are subordinate to normal accounts and cannot own any actual resources. All resources belong to the primary accounts.

-   STS account AccessKeys

    The Alibaba Cloud STS \(Security Token Service\) is a service that provides temporary access credentials. STS account AKs are issued by the STS. These AKs only allow access to buckets in accordance with the rules defined by STS.


## Implementation of identity authentication {#section_ny4_43v_tdb .section}

The three methods of authentication are:

-   AK authentication
-   RAM authentication
-   STS authentication

Before sending a request to OSS as an individual identity,

1.  a user must generate a signature string for the request according to the format specified by OSS
2.  and then encrypt the signature string using the AccessKeySecret to generate a verification code.
3.  After receiving the request, OSS finds the corresponding AccessKeySecret based on the AccessKeyID, and obtains the signature string and verification code in the same way.
    -   If the obtained verification code is similar to the verification code provided, the request is assumed valid.
    -   If not, OSS rejects the request and returns an HTTP 403 error.

Users can directly use the SDKs provided by OSS with different AccessKeys for different types of identity authentication.

## Permission control {#section_shd_t3v_tdb .section}

OSS provides the following permission control to access its stored objects:

-   Bucket-level permissions
-   Object-level permissions
-   Account-level permissions \(RAM\)
-   Temporary account permissions \(STS\)

## Bucket-level permissions {#section_jcr_53v_tdb .section}

-   Bucket permission types

    OSS provides an Access Control List \(ACL\) for permission control. The OSS ACL provides bucket-level access control. Currently, three access permissions are provided for a bucket: public-read-write, public-read, and private. They are described as follows:

    |Permission|Permission|Access Restriction|
    |:---------|:---------|:-----------------|
    |public-read-write|Public read and write|Anyone \(including anonymous users\) can read, write, and delete the objects in the bucket. The fees incurred by such operations must be borne by the owner of the bucket. Use this permission with caution.|
    |public-read|Public read, private write|Only the owner of the bucket can write or delete the objects in the bucket. Anyone \(including anonymous users\) can read the objects in the bucket.|
    |private|Private read and write|Only the owner of the bucket can read, write, and delete the objects in the bucket. Others cannot access the objects in the bucket without authorization.|

-   Bucket permission settings and read methods

    Reference:

    -   API: [Put BucketACL](../intl.en-US/API Reference/Bucket operations/Put Bucket ACL.md#)
    -   SDK: Java SDK - [Manage a bucket](https://www.alibabacloud.com/help/doc-detail/32012.htm)
    -   Console: [Create a bucket](../intl.en-US/Console User Guide/Manage buckets/Create a bucket.md#)
    -   API: [Get BucketACL](../intl.en-US/API Reference/Bucket operations/GetBucketAcl.md#)
    -   SDK: Java SDK - [Manage a bucket](https://www.alibabacloud.com/help/doc-detail/32012.htm)

## Object-level permissions {#section_af3_cjv_tdb .section}

-   Object permission types

    OSS ACL provides object-level permission access control. Currently, four access permissions are available for an object, including private, public-read, public-read-write and default. You can use the "x-oss-object-acl" header in the Put Object ACL request to set the access permission. Only the bucket owner has the permission to perform this operation.

    |Permission|Permission|Access restriction|
    |:---------|:---------|:-----------------|
    |public-read-write|Public read and write|Indicates that the object can be read and written by the public. That is, all users have the permission to read and write the object.|
    |public-read|Public read, private write|Indicates that the object can be read by the public. Only the owner of the object has the permission to read and write the object. Other users only have the permission to read the object.|
    |private|Private read and write|Indicates that the object is a private resource. Only the owner of the object has the permission to read and write the object. Other users have no permission to operate the object.|
    |default|Default permissions|Indicates that the object inherits the permission of the bucket.|

    **Note:** 

    -   If no ACL is configured for an object, the object uses the default ACL, indicating that the object has the same ACL as the bucket where the object is stored.
    -   If an ACL is configured for an object, the object ACL has higher-level permission than the bucket ACL. For example, an object with the public-read permission can be accessed by authenticated users and anonymous users, regardless of the bucket permission.
-   Object permission settings and read methods

    Reference:

    -   API: [Put Object ACL](../intl.en-US/API Reference/Object operations/Put Object ACL.md#)
    -   SDK: [Set the object ACL](https://www.alibabacloud.com/help/doc-detail/32015.htm) in Java SDK
    -   API: [Get Object ACL](../intl.en-US/API Reference/Object operations/GetObjectACL.md#)
    -   SDK: [Get the object ACL](https://www.alibabacloud.com/help/doc-detail/32015.htm) in Java SDK

## Account-level permissions \(RAM\) {#section_mjv_sjv_tdb .section}

-   Scenario

    If you have purchased cloud resources and multiple users in your organization use them, these users have to share the AccessKey of your Alibaba Cloud account. However, the following issues may occur:

    -   If the key is shared by many people, a high risk of data leakage is involved.
    -   You cannot determine which resources \(e.g. buckets\) can be accessed by the users.
    Solution: Under your Alibaba Cloud account, you can use RAM to create subusers with their own AccessKeys. In this case, your Alibaba Cloud account is the primary account and the created accounts are the subaccounts. Subaccounts can only use their AccessKeys for the operations and resources authorized by the primary account.

-   Specific implementation

    For more information, see [Overview](https://www.alibabacloud.com/help/doc-detail/28645.htm).

    The overview section provides the relevant links to Identity management section, Authorization management section and Typical application scenarios. For more information on how to configure policies required for authorization, see the final section Configuration Rules of this document.


## Temporary account permissions \(STS\) {#section_mjv_skv_tdb .section}

-   Scenario

    Users managed by your local identity system, such as your app users, your local corporate account, or third-party apps, may also directly access OSS resources called as the federated users. Additionally, users can also be the applications you create that have access to your Alibaba Cloud resources.

    Considering the federated users, short-term access permission management is provided to the Alibaba Cloud account \(or RAM users\) through the Security Token Service  \(STS\) of Alibaba Cloud. You do not need to reveal the long-term key \(such as the logon password and AccessKey\) of your Alibaba Cloud account \(or RAM users\), but must create a short-term access credential for a federated user. The access permission and validity of this credential is limited to you. You must not care about permission revocation. The access credentials automatically becomes invalid when it expires.

    STS-based access credentials include the security token \(SecurityToken\) and the temporary AccessKey \(AccessKeyId and AccessKeySecret\). The AccessKey method is same as the method of using the AccessKey of the Alibaba Cloud account or RAM user. It is required for each OSS access request to carry a security token.

-   Specific implementation

    For more information on STS, see [Roles](https://www.alibabacloud.com/help/doc-detail/28649.htm) in the RAM User Guide. The key is to call [AssumeRole](https://www.alibabacloud.com/help/doc-detail/28763.htm) of the STS interface to obtain valid access credential. You can directly use STS SDK to call the access credential.


## RAM and STS scenario practices {#section_scy_dlv_tdb .section}

In different scenarios, how the access identity is verified may change. The following are two methods described to access identity verification in typical scenarios.

A mobile app is used as an example. Assume that you are a mobile app developer. You attempt to use the Alibaba Cloud OSS to store end user data for that app. Make sure that the data is isolated between app users to prevent an app user from obtaining data of other app users.

-   Mode 1: Using AppServer for data transit and data isolation

    ![](images/982_en-US.png)

    As shown in the preceding figure, you must develop an AppServer. Only the AppServer can access ECS. The ClientApp can read or write data only through the AppServer. The AppServer makes sure the isolated access to different user data.

    In this method, you can use the key provided by your Alibaba Cloud account or RAM account for signature verification. In case of any security issues, do not directly use the key of your Alibaba Cloud account \(root account\) to access OSS.

-   Mode 2: Using STS for direct access to OSS

    The STS solution diagram:

    ![](images/983_en-US.png)

    Procedure

    1.  Log on as the app user. The app user is irrelative to the Alibaba Cloud account but is an end user of the app. The AppServer allows the app user to log on. For each valid app user, the AppServer must define the minimum access permission.
    2.  The AppServer requests a security token \(SecurityToken\) from the STS.  Before calling STS, the AppServer needs to determine the minimum access permission \(described in policy syntax\) of app users and the expiration time of the authorization. Then, the AppServer uses AssumeRole to obtain a security token indicating a role. For more information, see Roles in the RAM User Guide.
    3.  The STS returns a valid access credential to the AppServer, where the access credential includes a security token, a temporary AccessKey \(AccessKeyID and AccessKeySecret\), and the expiry time.
    4.  The AppServer returns the access credential to the ClientApp. The ClientApp caches this credential. When the credential becomes invalid, the ClientApp must request a new valid access credential from the AppServer. For example, if the access credential is valid for one hour, the ClientApp can request the AppServer to update the access credential every 30 minutes.
    5.  The ClientApp uses the access credential cached locally to request Alibaba Cloud Service APIs. The ECS perceives the STS access credential, relies on STS to verify the credential, and correctly responds to the user request.

## RAM and STS authorization policy configuration {#section_hbs_zlv_tdb .section}

Following is the example of the RAM or STS authorization policy configuration:

-   Example

    Policy example

    ```
    
        "Version": "1",
        "Statement": [
            
                "Action": [
                    "oss:GetBucketAcl",
                    "oss:ListObjects"
                
                "Resource": [
                    "acs:oss:*:1775305056529849:mybucket"
                
                "Effect": "Allow",
                "Condition": {
                    "StringEquals": {
                        "acs:UserAgent": "java-sdk",
                        "oss:Prefix": "foo"
                    
                    "IpAddress": {
                        "acs:SourceIp": "192.168.0.1"
                    }
                
            
            
                "Action": [
                    "oss:PutObject",
                    "oss:GetObject",
                    "oss:DeleteObject"
                
                "Resource": [
                    "acs:oss:*:1775305056529849:mybucket/file*"
                
                "Effect": "Allow",
                "Condition": {
                    "IpAddress": {
                        "acs:SourceIp": "192.168.0.1"
                    
                
            
        
    
    ```

    This is the authorization policy and it can be used to grant permissions for users through RAM or STS. The policy has a Statement \(one policy can have multiple Statements\). In this Statement, Action, Resource, Effect, and Condition are specified.

    This policy authorizes your `mybucket` and `mybucket/file*` resources to corresponding users and supports GetBucketAcl, GetBucket, PutObject, GetObject, and DeleteObject actions. The Condition indicates that authentication is successful and authorized users can access related resources only when UserAgent is java-sdk and the source IP address is 192.168.0.1. The Prefix and Delimiter conditions apply during the GetBucket \(ListObjects\) action. For more information about the two fields, see OSS API Documentation.

-   Configuration rules
    -   Version

        Policy version is defined. For configuration method in this document, it is set to "1".

    -   Statement

        The Statement describes the authorization meaning. It can contain multiple meanings based on the business scenario. Each meaning contains description of the Action, Effect, Resource, and Condition. The request system checks each statement one by one, for a match. All successfully matched statements are classified into 2 categories namely Allow and Deny based on the difference of Effect settings, and Deny is given priority. If the all the matches are Allow, the request passes the authentication. If one of the matches is Deny or no matches, this request is denied access.

    -   Action

        Actions fall into three categories. Service-level action is GetService used to list all buckets belongs to the user.

        -   Bucket-level actions include oss:PutBucketAcl and oss:GetBucketLocation. The action objects are buckets and the action names correspond to the involved interfaces in a one-to-one manner.
        -   Object-level actions include oss:GetObject, oss:PutObject, oss:DeleteObject, oss:DeleteObject, and oss:AbortMultipartUpload.
        To authorize actions for a type of object, you can select one or more of the preceding actions. Additionally, all action names must be prefixed with "oss:", as mentioned in the preceding example. Action is a list and there can be multiple Actions. The mapping between Actions and APIs is as follows:

        -   Service-level

            |API|Action|
            |:--|:-----|
            |Getservice \(listbuckets\)|oss:ListBuckets|

        -   Bucket-level

            |API|Action|
            |:--|:-----|
            |PutBucket|oss:PutBucket|
            |GetBucket（ListObjects）|oss:ListObjects|
            |PutBucketAcl|oss:PutBucketAcl|
            |DeleteBucket|oss:DeleteBucket|
            |GetBucketLocation|oss:GetBucketLocation|
            |GetBucketAcl|oss:GetBucketAcl|
            |GetBucketLogging|oss:GetBucketLogging|
            |PutBucketLogging|oss:PutBucketLogging|
            |DeleteBucketLogging|oss:DeleteBucketLogging|
            |GetBucketWebsite|oss:GetBucketWebsite|
            |PutBucketWebsite|oss:PutBucketWebsite|
            |DeleteBucketWebsite|oss:DeleteBucketWebsite|
            |GetBucketReferer|oss:GetBucketReferer|
            |PutBucketReferer|oss:PutBucketReferer|
            |GetBucketLifecycle|oss:GetBucketLifecycle|
            |PutBucketLifecycle|oss:PutBucketLifecycle|
            |DeleteBucketLifecycle|oss:DeleteBucketLifecycle|
            |ListMultipartUploads|oss:ListMultipartUploads|
            |PutBucketCors|oss:PutBucketCors|
            |GetBucketCors|oss:GetBucketCors|
            |DeleteBucketCors|oss:DeleteBucketCors|
            |PutBucketReplication|oss:PutBucketReplication|
            |GetBucketReplication|oss:GetBucketReplication|
            |DeleteBucketReplication|oss:DeleteBucketReplication|
            |GetBucketReplicationLocation|oss:GetBucketReplicationLocation|
            |GetBucketReplicationProgress|oss:GetBucketReplicationProgress|

        -   Object-level

            |API|Action|
            |:--|:-----|
            |GetObject|oss:GetObject|
            |HeadObject|oss:GetObject|
            |PutObject|oss:PutObject|
            |PostObject|oss:PutObject|
            |InitiateMultipartUpload|oss:PutObject|
            |UploadPart|oss:PutObject|
            |CompleteMultipart|oss:PutObject|
            |DeleteObject|oss:DeleteObject|
            |DeleteMultipartObjects|oss:DeleteObject|
            |AbortMultipartUpload|oss:AbortMultipartUpload|
            |ListParts|oss:ListParts|
            |CopyObject|oss:GetObject,oss:PutObject|
            |UploadPartCopy|oss:GetObject,oss:PutObject|
            |AppendObject|oss:PutObject|
            |GetObjectAcl|oss:GetObjectAcl|
            |PutObjectAcl|oss:PutObjectAcl|

-   Resource

    Resource stands for a specific resource or resources on OSS \(the wildcard is supported\). Resources are named in the format of `acs:oss:{region}:{bucket_owner}:{bucket_name}/{object_name}`. For all bucket-level actions, the final part "/object\_name" is not required. You can render it as `acs:oss:{region}:{bucket_owner}:{bucket_name}`. Resource is a list and there can be multiple Resources. Here, the region field is currently not supported and set as "\*".

-   Effect

    Effect indicates the authorization result of the Statement. Two value options are available: Allow and Deny.  When multiple Statement matches are observed, then the Deny option is given priority.

    For example, deny the deletion of a certain directory, but allow all operations for other files:

    ```
    
      "Version": "1",
      "Statement": [
        
          "Effect": "Allow",
          "Action": [
            "oss:*"
          
          "Resource": [
            "acs:oss:*:*:bucketname"
          
        
        
          "Effect": "Deny",
          "Action": [
            "oss:DeleteObject"
          
          "Resource": [
            "acs:oss:*:*:bucketname/index/*",
          
        
      
    
    ```

-   Condition

    Condition indicates the conditions for the authorization policy. In the preceding example, you can set check conditions for acs:UserAgent and acs:SourceIp. The oss:Delimiter and oss:Prefix fields are used to restrict resources during the GetBucket action.

    The OSS supports the following conditions:

    |Condition|Function|Valid value|
    |:--------|:-------|:----------|
    |acs:SourceIp|Specifying the IP address segment|Common IP address, wildcard \(\*\) supported|
    |acs:UserAgent|Specifying the http useragent header|String|
    |acs:CurrentTime|Specifying valid access time|ISO8601 format|
    |acs:SecureTransport|Whether HTTPS is used|"true" or "false"|
    |oss:Prefix|Used as the prefix for ListObjects|Valid object name|


## More examples {#section_ocz_zpv_tdb .section}

For more scenarios-specific authorization policy configuration examples, see [Tutorial: control access to buckets and folders](intl.en-US/Developer Guide/Access and control/Tutorial: Control access to buckets and folders.md#) and [OSS FAQ](https://www.alibabacloud.com/help/faq-list/39711.htm).

For the online policy graphical configuration tool, click [here](http://gosspublic.alicdn.com/ram-policy-editor/index.html?spm=a2c4g.11186623.2.23.VMkTIw).

## Best practices {#section_dlf_bqv_tdb .section}

-   [RAM and STS User Guide](../intl.en-US/Best Practices/Access control/Overview.md#)

