# Object tagging {#concept_zxf_jpy_pgb .concept}

You can use the object tagging function to classify OSS objects. You can set lifecycle rules and RAM policies for objects with the same tag in batches.

**Note:** The object tagging function is in the beta testing phase. You can [open a ticket](https://selfservice.console.aliyun.com/ticket/createIndex) to apply for the trial of this function.

The object tagging function uses a key-value pair to tag an object. You can add a tag to an object when uploading the object or add a tag to an existing object.

-   You can add up to 10 tags with different keys to an object.
-   The maximum length of a key and a value is 128 bytes and 256 bytes individually.
-   The key and value are both case-sensitive.
-   A tag can contain letters, numbers, spaces, and the following symbols: plus sign \(+\), hyphen \(-\), equal sign \(=\), period \(.\), underscore \(\_\), colon \(:\), and forward slash \(/\).
-   Only the owner of a bucket and authorized users can read and write the tags of an object in the bucket. The read and write permissions on the tags of an object are not restricted by the ACL for the object.
-   When you copy an object to another region, the tags of the object is also copied to the region.

## Application scenarios {#section_sfw_lxy_pgb .section}

The object tagging function is not limited by folders. You can select all objects with the same tag in a bucket and perform the following operations on them in batches.

-   Set lifecycle rules for objects with the specified tag. For example, you can add tags to objects that are generated periodically and do not need to be stored for a long period, and then set a lifecycle rule for the objects with the tags to delete them periodically.
-   Set a RAM policy to allow users to access objects with specified tags.

## Usage {#section_utm_dxd_qgb .section}

-   APIs that may be used by the object tagging function are listed as follows:
    -   [PutObjectTagging](../../../../reseller.en-US/API Reference/Object operations/PutObjectTagging.md#): Adds a tag to an object. If the target object already has a tag, the original tag is modified to the new tag.
    -   [GetObjectTagging](../../../../reseller.en-US/API Reference/Object operations/GetObjectTagging.md#): Reads the tags of an object.
    -   [DeleteObjectTagging](../../../../reseller.en-US/API Reference/Object operations/DeleteObjectTagging.md#): Deletes the tags of an object.
    -   [PutObject](../../../../reseller.en-US/API Reference/Object operations/PutObject.md#): When using PutObject to upload an object, you can specify the `x‑oss‑tagging` request header to add tags to the object.
    -   [InitiateMultipartUpload](../../../../reseller.en-US/API Reference/Multipart upload operations/InitiateMultipartUpload.md#): When initiating a multipart upload task, you can specify the `x‑oss‑tagging` request header to add tags to the object.
    -   [CopyObject](../../../../reseller.en-US/API Reference/Object operations/CopyObject.md#): When copying an object, you can specify the `x‑oss‑tagging‑directive` request header to copy the tags of the original object and specify the `x‑oss‑tagging` request header to specify the tags.
    -   [GetObject](../../../../reseller.en-US/API Reference/Object operations/GetObject.md#): If the user has the permission to read the tags of the target object, the `x‑oss‑tagging‑count` header is included in the response to the GetObject request to indicate the number of tags added to the target object.
    -   [HeadObject](../../../../reseller.en-US/API Reference/Object operations/HeadObject.md#): If the user has the permission to read the tags of the target object, the `x‑oss‑tagging‑count` header is included in the response to the HeadObject request to indicate the number of tags added to the target object.
-   Required permission

    The permissions required by APIs related to the object tagging function are listed as follows:

    -   GetObjectTagging: Requires the permission to obtain the tags of the object. With this permission, you can view the tags added to an object.
    -   PutObjectTagging: Requires the permission to configure the tags of the object. With this permission, you can configure tags for an object.
    -   DeleteObjectTagging: Requires the permission to delete the tags of the object. With this permission, you can delete the tags of an object.

## Object tagging and lifecycle rule management {#section_bmt_f4t_9p7 .section}

When setting a lifecycle rule, you can specify the condition of the rule so that it applies to objects with a specified prefix or a specified tag. You can also set the prefix and tag as the condition of a rule at the same time.

-   If you set a specified tag as the condition of a lifecycle rule, the rule applies to an object only when the key and value in the tag of the object both match the specified tag.
-   If you specify a prefix and multiple tags as the condition of a lifecycle rule, the rule applies to an object only when the prefix and tags of the object match the prefix and all tags specified in the rule.

Example:

``` {#codeblock_gwn_2ys_4e9}
<LifecycleConfiguration>
<Rule>
<ID>r1</ID>
<Prefix>rule1</Prefix>
<Tag><Key>xx</Key><Value>1</Value></Tag>
<Tag><Key>yy</Key><Value>2</Value></Tag>
<Status>Enabled</Status>
<Expiration>
<Days>30</Days>
</Expiration>
</Rule>
<Rule>
<ID>r2</ID>
<Prefix>rule2</Prefix>
<Tag><Key>xx</Key><Value>1</Value></Tag>
<Status>Enabled</Status>
<Transition>
<Days>60</Days>
<StorageClass>Archive</StorageClass>
</Transition>
</Rule>
</LifecycleConfiguration>
```

If you set the preceding lifecycle rule:

-   Objects with the rule1 prefix and "xx=1" and "yy=2" tags are deleted after 30 days.
-   The storage class of objects with the rule2 prefix and "xx=1" tag is converted to Archive after 60 days.

**Note:** For more information, see [Manage lifecycle rules](reseller.en-US/Developer Guide/Manage files/Manage lifecycle rules.md#li_o3p_ldc_e8o).

## Object tagging and RAM policies {#section_p6p_nw9_al7 .section}

-   You can authorize RAM users to allow them to view, configure, and delete object tags.

    You can authorize RAM users to allow them perform operations on all object tags or specified object tags. For example, if you specify the tag that can be operated by user A as "test=1", user A can only add the "test=1" tag to an object.

    **Note:** If you authorize a RAM user to allow the user to add a specified tag, the user can only add the specified tag to an existing object but cannot add the tag when uploading a new object.

-   You can grant the following permissions on objects with specified tags to a RAM user: read-only, read and write, and full permission.

