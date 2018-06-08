# API overview {#reference_wrz_l2q_tdb .reference}

The API interfaces provided by OSS are as follows:

## Service operations {#section_lmg_t2q_tdb .section}

|API|Description|
|---|-----------|
|[GetService](intl.en-US/API Reference/Service operations/GetService (ListBuckets).md#)|Obtain all the buckets of a specified account.|

## Bucket operations {#section_oqw_hb2_xdb .section}

|API|Description|
|:--|:----------|
|[Put Bucket](intl.en-US/API Reference/Bucket operations/PutBucket.md#)|Crate a bucket.|
|[Put Bucket ACL](intl.en-US/API Reference/Bucket operations/Put Bucket ACL.md#)|Set the bucket access permission.|
|[Put Bucket Logging](intl.en-US/API Reference/Bucket operations/PutBucketLogging.md#)|Enable logging for the bucket.|
|[Put Bucket website](intl.en-US/API Reference/Bucket operations/Putbucketwebsite.md#)|Configure static website hosting for the bucket.|
|[Put Bucket Referer](intl.en-US/API Reference/Bucket operations/Put Bucket Referer.md#)|Configure anti-leech rules for the bucket.|
|[Put Bucket Lifecycle](intl.en-US/API Reference/Bucket operations/Put Bucket Lifecycle.md#)|Configure lifecycle rules for objects in the bucket.|
|[Get Bucket ACL](intl.en-US/API Reference/Bucket operations/GetBucketAcl.md#)|Get the bucket access permission.|
|[Get Bucket location](intl.en-US/API Reference/Bucket operations/Getbucketlocation.md#)|Get the location information about the data center to which the bucket belongs.|
|[Get Bucket Logging](intl.en-US/API Reference/Bucket operations/GetBucketLogging.md#)|View the access log configuration of the bucket.|
|[Get Bucket website](intl.en-US/API Reference/Bucket operations/GetBucketWebsite.md#)|View the static website hosting status of the bucket.|
|[Get Bucket Referer](intl.en-US/API Reference/Bucket operations/Get Bucket Referer.md#)|View anti-leech rules for the bucket.|
|[Get Bucket Lifecycle](intl.en-US/API Reference/Bucket operations/Get Bucket Lifecycle.md#)|View the lifecycle rules of objects in the bucket.|
|[Delete Bucket](intl.en-US/API Reference/Bucket operations/Delete Bucket.md#)|Delete the bucket.|
|[Delete Bucket Logging](intl.en-US/API Reference/Bucket operations/DeleteBucketLogging.md#)|Disable the access logging feature of the bucket.|
|[Delete Bucket website](intl.en-US/API Reference/Bucket operations/DeleteBucketWebsite.md#)|Disable the static website hosting mode of the bucket.|
|[Delete Bucket Lifecycle](intl.en-US/API Reference/Bucket operations/DeleteBucketLifecycle.md#)|Delete the lifecycle rules of objects in the bucket.|
|[Get Bucket \(list object\)](intl.en-US/API Reference/Bucket operations/GetBucket (List Object).md#)|Get information of all the objects in the bucket.|
|[Get Bucket info](intl.en-US/API Reference/Bucket operations/GetBucketInfo.md#)|Get bucket Information|

## Object operations {#section_tq2_kb2_xdb .section}

|API|Description|
|:--|:----------|
|[Put Object](intl.en-US/API Reference/Object operations/PutObject.md#)|Upload an object.|
|[Copy Object](intl.en-US/API Reference/Object operations/CopyObject.md#)|Copy an object to make it another object.|
|[Get Object](intl.en-US/API Reference/Object operations/GetObject.md#)|Get an object.|
|[Delete Object](intl.en-US/API Reference/Object operations/DeleteObject.md#)|Delete an object.|
|[Delete Multiple Objects](intl.en-US/API Reference/Object operations/DeleteMultipleObjects.md#)|Delete multiple objects.|
|[Head Object](intl.en-US/API Reference/Object operations/HeadObject.md#)|Get the object meta information.|
|[Post Object](intl.en-US/API Reference/Object operations/PostObject.md#)|Upload an object in the Post mode.|
|[Append Object](intl.en-US/API Reference/Object operations/AppendObject.md#)|Append the upload data at the end of the object.|
|[Put Object ACL](intl.en-US/API Reference/Object operations/Put Object ACL.md#)|Set the object ACL.|
|[Get Object ACL](intl.en-US/API Reference/Object operations/GetObjectACL.md#)|Get the object ACL information.|
|[Callback](intl.en-US/API Reference/Object operations/Callback.md#)| Upload callback.|

## Multipart upload operations {#section_my2_rb2_xdb .section}

|API|Description|
|:--|:----------|
|[Initiate Multipart upload](intl.en-US/API Reference/Multipart upload operations/InitiateMultipartUpload.md#)|Initialize a multipart upload event.|
|[Upload Part](intl.en-US/API Reference/Multipart upload operations/UploadPart.md#)|Upload files in multiple parts.|
|[Upload Part Copy](intl.en-US/API Reference/Multipart upload operations/UploadPartCopy.md#)|Copy and upload files in multiple parts.|
|[Complete Multipart upload](intl.en-US/API Reference/Multipart upload operations/CompleteMultipartUpload.md#)|Complete the multipart upload of the entire file.|
|[Abort Multipart upload](intl.en-US/API Reference/Multipart upload operations/AbortMultipartUpload.md#)|Cancel a multipart upload event.|
|[List Multipart Uploads](intl.en-US/API Reference/Multipart upload operations/ListMultipartUploads.md#)|List all the ongoing multipart upload events.|
|[List Parts](intl.en-US/API Reference/Multipart upload operations/ListParts.md#)|List all successfully uploaded parts mapped to a specific upload ID.|

## Cross-Origin Resource Sharing \(CORS\) {#section_xhx_sb2_xdb .section}

|API|Description|
|:--|:----------|
|[Put Bucket cors](intl.en-US/API Reference/Cross-Origin Resource Sharing/PutBucketcors.md#)|Configure a CORS rule for a specified bucket.|
|[Get Bucket cors](intl.en-US/API Reference/Cross-Origin Resource Sharing/GetBucketcors.md#)|Get the current CORS rules of a specified bucket.|
|[Delete Bucket cors](intl.en-US/API Reference/Cross-Origin Resource Sharing/DeleteBucketcors.md#)|Disable the CORS function for a specified bucket and clear all the rules.|
|[Option Object](intl.en-US/API Reference/Cross-Origin Resource Sharing/OptionObject.md#)|Preflight request for cross-region access.|

