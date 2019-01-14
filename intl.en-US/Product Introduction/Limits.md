# Limits {#concept_pzk_crg_tdb .concept}

OSS has the following restrictions for use:

|Restricted item|Description|
|:--------------|:----------|
|Archive storage|It takes about one minute to restore data from the frozen state to the readable state.|
|Bucket| -   You can create a maximum of 30 buckets in a region.
-   The name, region, and storage class of a bucket cannot be modified.
-   The capacity of each bucket is unlimited.

 |
|File upload and download| -   The size of each file uploaded by console upload, [simple upload](../../../../../reseller.en-US/Developer Guide/Upload files/Simple upload.md#), [form upload](../../../../../reseller.en-US/Developer Guide/Upload files/Form upload.md#), and [append upload](../../../../../reseller.en-US/Developer Guide/Upload files/Append object.md#) cannot be greater than 5 GB. To upload a file greater than 5 GB, you must use [multipart upload](../../../../../reseller.en-US/Developer Guide/Upload files/Multipart upload.md#).
-   The size of each file uploaded by [multipart upload](../../../../../reseller.en-US/Developer Guide/Upload files/Multipart upload.md#) cannot be greater than 48.8 TB.
-   The default bandwidth throttling of upload and download is 10 Gbit/s in Mainland China regions and 5 Gbit/s for International regions, Hong Kong, Macau, and Taiwan. Once the throttling is reached, the DownloadTrafficRateLimitExceeded or UploadTrafficRateLimitExceeded error response is returned. If you need a higher bandwidth, contact your local technical support.
-   If you upload a file with same name as an existing file, the original file is overwritten.

 |
|Deleting a file| -   Deleted files cannot be restored.
-   You can delete up to 1,000 files in batches in the console. To delete more files in batches, you must use APIs or SDKs.

 |
|Domain name binding| -   You must apply for an ICP license for your bound domain name to direct your website to servers located in Mainland China for public visits.
-   You can bind up to 100 domain names for each bucket.

 |
|Lifecycle|You can configure up to 1,000 lifecycle rules for each bucket.|
|Image processing|For the original image: -   Only jpg, png, bmp, gif, webp, and tiff formats are supported.
-   File size cannot exceed 20 MB.
-   For the image rotation, the width or height of the image cannot exceed 4096.
-   The size of a single side cannot exceed 30,000.

For a thumbnail:-   The product of the width and height cannot exceed 4096 x 4096.
-   The length of each side cannot exceed 4096.

|

