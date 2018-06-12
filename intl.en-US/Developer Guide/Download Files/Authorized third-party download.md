# Authorized third-party download {#concept_xpq_p1c_5db .concept}

Use a URL signature, or provide temporary access credentials, to grant third party authorization to download objects in a private bucket. These methods are recommended as they prevent directly giving the AccessKey to users requesting download permissions, which can weaken account security.

## URL signature {#section_dkc_r1c_5db .section}

A developer can add a signature into the URL and forward this URL to a third party to authorize access.   The third-party user can then access this URL using an HTTP GET request to download the object.

-   Implementation method

    Example URL that includes a signature:

    ```
    http://<bucket>.<region>.aliyuncs.com/<object>?OSSAccessKeyId=<user access_key_id>&Expires=<unix time>&Signature=<signature_string>
    ```

    The signature in the URL must include the following three parameters:

    -   OSSAccessKeyId, which is the developer’s AccessKeyId.
    -   Expires, which is the developer’s expected URL expiration time.
    -   Signature, which is the developer’s signature string. For more information, see [Add a signature to a URL](../intl.en-US/API Reference/Access control/Add a signature to a URL.md#).

        **Note:** This link must undergo URL encoding.

-   Reference
    -   API: [Get Object](../intl.en-US/API Reference/Object operations/GetObject.md#)
    -   SDK: Java SDK-[Using URL Signature to Authorize Access](https://www.alibabacloud.com/help/doc-detail/32016.htm)
    -   Console: [Get object URL](../intl.en-US/Console User Guide/Manage objects/Get object URL.md#)

        **Note:** If the bucket permission is set to private read/write permission,  the access URL provided on the console contains a signature.


## Temporary access credentials {#section_ut1_d2c_5db .section}

Security Token Service \(STS\) can be used to provide temporary credentials to third-party users. By adding a signature in the request header, users can then access the object.  This authorization method is applicable to mobile scenario downloads.  For more information on the implementation of temporary access credentials, see [STS Java SDK](https://www.alibabacloud.com/help/doc-detail/28786.htm).

Implementation method

Third-party users send a request to the application server to obtain an AccessKeyID, AccessKeySecret, and STS Token issued by STS. Upon receipt, the AccessKeyID, AccessKeySecret, and STS Token are used as a signature to request the developer’s object resource.

## Reference {#section_i3c_p2c_5db .section}

-   API：[Temporary Access Credentials](../intl.en-US/API Reference/Access control/Temporary authorized access.md#)
-   SDK：Use STS temporary authorization in Java SDK-[Object](https://www.alibabacloud.com/help/doc-detail/32016.htm)
-   Console: [Get object URL](../intl.en-US/Console User Guide/Manage objects/Get object URL.md#)

## Best practices {#section_sdz_r2c_5db .section}

-   [RAM and STS User Guide](../intl.en-US/Best Practices/Access control/Overview.md#)

## Additional links {#section_yly_t2c_5db .section}

-   [File Upload Methods](intl.en-US/Developer Guide/Upload files/Simple upload.md#)
-   [Upload Callback](intl.en-US/Developer Guide/Upload files/Upload callback.md#)
-   [Mobile Client Development and Download Scenario Introduction](intl.en-US/Developer Guide/Access OSS/OSS-based app development.md#)
-   [Cloud Processing for Uploaded Images](intl.en-US/Developer Guide/Image Processing.md#)
-   [Cloud Processing for uploaded audio and video files](intl.en-US/Developer Guide/Cloud data processing.md#)
-   [Secure Download Access Control](intl.en-US/Developer Guide/Access and control/Access control.md#)
-   [Authorized Third-party Download for Download Security](intl.en-US/Developer Guide/Download Files/Authorized third-party download.md#)
-   [Copying, Deleting, and Managing Uploaded Files](intl.en-US/Developer Guide/Managing Objects/Object Meta.md#)

