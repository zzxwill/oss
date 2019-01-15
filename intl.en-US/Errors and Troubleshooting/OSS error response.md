# OSS error response {#concept_dt2_hq3_wdb .concept}

If an error occurs when a user accesses the OSS, the OSS returns the error code and error message, so that the user can locate the problem and handle it properly.

## OSS error response format {#section_zgc_wq3_wdb .section}

If an error occurs when the user accesses the OSS, the OSS returns an HTTP status code 3xx, 4xx, or 5xx and a message body in application/XML format.

Example of the message body for an error returned:

```
<? xml version="1.0" ? >
                <Error xmlns=”http://doc.oss-cn-hangzhou.aliyuncs.com”>
                <Code>
        AccessDenied
        </Code>
        <Message>
        Query-string authentication requires the Signature, Expires and OSSAccessKeyId parameters
        </Message>
    <RequestId>
        1D842BC5425544BB
    </RequestId>
    <HostId>
        oss-cn-hangzhou.aliyuncs.com
        </HostId>
        </Error>
```

All error message bodies include the following elements:

-   Code: the error code that OSS returns to the user.
-   Message: the detailed error message provided by OSS.
-   RequestId: the UUID that uniquely identifies a request. When you cannot solve the problem, you can seek help from OSS development engineers by providing the RequestId.
-   HostId: used to identify the accessed OSS cluster, which is consistent with the Host ID carried in the user request.

For special error information elements, see specific request descriptions.

## OSS error codes {#section_g4x_wq3_wdb .section}

The following table lists the OSS error codes:

