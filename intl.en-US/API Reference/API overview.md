# API overview {#reference_wrz_l2q_tdb .reference}

OSS provides the following APIs:

## Service-related operations {#section_lmg_t2q_tdb .section}

|API|Description|
|---|-----------|
|[GetService](intl.en-US/API Reference/Service operations/GetService (ListBuckets).md#)|Obtains all buckets owned by a specified account.|

## Bucket-related operations {#section_oqw_hb2_xdb .section}

|API|Description|
|:--|:----------|
|[PutBucket](intl.en-US/API Reference/Bucket operations/PutBucket.md#)|Creates a bucket.|
|[PutBucketACL](intl.en-US/API Reference/Bucket operations/PutBucketACL.md#)|Sets the ACL for a bucket.|
|[PutBucketLogging](intl.en-US/API Reference/Bucket operations/PutBucketLogging.md#)|Enables the logging function for a bucket.|
|[PutBucketWebsite](intl.en-US/API Reference/Bucket operations/PutBucketWebsite.md#)|Sets a bucket to static website hosting mode.|
|[PutBucketReferer](intl.en-US/API Reference/Bucket operations/PutBucketReferer.md#)|Configures anti-leech rules for a bucket.|
|[PutBucketLifecycle](intl.en-US/API Reference/Bucket operations/PutBucketLifecycle.md#)|Configures lifecycle rules for the objects in a bucket.|
|[GetBucket \(ListObject\)](intl.en-US/API Reference/Bucket operations/GetBucket (ListObject).md#)|Gets the information about all objects in a bucket.|
|[GetBucketAcl](intl.en-US/API Reference/Bucket operations/GetBucketAcl.md#)|Gets the ACL for a bucket.|
|[GetBucketLocation](intl.en-US/API Reference/Bucket operations/GetBucketLocation.md#)|Gets the location information about the data center to which a bucket belongs.|
|[GetBucketInfo](intl.en-US/API Reference/Bucket operations/GetBucketInfo.md#)|Obtains the information about a bucket.|
|[GetBucketLogging](intl.en-US/API Reference/Bucket operations/GetBucketLogging.md#)|Views the configuration of the logging function for a bucket.|
|[GetBucketWebsite](intl.en-US/API Reference/Bucket operations/GetBucketWebsite.md#)|Views the static website hosting status of a bucket.|
|[GetBucketReferer](intl.en-US/API Reference/Bucket operations/GetBucketReferer.md#)|Views the anti-leech rules for a bucket.|
|[GetBucketLifecycle](intl.en-US/API Reference/Bucket operations/GetBucketLifecycle.md#)|Views the lifecycle rules for the objects in a bucket.|
|[DeleteBucket](intl.en-US/API Reference/Bucket operations/DeleteBucket.md#)|Deletes a bucket.|
|[DeleteBucketLogging](intl.en-US/API Reference/Bucket operations/DeleteBucketLogging.md#)|Disables the logging function for a bucket.|
|[DeleteBucketWebsite](intl.en-US/API Reference/Bucket operations/DeleteBucketWebsite.md#)|Disables the static website hosting mode for a bucket.|
|[DeleteBucketLifecycle](intl.en-US/API Reference/Bucket operations/DeleteBucketLifecycle.md#)|Deletes the lifecycle rules for the objects in a bucket.|

## Object-related operations {#section_tq2_kb2_xdb .section}

|API|Description|
|:--|:----------|
|[PutObject](intl.en-US/API Reference/Object operations/PutObject.md#)|Uploads an object|
|[CopyObject](intl.en-US/API Reference/Object operations/CopyObject.md#)|Copies an object to another object.|
|[GetObject](intl.en-US/API Reference/Object operations/GetObject.md#)|Gets an object.|
|[AppendObject](intl.en-US/API Reference/Object operations/AppendObject.md#)|Appends the upload data to the end of an object.|
|[DeleteObject](intl.en-US/API Reference/Object operations/DeleteObject.md#)|Deletes an object|
|[DeleteMultiple Objects](intl.en-US/API Reference/Object operations/DeleteMultipleObjects.md#)|Deletes multiple objects.|
|[HeadObject](intl.en-US/API Reference/Object operations/HeadObject.md#)|Returns only the metadata of an object but not the object content.|
|[GetObjectMeta](intl.en-US/API Reference/Object operations/GetObjectMeta.md#)|Returns the metadata of an object, including the ETag, Size \(object size\), and LastModified and does not return the object content.|
|[PostObject](intl.en-US/API Reference/Object operations/PostObject.md#)|Uploads an object in Post mode.|
|[PutObjectACL](intl.en-US/API Reference/Object operations/PutObjectACL.md#)|Sets the ACL for an object.|
|[GetObjectACL](intl.en-US/API Reference/Object operations/GetObjectACL.md#)|Gets the ACL for an object.|
|[Callback](intl.en-US/API Reference/Object operations/Callback.md#)|Enables the callback function.|
|[PutSymlink](intl.en-US/API Reference/Object operations/PutSymlink.md#)|Creates a symbol link.|
|[GetSymlink](intl.en-US/API Reference/Object operations/GetSymlink.md#)|Obtains a symbol link.|
|[RestoreObject](intl.en-US/API Reference/Object operations/RestoreObject.md#)|Restores an object.|
|[SelectObject](intl.en-US/API Reference/Object operations/SelectObject.md#)|Queries objects using SQL statements.|

## Operations related to multipart upload {#section_my2_rb2_xdb .section}

|API|Description|
|:--|:----------|
|[InitiateMultipartUpload](intl.en-US/API Reference/Multipart upload operations/InitiateMultipartUpload.md#)|Initializes a MultipartUpload event.|
|[UploadPart](intl.en-US/API Reference/Multipart upload operations/UploadPart.md#)|Uploads an object in multiple parts.|
|[UploadPartCopy](intl.en-US/API Reference/Multipart upload operations/UploadPartCopy.md#)|Uploads and copies an object in multiple parts.|
|[CompleteMultipartUpload](intl.en-US/API Reference/Multipart upload operations/CompleteMultipartUpload.md#)|Complete the MultipartUpload event for an object.|
|[AbortMultipartUpload](intl.en-US/API Reference/Multipart upload operations/AbortMultipartUpload.md#)|Cancels a MultipartUpload event.|
|[ListMultipartUploads](intl.en-US/API Reference/Multipart upload operations/ListMultipartUploads.md#)|Lists all ongoing MultipartUpload events.|
|[ListParts](intl.en-US/API Reference/Multipart upload operations/ListParts.md#)|Lists all parts successfully uploaded in a MultipartUpload event with a specified upload ID.|

## Cross-Origin Resource Sharing \(CORS\) {#section_xhx_sb2_xdb .section}

|API|Description|
|:--|:----------|
|[PutBucketcors](intl.en-US/API Reference/Cross-Origin Resource Sharing/PutBucketcors.md#)|Sets a CORS rule for a specified bucket.|
|[GetBucketcors](intl.en-US/API Reference/Cross-Origin Resource Sharing/GetBucketcors.md#)|Gets the current CORS rules for a specified bucket.|
|[DeleteBucketcors](intl.en-US/API Reference/Cross-Origin Resource Sharing/DeleteBucketcors.md#)|Disables the CORS function for a specified bucket and clears all the CORS rules.|
|[OptionObject](intl.en-US/API Reference/Cross-Origin Resource Sharing/OptionObject.md#)|Specifies the preflight request for cross-region access.|

## Operations related to LiveChannel {#section_lgb_1lp_fgb .section}

|API|Description|
|---|-----------|
|[PutLiveChannelStatus](intl.en-US/API Reference/LiveChannel-related operations/PutLiveChannelStatus.md#)|Switches the status of LiveChannel.|
|[PutLiveChannel](intl.en-US/API Reference/LiveChannel-related operations/PutLiveChannel.md#)|Creates a LiveChannel.|
|[GetVodPlaylist](intl.en-US/API Reference/LiveChannel-related operations/GetVodPlaylist.md#)|Gets the specified playlist.|
|[PostVodPlaylist](intl.en-US/API Reference/LiveChannel-related operations/PostVodPlaylist.md#)|Generates a playlist.|
|[GetLiveChannelStat](intl.en-US/API Reference/LiveChannel-related operations/GetLiveChannelStat.md#)|Gets the stream pushing status of a LiveChannel.|
|[GetLiveChannelInfo](intl.en-US/API Reference/LiveChannel-related operations/GetLiveChannelInfo.md#)|Gets the configurations of a LiveChannel.|
|[GetLiveChannelHistory](intl.en-US/API Reference/LiveChannel-related operations/GetLiveChannelHistory.md#)|Gets the stream pushing record of a LiveChannel.|
|[ListLiveChannel](intl.en-US/API Reference/LiveChannel-related operations/ListLiveChannel.md#)|Lists LiveChannels.|
|[DeleteLiveChannel](intl.en-US/API Reference/LiveChannel-related operations/DeleteLiveChannel.md#)|Deletes a LiveChannel.|

