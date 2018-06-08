# Object Meta {#concept_lkf_swy_5db .concept}

Object Meta describes the attributes of files uploaded to OSS. These attributes are classified into two types: HTTP standard attributes \(HTTP Headers\) and User Meta \(custom metadata\).  File metadata can be configured when files are uploaded or copied.

-   HTTP standard attributes

    |Name|Description|
    |:---|:----------|
    |Cache-Control|Cache action of the web page when the object is downloaded|
    |Content-Disposition| Name of the object when downloaded|
    |Content-Encoding|Content encoding format when the object is downloaded|
    |Content-Language|Specifies the content language encoding when the object is downloaded|
    |Expires|Expiry time|
    |Content-Length|Size of the object|
    |Content-Type|File type of the object|
    |Last-Modified|Time of last modification|

-   User Meta

    This attribute allows you to enrich the description of objects using custom metadata. In OSS, all parameters prefixed with “x-oss-meta-“ are considered as  User  Meta, such as x-oss-meta-location.  A single object can have multiple similar parameters, but the total size of all User Meta cannot exceed 8 KB. User   Meta information is returned in the HTTP header during GetObject or HeadObject operations.


## Set object Meta when uploading objects {#section_y5f_qxy_5db .section}

You can set object Meta when uploading objects.

Reference:

-   API：[Put Object](../intl.en-US/API Reference/Object operations/PutObject.md#)
-   SDK：Java SDK-[Set the HTTP Headers](https://www.alibabacloud.com/help/doc-detail/32013.htm) and ****User-defined meta information** **

You can set object Meta when using multipart uploads.

Reference:

-   API：[InitiateMultipartUpload](../intl.en-US/API Reference/Multipart upload operations/InitiateMultipartUpload.md#)
-   SDK：Java SDK-[Initialize Multipart Upload](https://www.alibabacloud.com/help/doc-detail/32013.htm) 中的 **Initialize Multipart  Upload**

## Modify object Meta after uploading objects {#section_xmx_sxy_5db .section}

To modify the object metadata without modifying the actual data, using the copy object interface is recommended. In this way, you only need to apply the new metadata in the HTTP header and set the copy source and destination addresses to the current address of the object.

Reference

-   API：[Copying Objects](intl.en-US/Developer Guide/Managing Objects/Copy an object.md#)
-   SDK：Java SDK-[Use CopyObjectRequest to copy objects](https://www.alibabacloud.com/help/doc-detail/32015.htm)

## Retrieve object Meta {#section_t21_5xy_5db .section}

This feature applies when the you must retrieve object Meta, but not the object data.

Reference:

-   API：[Head Object](../intl.en-US/API Reference/Object operations/HeadObject.md#)
-   SDK：Java SDK-[Retrieve Object Metadata](https://www.alibabacloud.com/help/doc-detail/32015.htm)