|Error Code|Description|HTTP Status Code|Description|
|:---------|:----------|:---------------|:----------|
|AccessDenied| Access is denied.|403|To learn the cause and for troubleshooting, see [Permission and Troubleshooting](reseller.en-US/Errors and Troubleshooting/OSS permission.md#).|
|BucketAlreadyExists| The bucket already exists.|409|The bucket name specified by the [CreateBucket](../../../../../reseller.en-US/API Reference/Bucket operations/PutBucket.md#) operation has been used. Select a new [BucketName](../../../../../reseller.en-US/Developer Guide/Basic concepts.md#)|
|BucketNotEmpty|The bucket is not empty.|409|Before you perform a [DeleteBucket](../../../../../reseller.en-US/API Reference/Bucket operations/Delete Bucket.md#) operation, delete the files and unfinished multipart upload tasks in the bucket.|
|CallbackFailed|Upload callback fails.|203|To learn the cause and troubleshooting, see [Upload Callback Error and Troubleshooting](reseller.en-US/Errors and Troubleshooting/Upload callback.md#).|
|EntityTooLarge|The entity is too large.|400|The message length of the Post request exceeds 5 GB. To learn the cause and for troubleshooting, see [PostObject Error and Troubleshooting](reseller.en-US/Errors and Troubleshooting/PostObject.md#).|
|EntityTooSmall|The entity is too small.|400|The message length of the Post request is too short. For troubleshooting, see [PostObject Error and Troubleshooting](reseller.en-US/Errors and Troubleshooting/PostObject.md#).|
|FieldItemTooLong|The form field in the Post request is too large|400|Only the field `file`can exceed 4 KB. For troubleshooting, see [PostObject Error and Troubleshooting](reseller.en-US/Errors and Troubleshooting/PostObject.md#).|
|FilePartInterity|The file part has changed.|400|The data and the checksum are not consistent during partition data reading.|
|FilePartNotExist| The file part does not exist.|400|The partitions submitted by the [CompleteMultipartUpload](../../../../../reseller.en-US/API Reference/Multipart upload operations/CompleteMultipartUpload.md#) operation are not uploaded.|
|FilePartStale| The file part times out.|400|The data and the length are not consistent during partition data reading.|
|IncorrectNumberOfFilesInPOSTRequest|The number of files in the Post request is invalid.|400|Only one`file`field is allowed in the form fields of the Post request. For troubleshooting, see Post Object Error and Troubleshooting.|
|InvalidArgument|The parameter format is incorrect.|400|The parameter format does not comply with the requirements. Follow the instructions of corresponding [API](../../../../../reseller.en-US/API Reference/API overview.md#).|
|InvalidAccessKeyId|The AccessKeyId does not exist.|403|The AccessKeyId is invalid or has timed out. For troubleshooting, see [403 Error and Troubleshooting](reseller.en-US/Errors and Troubleshooting/OSS 403.md#).|
|InvalidBucketName|Invalid bucket name.|400|For the bucket naming rules, see [Developer Guide](../../../../../reseller.en-US/Developer Guide/Basic concepts.md#).|
|InvalidDigest|Invalid digest.|400|The specified MD5 checksum is inconsistent with the file. For MD5 calculations, see[PutObject](../../../../../reseller.en-US/API Reference/Object operations/PutObject.md#).|
|InvalidEncryptionAlgorithmError| The specified entropy encryption algorithm is incorrect.|400|Currently only the `AES256`encryption algorithm is supported. For more information, see [PutObject](../../../../../reseller.en-US/API Reference/Object operations/PutObject.md#).|
|InvalidObjectName| Invalid object name.|400|For the object naming rules, see [Developer Guide](../../../../../reseller.en-US/Developer Guide/Basic concepts.md#).|
|InvalidPart|Invalid part.|400|The part submitted by the [CompleteMultipartUpload](../../../../../reseller.en-US/API Reference/Multipart upload operations/CompleteMultipartUpload.md#) operation is invalid. The`PartNumber` or the `ETag` is incorrect.|
|InvalidPartOrder|Invalid part sequence.|400|The parts submitted by the [CompleteMultipartUpload](../../../../../reseller.en-US/API Reference/Multipart upload operations/CompleteMultipartUpload.md#) operation is in an ascending sort order of the`PartNumber`.|
|InvalidPolicyDocument| Invalid policy document.|400|The `Policy`in the Post request is invalid. For troubleshooting, see [Post Object Error and Troubleshooting.](reseller.en-US/Errors and Troubleshooting/PostObject.md#)|
|InvalidTargetBucketForLogging|An invalid target bucket exists in the logging operation.|400|The target bucket for storing the [Logging](../../../../../reseller.en-US/API Reference/Bucket operations/PutBucketLogging.md#)does not exist. Change it.|
|InternalError|An error occurs in OSS.|500|Try again.|
|MalformedXML|XML format is invalid.|400|The `XML` in the request is invalid, excluding [DeleteObjects](../../../../../reseller.en-US/API Reference/Object operations/DeleteMultipleObjects.md#), [CompleteMultipartUpload](../../../../../reseller.en-US/API Reference/Multipart upload operations/CompleteMultipartUpload.md#), [PutBucketLogging](../../../../../reseller.en-US/API Reference/Bucket operations/PutBucketLogging.md#), [PutBucketWebsite](../../../../../reseller.en-US/API Reference/Bucket operations/PutBucketWebsite.md#), [PutBucketLifecycle](../../../../../reseller.en-US/API Reference/Bucket operations/PutBucketLifecycle.md#), [PutBucketReferer](../../../../../reseller.en-US/API Reference/Bucket operations/Put Bucket Referer.md#), and [PutBucketCORS](../../../../../reseller.en-US/API Reference/Cross-Origin Resource Sharing/PutBucketcors.md#) according to specific requests.|
|MalformedPOSTRequest|The body format of the Post request is invalid.|400|The form field format is invalid. For troubleshooting, see [Post Object Error and Troubleshooting](reseller.en-US/Errors and Troubleshooting/PostObject.md#).|
|MaxPOSTPreDataLengthExceededError|The size of the body outside the uploaded file content of the Post request is too large.|400|Only the field`file`can exceed 4 KB. For troubleshooting, see [Post Object Error and Troubleshooting](reseller.en-US/Errors and Troubleshooting/PostObject.md#).|
|MethodNotAllowed|Unsupported method.|405|Access the resource with a method not supported by OSS.|
|MissingArgument|Missing argument.|411|See the specific[API](../../../../../reseller.en-US/API Reference/API overview.md#)for resolution.|
|MissingContentLength| Missing content length.|411|The message is not of the [chunked encoding](https://tools.ietf.org/html/rfc2616#section-3.6.1) and does not carry the `Content-Length`.|
|NoSuchBucket|The bucket does not exist.|404| |
|NoSuchKey|The object does not exist.|404| |
|NoSuchUpload|The multipart upload ID does not exist.|404|No Initialize Multipart Upload or the initialized Multipart Upload Expires.|
|NotImplemented|The method cannot be implemented.|400| Operation not supported by OSS.|
|ObjectNotAppendable| Not an appendable file.|409|OSS has three files types: [normal](../../../../../reseller.en-US/API Reference/Object operations/PutObject.md#), [appendable](../../../../../reseller.en-US/API Reference/Object operations/AppendObject.md#), and [multipart](../../../../../reseller.en-US/API Reference/Multipart upload operations/InitiateMultipartUpload.md#). Only the `appendable` type of file can run the`AppendObject`operation.|
|PositionNotEqualToLength|The appending position does not match the file length.|409|For more information, see [AppendObject](../../../../../reseller.en-US/API Reference/Object operations/AppendObject.md#).|
|PreconditionFailed|Pre-processing error.|412|The downloading conditions are not met. For more information, see [GetObject](../../../../../reseller.en-US/API Reference/Object operations/GetObject.md#).|
|RequestTimeTooSkewed|The request initiation time exceeds the server time by 15 minutes.|403|For troubleshooting, see [403 Error and Troubleshooting](reseller.en-US/Errors and Troubleshooting/OSS 403.md#).|
|RequestTimeout|The request times out.|400|Try again.|
|RequestIsNotMultiPartContent| The content-type of the Post request is invalid.|400|For troubleshooting, see [Post Object Error and Troubleshooting](reseller.en-US/Errors and Troubleshooting/PostObject.md#).|
|Downloadtrafficratelimitexceeded| The downloading traffic exceeds the limit.|503| |
|UploadTrafficRateLimitExceeded|The uploading traffic exceeds the limit.|503| |
|SignatureDoesNotMatch|Signature error.|403|For troubleshooting, see [Signature in Header](../../../../../reseller.en-US/API Reference/Access control/Add a signature to the header.md#) and [Signature in URL](../../../../../reseller.en-US/API Reference/Access control/Add a signature to a URL.md#).|
|TooManyBuckets| The number of buckets exceeds the limit.|400| |

OSS common errors and troubleshooting

-   [Upload Callback Error and Troubleshooting](reseller.en-US/Errors and Troubleshooting/Upload callback.md#)
-   [403 Error and Troubleshooting](reseller.en-US/Errors and Troubleshooting/OSS 403.md#)
-   [Post Object Error and Troubleshooting](reseller.en-US/Errors and Troubleshooting/PostObject.md#)
-   [Permission Error and Troubleshooting](reseller.en-US/Errors and Troubleshooting/OSS permission.md#)
-   [CORS Error and Troubleshooting](reseller.en-US/Errors and Troubleshooting/OSS CORS.md#)
-   [Anti-Leech Referer Configuration and Error Elimination](reseller.en-US/Errors and Troubleshooting/Referer.md#)
-   [STS Common Issues and Troubleshooting](reseller.en-US/Errors and Troubleshooting/STS common errors and troubleshooting.md#)

SDK/Tool common errors and troubleshooting

-   [ossfs](../../../../../reseller.en-US/Tools/ossfs/FAQ.md#)
-   [ossftp](../../../../../reseller.en-US/Tools/ossftp/Quick installation for OSS FTP.md#)

## Operations not supported by OSS {#section_ecd_wr3_wdb .section}

If an operation not supported by the OSS is used to access a resource, the OSS returns error 405 Method Not Allowed.

Example of an invalid request:

```
ABC /1.txt HTTP/1.1
Host: bucketname.oss-cn-shanghai.aliyuncs.com
Date: Thu, 11 Aug 2016 03:53:40 GMT
Authorization: signatureValue
```

Returns an example:

```
HTTP/1.1 405 Method Not Allowed
Server: AliyunOSS
Date: Thu, 11 Aug 2016 03:53:44 GMT
Content-Type: application/xml
Content-Length: 338
Connection: keep-alive
x-oss-request-id: 57ABF6C8BC4D25D86CBA5ADE
Allow: GET DELETE HEAD PUT POST OPTIONS
<? xml version="1.0" encoding="UTF-8"? >
<Error>
<Code>MethodNotAllowed</Code>
<Message>The specified method is not allowed against this resource.</Message>
  <RequestId>57ABF6C8BC4D25D86CBA5ADE</RequestId>
  <HostId>bucketname.oss-cn-shanghai.aliyuncs.com</HostId>
  <Method>abc</Method>
  <ResourceType>Bucket</ResourceType>
  </Error>
```

**Note:** If the accessed resource is /bucket/, ResourceType must be set to `bucket`.If the accessed resource is /bucket/object, ResourceType must be set to `object`.

## Operations supported by the OSS but not supported by parameters {#section_hrc_fs3_wdb .section}

If parameters not supported by the OSS are added to an operation supported by the OSS \(for example, an If-Modified-Since parameter is added to the PUT operation\), the OSS returns error 400  Bad Request.

Example of an invalid request:

```
PUT /abc.zip HTTP/1.1
Host: bucketname.oss-cn-shanghai.aliyuncs.com
Accept: */*
Date: Thu, 11 Aug 2016 01:44:50 GMT
If-Modified-Since: Thu, 11 Aug 2016 01:43:51 GMT
Content-Length: 363
```

Response example:

```
HTTP/1.1 400 Bad Request
Server: AliyunOSS
Date: Thu, 11 Aug 2016 01:44:54 GMT
Content-Type: application/xml
Content-Length: 322
Connection: keep-alive
x-oss-request-id: 57ABD896CCB80C366955187E
x-oss-server-time: 0
<? xml version="1.0" encoding="UTF-8"? >
<Error>
<Code>NotImplemented</Code>
<Message>A header you provided implies functionality that is not implemented.</Message>
<RequestId>57ABD896CCB80C366955187E</RequestId>
<HostId>bucketname.oss-cn-shanghai.aliyuncs.com</HostId>
<Header>If-Modified-Since</Header>
</Error>
```

