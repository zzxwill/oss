# OSS request process {#concept_xcz_ff5_tdb .concept}

Based on whether the authentication information is included, HTTP requests sent to OSS are divided into two types: requests with authentication information and anonymous requests without authentication information. An anonymous request does not include authentication information. In contrast, a request with authentication information includes signature information in the request header or request URL, which is in compliance with the OSS API documents.

## Access to OSS using anonymous requests {#section_dk1_gx2_2gb .section}

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4345/1549849754959_en-US.png)

1.  A user request is sent to the HTTP server of OSS.
2.  OSS parses the URL to obtain the target bucket and object.
3.  OSS checks whether an ACL is set for the object.
    -   If no ACL is set for the object, the process proceeds to step 4.
    -   If an ACL is set for the object, OSS checks whether the ACL allows anonymous access.
        -   If the ACL allows anonymous access, the process proceeds to step 5.
        -   If the ACL does not allow anonymous access, the request is rejected and the process ends.
4.  OSS checks whether the bucket ACL allows anonymous access.
    -   If the ACL allows anonymous access, the process proceeds to step 5.
    -   If the ACL does not allow anonymous access, the request is rejected and the process ends.
5.  The request passes the authentication, and the object content is returned to the user.

## Access to OSS using requests with authentication information {#section_ax4_3x2_2gb .section}

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4345/15498497541026_en-US.png)

1.  A user request is sent to the HTTP server of OSS.
2.  OSS parses the URL to obtain the target bucket and object.
3.  OSS obtains the identity information about the requester for authentication based on the AccessKeyId of the request.
    -   If the identity information is not obtained, the request is rejected and the process ends.
    -   If the identity information is obtained, but the requester is not allowed to access the resource, the request is rejected and the process ends.
    -   If the identity information is obtained, but the signature calculated based on the HTTP parameters in the request does not match the signature contained in the request, the request is rejected and the process ends.
    -   If the authentication succeeds, the process proceeds to step 4.
4.  OSS checks whether an ACL is set for the object.
    -   If no ACL is set for the object, the process proceeds to step 5.
    -   If an ACL is set for the object, OSS checks whether the object ACL allows access by the user.
        -   If the ACL allows access by the user, the process proceeds to step 6.
        -   If the ACL does not allow the access, the request is rejected and the process ends.
5.  OSS checks whether the bucket ACL allows access by the user.
    -   If the ACL allows access by the user, the process proceeds to step 6.
    -   If the ACL does not allow access by the user, the request is rejected and the process ends.
6.  The request passes the authentication, and the object content is returned to the user.

## AccessKey types {#section_en5_23v_tdb .section}

Currently, the following three types of [AccessKeys](reseller.en-US/Developer Guide/Basic concepts.md#section_u3j_nmt_tdb) \(AKs\) are used to access OSS:

-   AK of an Alibaba Cloud account

    An AK of an Alibaba Cloud account indicates the AK of the bucket owner. The AK of an Alibaba Cloud has full access to all resources under the corresponding account. Each Alibaba Cloud account can have a maximum of five AK pairs \(AccessKeyId and AccessKeySecret\) and the AKs can be in either an active or inactive state.

    You can log on to the [AccessKey console](https://partners-intl.console.aliyun.com/#/ak) and add or delete AK pairs.

    An AK pair can be in two states: active and inactive.

    -   An AK in the active state can be used for authentication.
    -   An AK in the inactive state cannot be used for authentication.
    **Note:** For security reasons, avoid using the AK of your Alibaba Cloud account.

-   AK of a RAM user

    Resource Access Management \(RAM\) is a resource access control service provided by Alibaba Cloud. AKs of RAM users are authorized by the corresponding Alibaba Cloud account through RAM. These AKs can be used only to access OSS resources in buckets in accordance with the rules defined in RAM. By configuring RAM policies, you can manage multiple users in a centralized manner and control the resources that can be accessed by the users. For example, you can control the permission of a user so that the user can only read a specified bucket. A RAM user is subjected to the Alibaba Cloud account under which it was created, and does not own any actual resources. That is, all resources belong to the corresponding Alibaba Cloud account.

-   AK of an STS account

    Security Token Service \(STS\) is an Alibaba Cloud service that provides temporary access credentials. AKs of STS accounts are authorized by STS. These AKs can be used only to access OSS resources in buckets in accordance with the rules defined in STS.


## Authentication implementation {#section_qbb_l1f_2gb .section}

Authentication is implemented in the following three methods:

-   AK authentication
-   RAM authentication
-   STS authentication

When a user sends a request to OSS as an individual identity, authentication is performed on the user as follows:

1.  The user generates a signature string based on the request in the format specified by OSS.
2.  The user uses the AccessKeySecret to encrypt the signature string and generate a verification code.
3.  After receiving the request, OSS locates the corresponding AccessKeySecret based on the AccessKeyId, and obtains the signature string and verification code using the same method.
    -   If the calculated verification code is the same as the provided verification code, OSS determines that the request is valid.
    -   If the obtained verification code is different from the provided verification code, OSS rejects the request and returns an HTTP 403 error.

## Three methods of accessing OSS with authentication {#section_bpf_5qg_l2b .section}

-   Access OSS in the console: The authentication process is invisible to users, which means users do not need to worry about authentication configurations when they access OSS in the console. For more information, see [Download an object](../../../../../reseller.en-US/Console User Guide/Manage objects/Download an object.md#).
-   Access OSS using SDKs: OSS provides SDKs for multiple development languages, in which the signature algorithm is implemented. Therefore, users only need to input the AK information to access OSS using SDKs. For more information, see the **access control** part in the SDK documents for different development languages, such as [Java SDK: Authorized access](../../../../../reseller.en-US/SDK Reference/Java/Authorized access.md#ul_nh5_wbd_kfb) and [Python SDK: Authorized access](../../../../../reseller.en-US/SDK Reference/Python/Authorized access.md#section_zx1_55k_kfb).
-   Access OSS using APIs: To write code to package a call to the RESTful API, you must implement a signature algorithm to calculate the signature. For more information, see [Add a signature to the header](../../../../../reseller.en-US/API Reference/Access control/Add a signature to the header.md#) and [Add a signature to a URL](../../../../../reseller.en-US/API Reference/Access control/Add a signature to a URL.md#).

