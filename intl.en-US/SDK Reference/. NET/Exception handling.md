# Exception handling {#concept_32097_zh .concept}

OSS .NET SDK has two types of exceptions: ClientException and OSSException. Both share the properties of RuntimeException.

## ClientException {#section_syf_ct2_lfb .section}

The ClientException exception occurs when the client attempts to send requests and when the data is transmitted to OSS. For example, when a request is sent, ClientException may occur due to unavailable network connections. ClientException may occur during file uploads due to IO exceptions.

## OSSException { .section}

OSSException indicates a server exception. The exception occurs because a server error message is parsed. OSSException includes the error code and information returned from OSS. It is used to locate and handle problems.

The following table describes common OSSException information.

|Parameter|Description|
|:--------|:----------|
|Code|Specifies the error code returned by OSS.|
|Message|Specifies the detailed error information returned by OSS.|
|RequestId|Specifies the UUID. It is unique and used to identify a request. If a problem persists, send the RequestId to OSS developers for help.|
|HostId|Identifies an OSS cluster. Ensure that the value of HostId is consistent with the Host \(endpoint to access a bucket\) used in the request.|

## OSS error codes { .section}

|Error code|Description|
|:---------|:----------|
|AccessDenied|Access denied|
|BucketAlreadyExists|The bucket already exists.|
|BucketNotEmpty|The bucket is not empty.|
|EntityTooLarge|The entity size exceeds the maximum limit.|
|EntityTooSmall|The entity size is below the minimum limit.|
|FileGroupTooLarge|The total file group size exceeds the maximum limit.|
|FilePartNotExist|No such part exists.|
|FilePartStale|The part has expired.|
|InvalidArgument|The parameter format is invalid.|
|InvalidAccessKeyId|No such AccessKeyId exists.|
|InvalidBucketName|The bucket name is invalid.|
|InvalidDigest|The digest is invalid.|
|InvalidObjectName|The object name is invalid.|
|InvalidPart|The part is invalid.|
|InvalidPartOrder|The part order is invalid.|
|InvalidTargetBucketForLogging|The buckets that store log files are invalid.|
|InternalError|Internal OSS error|
|MalformedXML|The XML format is invalid.|
|MethodNotAllowed|The method is not allowed.|
|MissingArgument|Parameters are not configured.|
|MissingContentLength|The content length is not configured.|
|NoSuchBucket|No such bucket exists.|
|NoSuchKey |No such object exists.|
|NoSuchUpload|No such uploadId exists.|
|NotImplemented|The methods are not implemented.|
|PreconditionFailed|Precondition error|
|RequestTimeTooSkewed|The local time set for OSSClient is deviated from the time set for the OSS server by over 15 minutes.|
|RequestTimeout|Request timeout|
|SignatureDoesNotMatch|Signature mismatch|
|TooManyBuckets|The number of buckets exceeds the limit.|

