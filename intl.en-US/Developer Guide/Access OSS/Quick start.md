# Quick start {#concept_c31_lx1_5db .concept}

1.  Log on to the [OSS console](https://oss.console.aliyun.com/).
2.  Create a bucket.
3.  Upload and download files.

For more information, see [Get started with Alibaba Cloud OSS](../intl.en-US/Quick Start/Get started with Object Storage Service.md#).

## Get familiar with OSS upload and download {#section_itz_nx1_5db .section}

Before you use OSS SDKs, we recommend that you have basic familiarity with the OSS upload and download methods.

OSS uses RESTful APIs to perform operations and all requests are standard HTTP requests.

-   OSS provides different upload methods to meet different requirements. You can: Use the Put Object method to upload a single file smaller than 5 GB to OSS. For more information, see [Simple upload](intl.en-US/Developer Guide/Upload files/Simple upload.md#). Use the Post Object method \(HTTP form\) to upload a file smaller than 5 GB to OSS from a browser. For more information, see [Form upload](intl.en-US/Developer Guide/Upload files/Form upload.md#). Use the Multipart Upload method to upload a file larger than 5 GB. For more information, see [Multipart upload](intl.en-US/Developer Guide/Upload files/Multipart upload.md#). Use the Append Object method to directly append content to the end of an object. This method is particularly well suited for video monitoring and live video broadcasting. For more information, see [Append object](intl.en-US/Developer Guide/Upload files/Append object.md#).
-   OSS also provides different download methods. For more information, see [Simple download](intl.en-US/Developer Guide/Download Files/Simple download.md#) and [Multipart download](intl.en-US/Developer Guide/Download Files/Multipart download.md#).

## General process of using OSS SDKs {#section_ktz_nx1_5db .section}

1.  Obtain the AccessKeyId and AccessKeySecret from the console. For more information, see Create an AccessKey.
2.  Download the OSS SDKs in your preferred programming language from GitHub.
3.  Perform uploads, downloads, and other operations.

For more information about how to use the OSS SDKs in different programming languages, see [OSS SDK Reference](https://www.alibabacloud.com/help/doc-detail/52834.htm).

