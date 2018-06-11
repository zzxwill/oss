# Simple download {#concept_irq_zzb_5db .concept}

A simple download occurs when a user downloads an uploaded file \(object\). The object download is accomplished through an HTTP GET request.  For the rules of generating object URLs, see [Accessing OSS](intl.en-US/Developer Guide/Access and control/ACL verification.md#). For the access to an object by a user-defined domain name, see[Accessing OSS with User-defined Domain Names](intl.en-US/Developer Guide/Access and control/Bind a custom domain name.md#).

A user may access a certain object in two conditions:

-   This object does not have anonymous read permission, but the user has a corresponding AccessKey, which can be used to sign the GET request and access the object.
-   This object has anonymous read permission, so all users can directly access the object through GET requests.

For more information about object and bucket access permission control, see [Access Control](intl.en-US/Developer Guide/Access and control/Access control.md#).

To authorize a third-party user to download an object from a private bucket, see [Authorized Third-party Download](intl.en-US/Developer Guide/Download Files/Authorized third-party download.md#).

To use multipart download, see [Multipart Download](intl.en-US/Developer Guide/Download Files/Multipart download.md#).

## Reference {#section_m2t_b1c_5db .section}

-   API：[Get Object](../intl.en-US/API Reference/Object operations/GetObject.md#)
-   SDK：Java SDK-[Object](https://www.alibabacloud.com/help/doc-detail/32014.htm)
-   Console: [Get object URL](../intl.en-US/Console User Guide/Manage objects/Get object URL.md#)

## Best practices {#section_ynv_c1c_5db .section}

-   [RAM and STS User Guide](../intl.en-US/Best Practices/Access control/Overview.md#)

## Additional links {#section_pxz_21c_5db .section}

-   [File Upload Methods](intl.en-US/Developer Guide/Upload files/Simple upload.md#)
-   [Upload Callback](intl.en-US/Developer Guide/Upload files/Upload callback.md#)
-   [Mobile Client Development and Download Scenario Introduction](intl.en-US/Developer Guide/Access OSS/OSS-based app development.md#)
-   [Cloud Processing for uploaded images](intl.en-US/Developer Guide/Image Processing.md#)
-   [Cloud Processing for  uploaded audio and video files](intl.en-US/Developer Guide/Cloud data processing.md#)
-   [Secure Download Access Control](intl.en-US/Developer Guide/Access and control/Access control.md#)
-   [Authorized Third-party Download for Download Security](intl.en-US/Developer Guide/Download Files/Authorized third-party download.md#)
-   [Copying, Deleting, and Managing Uploaded Files](intl.en-US/Developer Guide/Managing Objects/Object Meta.md#)

