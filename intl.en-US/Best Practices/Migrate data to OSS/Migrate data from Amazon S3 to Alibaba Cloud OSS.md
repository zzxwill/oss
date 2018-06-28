# Migrate data from Amazon S3 to Alibaba Cloud OSS {#concept_grc_5lg_vdb .concept}

OSS provides S3 API compatibility that allows seamless migration of data from Amazon S3 to Alibaba Cloud OSS. After data is migrated from Amazon S3 to OSS, you can still use S3 APIs to access OSS. You only need to configure your S3 client application as follows:

1.  Acquire the AccessKeyId and AccessKeySecret of your OSS primary account and sub-account, and configure the acquired AccessKeyID and AccessKeySecret in the client and SDK you are using.
2.  Configure the endpoint for client connection to OSS endpoint. For more information, see [Regions and endpoints](../../../../intl.en-US/Developer Guide/Regions and endpoints.md#).

## Migration procedures {#section_zq5_xlg_vdb .section}

For details about migration procedures, see [Use OssImport to migrate data](intl.en-US/Best Practices/Migrate data to OSS/Use OssImport to migrate data.md#).

## Use S3 APIs to access OSS after migration {#section_mpr_ylg_vdb .section}

Take note of the following when you use S3 APIs to access OSS after the migration from S3 to OSS.

-   Path style and virtual hosted style

    [Virtual hosted style](http://docs.aws.amazon.com/AmazonS3/latest/dev/VirtualHosting.html) supports accessing OSS by placing the bucket into the host header. For security reasons, OSS only supports virtual  hosted style access. Therefore, configurations on your client application are required after the migration from S3 to OSS. Some S3 tools use  path style access by default, which also requires proper configurations. Otherwise, OSS may report errors and prohibit access.

-   Permission definitions in OSS are not quite the same as they are in S3. You may adjust the permissions as necessary after the migration. See the following table for the main differences between the two.

    **Note:** 

    -   See [OSS access](../../../../intl.en-US/Developer Guide/Access and control/ACL verification.md#) for more information on the differences.
    -   OSS supports only three canned ACL modes in S3: private, public-read, and public-read-write.
    |Items|Amazon S3 permissions|Amazon S3|Alibaba Cloud OSS|
    |:----|:--------------------|:--------|:----------------|
    |Bucket|READ|With the List permission on the bucket|For all objects under the bucket, if no object permission is set for an object, the object is readable.|
    |WRITE| Objects in the bucket are writable or overwritable.|     -   Writable for objects not existing under the bucket. 
    -   If no object permission is set for an object existing in the bucket, the object is overwritable. 
    -   Initiate multipart upload is allowed.
 |
    |READ\_ACP|Read bucket ACLs.|Read bucket ACLs. Only the bucket owner and the authorized sub-account have the permission of reading bucket ACLs.|
    |WRITE\_ACP|Configure bucket ACLs.|Configure bucket ACLs. Only the bucket owner and the authorized sub-account have the permission of configuring bucket ACLs.|
    |Object|READ|Objects are readable.|Objects are readable.|
    |WRITE|N/A|Objects are overwritable.|
    |READ\_ACP|Read object ACLs.|Read object ACLs. Only the bucket owner and the authorized sub-account have the permission of reading object ACLs.|
    |WRITE\_ACP|Configure object ACLs.|Configure object ACLs. Only the bucket owner and the authorized sub-account have the permission of configuring object ACLs.|

-   Storage classes

    OSS supports the Standard, IA, and Archive storage classes, which correspond to STANDARD, STANDARD\_IA, and GLACIER respectively in Amazon S3.

    Different from Amazon  S3, OSS does not support specifying the storage class directly when uploading an object. The storage class of the object is determined by that of the bucket. OSS supports three bucket storage classes: Standard, IA, and Archive. You can use the lifecycle rules to automatically transition objects between storage classes. 

    To read an Archive object in OSS, restore it first by initiating a restore request. Different from S3, OSS does not allow setting the lifetime of the restored \(active\) copy. Therefore, OSS ignores the lifetime \(Days\) set in the S3 API. The restored state lasts for one day by default, and can be prolonged to seven days at most. After that, the object enters the frozen state again.

-   ETag
    -   For the object uploaded by using a PUT request, the ETag of an OSS object and that of an Amazon  S3 object differ in case sensitivity. The ETag is in upper case for an OSS object and in lower case for an S3 object. If your client has ETag validation, ignore case.

    -   For the objects uploaded by Multipart Upload, OSS takes the ETag calculation method that is different from S3.


## Compatible S3 APIs {#section_ddt_ltg_vdb .section}

-   Bucket operations:
    -   Delete Bucket
    -   Get Bucket （list objects）
    -   Get Bucket ACL
    -   Get Bucket lifecycle
    -   Get Bucket location
    -   Get bucket Logging
    -   Head Bucket
    -   Put Bucket
    -   Put Bucket ACL
    -   Put Bucket lifecycle
    -   Put Bucket logging
-   Object operations:
    -   Delete Object
    -   Delete Objects
    -   Get Object
    -   Get Object ACL
    -   Head Object
    -   Post Object
    -   Put Object
    -   Put Object Copy
    -   Put Object ACL
-   Multipart operations:
    -   Abort Multipart Upload
    -   Complete Multipart Upload
    -   Initiate Multipart Upload
    -   List Parts
    -   Upload Part
    -   Upload Part Copy

