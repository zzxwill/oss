# Multipart upload {#concept_84786_zh .concept}

This topic describes how to use multipart upload.

For the complete code of multipart upload, see [GitHub](https://github.com/aliyun/aliyun-oss-java-sdk/blob/master/src/samples/MultipartUploadSample.java).

To enable multipart upload, perform the following steps:

1.  Initiate a multipart upload event.

    You can call ossClient.initiateMultipartUpload to return the globally unique uploadId created in OSS.

2.  Upload parts.

    You can call ossClient.uploadPart to upload part data.

    **Note:** 

    -   For parts with a same uploadId, parts are sequenced by their part numbers. If you have uploaded a part and use the same part number to upload another part, the later part will replace the former part.
    -   OSS places the MD5 value of part data in ETag and returns the MD5 value to the user.
    -   SDK automatically configures Content-MD5. OSS calculates the MD5 value of uploaded data and compares it with the MD5 value calculated by SDK. If the two values vary, the error code of InvalidDigest is returned.
3.  Complete multipart upload.

    After you have uploaded all parts, call partossClient.completeMultipartUpload to combine these parts into a complete object.


The following code is used as a complete example that describes the process of multipart upload:

```language-java
// This example uses endpoint China East 1 (Hangzhou). Specify the actual endpoint based on your requirements.
String endpoint = "http://oss-cn-hangzhou.aliyuncs.com";
// It is highly risky to log on with AccessKey of an Alibaba Cloud account because the account has permissions on all APIs in OSS. We recommend that you log on as a RAM user to access APIs or perform routine operations and maintenance. To create a RAM account, log on to https://ram.console.aliyun.com.
String accessKeyId = "<yourAccessKeyId>";
String accessKeySecret = "<yourAccessKeySecret>";
String bucketName = "<yourBucketName>";
String objectName = "<yourObjectName>";

// Create an OSSClient instance.
OSSClient ossClient = new OSSClient(endpoint, accessKeyId, accessKeySecret);

/* Step 1: Initiate a multipart upload event.
*/
InitiateMultipartUploadRequest request = new InitiateMultipartUploadRequest(bucketName, objectName);
InitiateMultipartUploadResult result = ossClient.initiateMultipartUpload(request);
// An uploadId is returned. It is the unique identifier for a part upload event. You can initiate related operations (such as part upload cancelation and query) based on the uploadId.
String uploadId = result.getUploadId();

/* Step 2: Upload parts.
*/
// partETags is a set of PartETags. A PartETag consists of an ETag and a part number.
List<PartETag> partETags =  new ArrayList<PartETag>();
// Calculate the total number of parts.
final long partSize = 1 * 1024 * 1024L;   // 1MB
final File sampleFile = new File("<localFile>");
long fileLength = sampleFile.length();
int partCount = (int) (fileLength / partSize);
if (fileLength % partSize ! = 0) {
    partCount++;
 }
// Upload each part simultaneously until all parts are uploaded.
for (int i = 0; i < partCount; i++) {
    long startPos = i * partSize;
    long curPartSize = (i + 1 == partCount) ? (fileLength - startPos) : partSize;
    InputStream instream = new FileInputStream(sampleFile);
    // Skip parts that have been uploaded.
    instream.skip(startPos);
    UploadPartRequest uploadPartRequest = new UploadPartRequest();
    uploadPartRequest.setBucketName(bucketName);
    uploadPartRequest.setKey(objectName);
    uploadPartRequest.setUploadId(uploadId);
    uploadPartRequest.setInputStream(instream);
    // Configure the size available for each part. Aside from the last part, all parts are sized 100 KB at least.
    uploadPartRequest.setPartSize(curPartSize);
    // Configure part numbers. Each part is configured with a part number. The value can be from 1 to 10,000. If you configure a number beyond the range, OSS returns an InvalidArgument error code.
    uploadPartRequest.setPartNumber( i + 1);
	// Do not upload each part sequentially. They can be uploaded from different OSSClients. Then they are sequenced and combined into a complete object based on part numbers.
    UploadPartResult uploadPartResult = ossClient.uploadPart(uploadPartRequest);
	// After you upload a part, OSS returns a result that contains a PartETag. The PartETag is stored in partETags.
    partETags.add(uploadPartResult.getPartETag());
}

/* Step 3: Complete multipart upload.
*/
// Sequence parts. partETags must be sequenced by part numbers in ascending order.
Collections.sort(partETags, new Comparator<PartETag>() {
    public int compare(PartETag p1, PartETag p2) {
        return p1.getPartNumber() - p2.getPartNumber();
    }
});
// You must provide all valid PartETags when you perform this operation. OSS verifies the validity of all parts one by one after it receives PartETags. After part verification is successful, OSS combines these parts into a complete object.
CompleteMultipartUploadRequest completeMultipartUploadRequest =
        new CompleteMultipartUploadRequest(bucketName, objectName, uploadId, partETags);
ossClient.completeMultipartUpload(completeMultipartUploadRequest);

// Close your OSSClient.
ossClient.shutdown();

```

## Cancel a multipart upload event {#section_rly_5pb_kfb .section}

You can call ossClient.abortMultipartUpload to cancel a multipart upload event. If you cancel a multipart upload event, you are not allowed to perform any other operations with this uploadId anymore. The uploaded parts will be deleted.

Run the following code to cancel a multipart upload event:

```language-java
// This example uses endpoint China East 1 (Hangzhou). Specify the actual endpoint based on your requirements.
String endpoint = "http://oss-cn-hangzhou.aliyuncs.com";
// It is highly risky to log on with AccessKey of an Alibaba Cloud account because the account has permissions on all the APIs in OSS. We recommend that you log on as a RAM user to access APIs or perform routine operations and maintenance. To create a RAM account, log on to https://ram.console.aliyun.com.
String accessKeyId = "<yourAccessKeyId>";
String accessKeySecret = "<yourAccessKeySecret>";

// Create an OSSClient instance.
OSSClient ossClient = new OSSClient(endpoint, accessKeyId, accessKeySecret);

// Cancel multipart upload. The uploadId is from InitiateMultipartUpload.
AbortMultipartUploadRequest abortMultipartUploadRequest =
        new AbortMultipartUploadRequest("<yourBucketName>", "<yourObjectName>", "<uploadId>");
ossClient.abortMultipartUpload(abortMultipartUploadRequest);

// Close your OSSClient.
ossClient.shutdown();

```

## List uploaded parts { .section}

Call ossClient.listParts to list all uploaded parts with a specified uploadId.

-   Simple list

    Run the following code for simple list:

    ```language-java
    // This example uses endpoint China East 1 (Hangzhou). Specify the actual endpoint based on your requirements.
    String endpoint = "http://oss-cn-hangzhou.aliyuncs.com";
    // It is highly risky to log on with AccessKey of an Alibaba Cloud account because the account has permissions on all the APIs in OSS. We recommend that you log on as a RAM user to access APIs or perform routine operations and maintenance. To create a RAM account, log on to https://ram.console.aliyun.com.
    String accessKeyId = "<yourAccessKeyId>";
    String accessKeySecret = "<yourAccessKeySecret>";
    
    // Create an OSSClient instance.
    OSSClient ossClient = new OSSClient(endpoint, accessKeyId, accessKeySecret);
    
    // List uploaded parts. The uploadId is returned from InitiateMultipartUpload.
    ListPartsRequest listPartsRequest = new ListPartsRequest("<yourBucketName>", "<yourObjectName>", "<uploadId>");
     // Configure uploadId.
     //listPartsRequest.setUploadId(uploadId);
     // Set the maximum number of parts that can be displayed on each page to 100. The default number is 1,000.
     listPartsRequest.setMaxParts(100);
     // Specify the initial position in the list. Only the parts with part numbers greater than this parameter value are listed.
     listPartsRequest.setPartNumberMarker(2);
    PartListing partListing = ossClient.listParts(listPartsRequest);
    
    for (PartSummary part : partListing.getParts()) {
        // Obtain part numbers.
        part.getPartNumber();
        // Obtain the size of part data.
        part.getSize();
        // Obtain ETag.
        part.getETag();
        // Obtain the latest time parts are modified.
        part.getLastModified();
    }
    
    // Close your OSSClient.
    ossClient.shutdown();
    
    ```

-   List all uploaded parts

    By default, listParts can only list a maximum of 1,000 parts at a time. To list more than 1,000 uploaded parts, run the following code:

    ```language-java
    // This example uses endpoint China East 1 (Hangzhou). Specify the actual endpoint based on your requirements.
    String endpoint = "http://oss-cn-hangzhou.aliyuncs.com";
    // It is highly risky to log on with AccessKey of an Alibaba Cloud account because the account has permissions on all the APIs in OSS. We recommend that you log on as a RAM user to access APIs or perform routine operations and maintenance. To create a RAM account, log on to https://ram.console.aliyun.com.
    String accessKeyId = "<yourAccessKeyId>";
    String accessKeySecret = "<yourAccessKeySecret>";
    
    // Create an OSSClient instance.
    OSSClient ossClient = new OSSClient(endpoint, accessKeyId, accessKeySecret);
    
    // List all uploaded parts.
    PartListing partListing;
    ListPartsRequest listPartsRequest = new ListPartsRequest("<yourBucketName>", "<yourObjectName>", "<uploadId>");
    
    do {
        partListing = ossClient.listParts(listPartsRequest);
    	
        for (PartSummary part : partListing.getParts()) {
            // Obtain part numbers.
            part.getPartNumber();
            // Obtain the part size.
            part.getSize();
            // Obtain ETag.
            part.getETag();
            // Obtain the latest time parts are modified.
            part.getLastModified();
        }
    // Specify the initial position for the list. Only the parts with part numbers greater than the initial value are listed.
    listPartsRequest.setPartNumberMarker(partListing.getNextPartNumberMarker());
    } while (partListing.isTruncated());
    
    // Close your OSSClient.
    ossClient.shutdown()
    
    ```

-   List all uploaded parts on one or more pages

    Run the following code to specify the maximum number of parts displayed on each page:

    ```language-java
    // This example uses endpoint China East 1 (Hangzhou). Specify the actual endpoint based on your requirements.
    String endpoint = "http://oss-cn-hangzhou.aliyuncs.com";
    // It is highly risky to log on with AccessKey of an Alibaba Cloud account because the account has permissions on all the APIs in OSS. We recommend that you log on as a RAM user to access APIs or perform routine operations and maintenance. To create a RAM account, log on to https://ram.console.aliyun.com.
    String accessKeyId = "<yourAccessKeyId>";
    String accessKeySecret = "<yourAccessKeySecret>";
    
    // Create an OSSClient instance.
    OSSClient ossClient = new OSSClient(endpoint, accessKeyId, accessKeySecret);
    
    // List all uploaded parts on one or more pages.
    PartListing partListing;
    ListPartsRequest listPartsRequest = new ListPartsRequest("<yourBucketName>", "<yourObjectName>", "<uploadId>");
    // Set the maximum number of parts displayed on each page to 100.
    listPartsRequest.setMaxParts(100);
    
    do {
        partListing = ossClient.listParts(listPartsRequest);
        
        for (PartSummary part : partListing.getParts()) {
            // Obtain part numbers.
            part.getPartNumber();
            // Obtain the part size.
            part.getSize();
            // Gets the etag of the slice.
            // Obtain ETag.
            // Obtain the latest time parts are modified.
            part.getLastModified();
        }
        
        listPartsRequest.setPartNumberMarker(partListing.getNextPartNumberMarker());
        
    } while (partListing.isTruncated());
    
    // Close your OSSClient.
    ossClient.shutdown();
    
    ```


## List all part upload events for a bucket { .section}

Call ossClient.listMultipartUploads to list all ongoing part upload events \(events that have been initiated but not completed or have been canceled\). You can configure the following parameters:

|Parameter|Description|Configuration method|
|---------|-----------|--------------------|
|prefix|Specifies the prefix that must be included in the returned object name. Note that if you use a prefix for query, the returned object name will contain the prefix.|ListMultipartUploadsRequest.setPrefix\(String prefix\)|
|delimiter|Specifies a delimiter of a forward slash \(/\) used to group object names. The object between the specified prefix and the first occurrence of a delimiter of a forward slash \(/\) is commonPrefixes.|ListMultipartUploadsRequest.setDelimiter\(String delimiter\)|
|maxUploads|Specifies the maximum number of part upload events. The maximum value \(also default value\) you can set is 1,000.|ListMultipartUploadsRequest.setMaxUploads\(Integer maxUploads\)|
|keyMarker|Lists all part upload events with the object whose names start with a letter that comes after the keyMarker value in the alphabetical order. You can use this parameter with the uploadIdMarker parameter to specify the initial position for the specified returned result.|ListMultipartUploadsRequest.setKeyMarker\(String keyMarker\)|
|uploadIdMarker|You can use this parameter with the keyMarker parameter to specify the initial position for the specified returned result. If you do not configure keyMarker, the uploadIdMarker parameter is invalid. If you configure keyMarker, the query result contains:-   All objects whose names start with a letter that comes after the keyMarker value.
-   All objects whose names start with a letter that is the same as the keyMarker value in the alphabetical order and the value of uploadId greater than that of uploadIdMarker.

|ListMultipartUploadsRequest.setUploadIdMarker\(String uploadIdMarker\)|

-   Simple list

    Run the following code to list multipart upload events:

    ```language-java
    // This example uses endpoint China East 1 (Hangzhou). Specify the actual endpoint based on your requirements.
    String endpoint = "http://oss-cn-hangzhou.aliyuncs.com";
    // It is highly risky to log on with AccessKey of an Alibaba Cloud account because the account has permissions on all the APIs in OSS. We recommend that you log on as a RAM user to access APIs or perform routine operations and maintenance. To create a RAM account, log on to https://ram.console.aliyun.com.
    String accessKeyId = "<yourAccessKeyId>";
    String accessKeySecret = "<yourAccessKeySecret>";
    String bucketName = "<yourBucketName>";
    
    // Create an OSSClient instance.
    OSSClient ossClient = new OSSClient(endpoint, accessKeyId, accessKeySecret);
    
    // Lists multipart upload events. You can list 1,000 parts by default.
    ListMultipartUploadsRequest listMultipartUploadsRequest = new ListMultipartUploadsRequest(bucketName);
    MultipartUploadListing multipartUploadListing = ossClient.listMultipartUploads(listMultipartUploadsRequest);
    
    for (MultipartUpload multipartUpload : multipartUploadListing.getMultipartUploads()) {
        // Obtain uploadId.
        multipartUpload.getUploadId();
        // Obtain object names.
        multipartUpload.getKey();
        // Obtain the time part upload events are initiated.
        multipartUpload.getInitiated();
    }
    
    // Close your OSSClient.
    ossClient.shutdown();
    
    ```

    If the returned result shows that the value of isTruncated is false, the values of nextKeyMarker and nextUploadIdMarker are returned and used as the initial position for the next object reading. If you fail to obtain all part upload events at a time, list them by pagination.

-   List all part upload events

    By default, listMultipartUploads can only list a maximum of 1,000 parts at a time. To list more than 1,000 parts, run the following code to list all parts:

    ```language-java
    // This example uses endpoint China East 1 (Hangzhou). Specify the actual endpoint based on your requirements.
    String endpoint = "http://oss-cn-hangzhou.aliyuncs.com";
    // It is highly risky to log on with AccessKey of an Alibaba Cloud account because the account has permissions on all the APIs in OSS. We recommend that you log on as a RAM user to access APIs or perform routine operations and maintenance. To create a RAM account, log on to https://ram.console.aliyun.com.
    String accessKeyId = "<yourAccessKeyId>";
    String accessKeySecret = "<yourAccessKeySecret>";
    String bucketName = "<yourBucketName>";
    
    // Create an OSSClient instance.
    OSSClient ossClient = new OSSClient(endpoint, accessKeyId, accessKeySecret);
    
    // Lists multipart upload events.
    MultipartUploadListing multipartUploadListing;
    ListMultipartUploadsRequest listMultipartUploadsRequest = new ListMultipartUploadsRequest(bucketName);
    
    do {
        multipartUploadListing = ossClient.listMultipartUploads(listMultipartUploadsRequest);
    
        for (MultipartUpload multipartUpload : multipartUploadListing.getMultipartUploads()) {
            // Obtain uploadId.
            multipartUpload.getUploadId();
            // Obtain object names.
            multipartUpload.getKey();
            // Obtain the time part upload events are initiated.
            multipartUpload.getInitiated();
        }
        
        listMultipartUploadsRequest.setKeyMarker(multipartUploadListing.getNextKeyMarker());
        
        listMultipartUploadsRequest.setUploadIdMarker(multipartUploadListing.getNextUploadIdMarker());
    } while (multipartUploadListing.isTruncated());
    
    // Close your OSSClient.
    ossClient.shutdown();
    
    ```

-   List part upload events on one or more pages

    Run the following code to list part upload events on one or more pages:

    ```language-java
    // This example uses endpoint China East 1 (Hangzhou). Specify the actual endpoint based on your requirements.
    String endpoint = "http://oss-cn-hangzhou.aliyuncs.com";
    // It is highly risky to log on with AccessKey of an Alibaba Cloud account because the account has permissions on all the APIs in OSS. We recommend that you log on as a RAM user to access APIs or perform routine operations and maintenance. To create a RAM account, log on to https://ram.console.aliyun.com.
    String accessKeyId = "<yourAccessKeyId>";
    String accessKeySecret = "<yourAccessKeySecret>";
    String bucketName = "<yourBucketName>";
    
    // Create an OSSClient instance.
    OSSClient ossClient = new OSSClient(endpoint, accessKeyId, accessKeySecret);
    
    // Lists multipart upload events.
    MultipartUploadListing multipartUploadListing;
    ListMultipartUploadsRequest listMultipartUploadsRequest = new ListMultipartUploadsRequest(bucketName);
    // Configure the number of part upload events that can be displayed on each page.
    listMultipartUploadsRequest.setMaxUploads(50);
    
    do {
        multipartUploadListing = ossClient.listMultipartUploads(listMultipartUploadsRequest);
    
        for (MultipartUpload multipartUpload : multipartUploadListing.getMultipartUploads()) {
            // Obtain uploadId.
            multipartUpload.getUploadId();
            // Obtain object names.
            multipartUpload.getKey();
            // Obtain the time part upload events are initiated.
            multipartUpload.getInitiated();
        }
        
        listMultipartUploadsRequest.setKeyMarker(multipartUploadListing.getNextKeyMarker());
        listMultipartUploadsRequest.setUploadIdMarker(multipartUploadListing.getNextUploadIdMarker());
        
    } while (multipartUploadListing.isTruncated());
    
    // Close your OSSClient.
    ossClient.shutdown();
    
    ```


