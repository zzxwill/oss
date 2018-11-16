# Exception handling {#concept_35028_zh .concept}

OSS PHP SDK exceptions \(OssException\) include various errors, such as invalid parameters and file missing. You can use the getMessage method to obtain error messages.

For more information about OssException, see [GitHub](https://github.com/aliyun/aliyun-oss-php-sdk/blob/master/src/OSS/Core/OssException.php).

## Example for handling exceptions {#section_u2b_zcs_kfb .section}

The following code shows how to handle exceptions when creating a existing bucket and prints error messages.

```language-php
    try {
        $ossClient->createBucket($bucket);
    } catch (OssException $e) {
		print("Exception:" . $e->getMessage() . "\n");
    }

```

You can also obtain the following information:

|Parameter|Description|
|:--------|:----------|
|HTTPStatus|HTTP status code. You can get the status code using the getHTTPStatus method.|
|ErrorCode|Specifies the error code returned by OSS. You can get the error code using the getErrorCode method.|
|ErrorMessage|Specifies the error messages returned by OSS. You can get the error messages using the getErrorMessage method.|
|RequestId|Specifies the UUID. It is unique and used to identify a request. If a problem persists, send the RequestId to OSS developers for help. You can get the RequestId using the getRequestId method.|
|Details|Specifies detailed error messages returned by OSS. You can get the detailed error messages using the getDetails method.|

## Common OSS error codes { .section}

|Error code|Description|HTTP status code|
|:---------|:----------|:---------------|
|AccessDenied|Access denied|403|
|BucketAlreadyExists|The bucket already exists.|409|
|BucketNotEmpty|The bucket is not empty.|409|
|EntityTooLarge|The entity size exceeds the maximum limit.|400|
|EntityTooSmall|The entity size is below the minimum limit.|400|
|FileGroupTooLarge|The total file group size exceeds the maximum limit.|400|
|FilePartNotExist|No such part exists.|400|
|FilePartStale|The part has expired.|400|
|InvalidArgument|The parameter format is invalid.|400|
|InvalidAccessKeyId|No such AccessKeyId exists.|403|
|InvalidBucketName|The bucket name is invalid.|400|
|InvalidDigest|The digest is invalid.|400|
|InvalidObjectName|The object name is invalid.|400|
|InvalidPart|The part is invalid.|400|
|InvalidPartOrder|The part order is invalid.|400|
|InvalidTargetBucketForLogging|The buckets that store log files are invalid.|400|
|InternalError|Internal OSS error|500|
|MalformedXML|The XML format is invalid.|400|
|MethodNotAllowed|The method is not allowed.|405|
|MissingArgument|Parameters are not configured.|411|
|MissingContentLength|The content length is not configured.|411|
|NoSuchBucket|No such bucket exists.|404|
|NoSuchKey |No such object exists.|404|
|NoSuchUpload|No such uploadId exists.|404|
|NotImplemented|The methods are not implemented.|501|
|PreconditionFailed|Precondition error|412|
|RequestTimeTooSkewed|The local time set for OSSClient is deviated from the time set for the OSS server by over 15 minutes.|403|
|RequestTimeout|Request timeout|400|
|SignatureDoesNotMatch|Signature mismatch|403|
|InvalidEncryptionAlgorithmError|The specified encryption algorithm \(the entropy encoding type\) is invalid.|400|

