# Simple download {#concept_irq_zzb_5db .concept}

A simple download occurs when a user downloads an uploaded file \(object\). The object download is accomplished through an HTTP GET request. For the rules of generating object URLs, see [Accessing OSS](reseller.en-US/Developer Guide/Access and control/ACL verification.md#). For the access to an object by a user-defined domain name, see [Accessing OSS with User-defined Domain Names](reseller.en-US/Developer Guide/Access and control/Bind a custom domain name.md#).

A user may access a certain object in two conditions:

-   This object does not have anonymous read permission, but the user has a corresponding AccessKey, which can be used to sign the GET request and access the object.
-   This object has anonymous read permission, so all users can directly access the object through GET requests.

For more information about object and bucket access permission control, see [Access control](reseller.en-US/Developer Guide/Access and control/Access control.md#).

To authorize a third-party user to download an object from a private bucket, see [Authorized third-party download](reseller.en-US/Developer Guide/Download files/Authorized third-party download.md#).

To use multipart download, see [Multipart download](reseller.en-US/Developer Guide/Download files/Multipart download.md#).

## Reference {#section_m2t_b1c_5db .section}

-   API: [GetObject](../../../../reseller.en-US/API Reference/Object operations/GetObject.md#)
-   SDK: Java SDK-[Object](https://partners-intl.aliyun.com/help/doc-detail/32014.htm)
-   Console: [Get object URL](../../../../reseller.en-US/Console User Guide/Manage objects/Download an object.md#)

## Best practices {#section_ynv_c1c_5db .section}

-   [RAM and STS User Guide](../../../../reseller.en-US/Best Practices/Access control/Overview.md#)

