# ACL verification {#concept_xcz_ff5_tdb .concept}

## Security of OSS access {#section_n2b_hf5_tdb .section}

Based on whether the identity authentication information is included or not, HTTP requests sent to OSS are divided into two types: requests with identity authentication information and anonymous requests without identity authentication information. The identity authentication information in requests comes in two forms:

-   Authorization contained in the request header, in the format of OSS + AccessKeyId + signature string
-   OSS AccessKeyId and a signature contained in the request URL.

## OSS access process {#section_f4l_3f5_tdb .section}

**Access process for anonymous requests**

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4345/959_en-US.png)

1.  The user’s request is sent to the HTTP server of OSS.
2.  OSS parses the URL to get the bucket and object information.
3.  OSS checks whether the object has ACL settings.
    -   If no ACL is set for the object, go to step 4.
    -   If an ACL is set for the object, OSS checks whether the ACL permits anonymous access.
        -   If the ACL permits anonymous access, go to step 5.
        -   If not, the request is rejected and the process ends.
4.  OSS checks whether the bucket ACL permits anonymous access.
    -   If the ACL permits anonymous access, go to step 5.
    -   If not, the request is rejected and the process ends.
5.  The request passes the access verification, and the object content is returned to the user.

**Access process for requests with identity authentication information**

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4345/1026_en-US.png)

1.  The user’s request is sent to the HTTP server of OSS.
2.  OSS parses the URL to get the bucket and object information.
3.  Based on the AccessKeyId of the request, OSS obtains the identity information of the request sender to perform authentication.
    -   If the information is not obtained, the request is rejected and the process ends.
    -   If the information is obtained, but the request sender is not permitted to access the resource, the request is rejected and the process ends.
    -   If the information is obtained, but the signature calculated based on the request HTTP parameters does not match the signature contained in the request, the request is rejected and the process ends.
    -   If the authentication succeeds, go to step 4.
4.  OSS checks whether the object has ACL settings.
    -   If no ACL is set for the object, go to step 5.
    -   If an ACL is set for the object, OSS checks whether the object ACL permits the user’s access.
        -   If the ACL permits the user’s access, go to step 6.
        -   If not, the request is rejected and the process ends.
5.  OSS checks whether the bucket ACL permits the user’s access.
    -   If the ACL permits the user’s access, go to step 6.
    -   If not, the request is rejected and the process ends.
6.  The request passes the access verification, and the object content is returned to the user.

## Three methods for OSS access with identity authentication information {#section_frb_4rg_l2b .section}

-   Access OSS by using the console: The identity authentication process is invisible to users in the console. When users access OSS through the console, they do not have to concern about the details of this process.

-   Access OSS by using SDKs: OSS provides SDKs for multiple programming languages. A signature algorithm is implemented in an SDK, where users only need to input the AK information.

-   Access OSS by using APIs: If you want to write your own code to call the RESTful APIs, you must implement a signature algorithm to calculate the signature.  For more information about the signature algorithm, see [Add a signature to the header](../../../../intl.en-US/API Reference/Access control/Add a signature to the header.md#) and [Add a signature to the URL](../../../../intl.en-US/API Reference/Access control/Add a signature to a URL.md#) in the *OSS API Reference*.


For more information about AccessKey and identity authentication, see [Access control](intl.en-US/Developer Guide/Access and control/Access control.md#).

