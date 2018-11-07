# Exception handling {#concept_32023_zh .concept}

OSS Java SDK has two types of exceptions: ClientException and OSSException. Both share the properties of RuntimeException.

## Example for handling exceptions {#section_okx_3gd_kfb .section}

Run the following code to handle exceptions:

```language-java
try {
    // Perform operations on OSS, for example, object uploads.
    ossClient.putObject(...);
} catch (OSSException oe) {
    System.out.println("Caught an OSSException, which means your request made it to OSS, "
            + "but was rejected with an error response for some reason.") ;
    System.out.println("Error Message: " + oe.getErrorCode());
    System.out.println("Error Code:       " + oe.getErrorCode());
    System.out.println("Request ID:      " + oe.getRequestId());
    System.out.println("Host ID:           " + oe.getHostId());
} catch (ClientException ce) {
    System.out.println("Caught an ClientException, which means the client encountered "
            + "a serious internal problem while trying to communicate with OSS, "
            + "such as not being able to access the network.") ;
    System.out.println("Error Message: " + ce.getMessage());
} finally {
    if (ossClient ! = null) {
        ossClient.shutdown();
    }
}

```

## ClientException { .section}

The ClientException exception occurs when the client attempts to send requests and when the data is transmitted to OSS. For example, when a request is sent, ClientException may occur due to unavailable network connections. ClientException may occur during file uploads due to IO exceptions.

## OSSException { .section}

ServiceException indicates a server exception. The exception occurs because a server error message is parsed. OSSException includes the error code and information returned from OSS. It is used to locate and handle problems.

The following table describes common OSSException information.

|Parameter|Description|
|:--------|:----------|
|Code|Specifies the error code returned by OSS.|
|Message|Specifies the detailed error information returned by OSS.|
|RequestId|Specifies the UUID. It is unique and used to identify a request. If a problem persists, send the RequestId to OSS developers for help.|
|HostId|Identifies an OSS cluster. Ensure that the value of HostId is consistent with the Host \(endpoint to access a bucket\) used in the request.|

## Common OSS error codes { .section}

|Error Code|Description|HTTP status code|
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
|NoSuchKey|No such object exists.|404|
|NoSuchUpload|No such uploadId exists.|404|
|NotImplemented|The methods are not implemented.|501|
|PreconditionFailed|Precondition error|412|
|RequestTimeTooSkewed|The local time set for OSSClient is deviated from the time set for the OSS server by over 15 minutes.|403|
|RequestTimeout|Request timeout|400|
|SignatureDoesNotMatch|Signature mismatch|403|
|InvalidEncryptionAlgorithmError|The specified encryption algorithm \(the entropy encoding type\) is invalid.|400|

