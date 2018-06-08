# OSS permission {#concept_jfh_4s3_wdb .concept}

## OSS errors 403 {#section_llv_4s3_wdb .section}

An OSS error 403 indicates that the HTTP status code returned from OSS is 403 and that the server receives your request but rejects to provide service because you have no access permission. OSS errors 403 and causes are listed in the following table:

|Error|Message|Cause|Solution|
|:----|:------|:----|:-------|
|SignatureDoesNotMatch|ErrorCode: SignatureDoesNotMatchErrorMessage: The request signature we calculated does not match the signature you provided. Check your key and signing method.|Client and service calculated signatures do not match|[OSS 403 errors and troubleshooting](intl.en-US/Errors and Troubleshooting/OSS 403.md#)|
|Postobject|ErrorCode: AccessDeniedErrorMessage: Invalid according to Policy: Policy expired.ErrorCode: AccessDenied ErrorMessage: Invalid according to Policy: Policy Condition failed: …|Invalid policy in postobject|[PostObject](../intl.en-US/API Reference/Object operations/PostObject.md#)|
|Cors|ErrorCode: AccessForbiddenErrorMessage: CORSResponse: This CORS request is not allowed. This is usually because the evalution of Origin, request method / Access-Control-Request-Method or Access-Control-Requet-Headers are not whitelisted by the resource’s CORS spec.|CORS is not configured or is not configured incorrectly|[OSS set up cross-domain access](../intl.en-US/Console User Guide/Manage buckets/Set Cross-Origin Resource Sharing (CORS).md#)|
|Refers|ErrorCode: AccessDeniedErrorMessage: You are denied by bucket referer policy.|Check the Referer configuration for the bucket|[OSS Anti-leech](../intl.en-US/Console User Guide/Manage buckets/Set anti-leech.md#)|
|AccessDenied|See the following permissions for common errors|You have no permission.|See the following content for more information.|

Among them, the permissions issue is part of the 403 error. The error with the permission problem is `AccessDenied`. These errors are described in detail below.

## Common permissions errors {#section_ttw_rs3_wdb .section}

The privilege issue is that the current user does not have permission to specify an action. The errors returned by OSS and their causes can be found in the following table:

|SN|Error|Cause|
|:-|:----|:----|
|1|ErrorCode: AccessDeniedErrorMessage: The bucket you are attempting to access must be addressed using the specified endpoint. Please send all future requests to this endpoint.|The bucket does not match the endpoint.|
|2|ErrorCode: AccessDeniedErrorMessage: You are forbidden to list buckets.|You have no permissions for listBuckets.|
|3|ErrorCode: AccessDeniedErrorMessage: You do not have write acl permission on this object|You have no permissions for setObjectAcl.|
|4|ErrorCode: AccessDeniedErrorMessage: You do not have read acl permission on this object.|You have no permissions for getObjectAcl.|
|5|ErrorCode: AccessDeniedErrorMessage: The bucket you visit is not belong to you.|The subaccount has no permissions for bucket management like getBucketAcl, CreateBucket, deleteBucket, setBucketReferer, and getBucketReferer.|
|6|ErrorCode: AccessDeniedErrorMessage: You have no right to access this object because of bucket acl.|The subaccount/temporary account has no permissions to access the object like putObject getObject, appendObject, deleteObject, and postObject.|
|7|ErrorCode: AccessDeniedErrorMessage: Access denied by authorizer’s policy.|The temporary account has no access permissions. The authorization policy specified for assuming the role of this temporary account has no permissions.|
|8|ErrorCode: AccessDeniedErrorMessage: You have no right to access this object.|The subaccount/temporary account has no permissions for the current operation like initiateMultipartUpload.|

## Permission error troubleshooting {#section_usf_5s3_wdb .section}

Check whether the key is for the primary user, the subaccount or the temporary account.

-   Check whether the key is for a primary user.

    Log on to the [console](https://ak-console.aliyun.com) to check whether the AccessKeyID exists. If it does exist, the key is for a primary user.

-   Check the subaccount permission, that is, the authorization policy.

    In the console, navigate to**Resource Access Management** \> **User Management** \> **Management** \> **User Details** \> **User AccessKey**, view the AccessKeyID of the subaccount , and find the corresponding subaccount; In the console**Resource Access Management** \> **User Management** \> **Management** \> **User Authorization Policy** \> **Individual Authorization Policy/Authorization Policy for join Group** \> **Permission to view sub-accounts**

    .

-   Check the permissions for a temporary account.

    The AccessKeyID for the temporary account can be recognized easily since it starts with “STS”, for example, “STS.MpsSonrqGM8bGjR6CRKNMoHXe”.  Log on to the console and navigate to**Resource Access Management ** \> **Role Management** \> **Management** \> **Role Authorization Policy** \> **View Permissions**

    .


The access rights error process is shown in the following figure:

Procedures for checking the permissions:

1.  List the required permissions and resources.
2.  Check whether Action has the required operation.
3.  Check whether Resource is the required operation object.
4.  Check whether Effect is “Allow” instead of “Deny”.
5.  Check whether Condition is set correctly.

If it is unable to detect the error through checking, the following adjustments are required:

1.  The condition, if any, must be removed.
2.  Remove “Deny” in Effect.
3.  Change Resource to “Resource”: “\*”.
4.  Change Action to “Action”: “oss:\*”.

**Note:** 

-   We recommend that you the OSS authorization policy generation tool, [RAM Policy Editor](http://gosspublic.alicdn.com/ram-policy-editor/index.html?spm=a2c4g.11186623.2.11.xeubSy).
-   If you want to learn more about RAM, see [access control for Alibaba Cloud](https://yq.aliyun.com/articles/57895?spm=a2c4g.11186623.2.12.xeubSy).

