# Multipart upload {#concept_90222_zh .concept}

This topic describes how to use multipart upload.

To enable multipart upload, perform the following steps:

1.  Initialize a multipart upload event.

    You can call InitiateMultipartUpload to return the globally unique upload ID created in OSS.

2.  Start the multipart upload event.

    Call UploadPart to upload multipart data.

    **Note:** 

    -   For parts that share an upload ID, their part numbers identify their relative positions in the entire object. If you have uploaded a part and used its part number again to upload another part, the later part overwrites the former part.
    -   OSS places the MD5 value of part data in ETag and returns the MD5 value to the user.
    -   The SDK automatically configures Content-MD5. OSS calculates the MD5 value of uploaded data and compares it with the MD5 value calculated by SDK. If the two values vary, the InvalidDigest error is returned.
3.  Complete the multipart upload event.

    After you have uploaded all parts, call CompleteMultipartUpload to combine these parts into a complete object.


Use the following code to complete multipart upload:

```
#include <alibabacloud/oss/OssClient.h>
using namespace AlibabaCloud::OSS; 
                                                        
int64_t getFileSize(const std::string& file)
{
    std::fstream f(file, std::ios::in | std::ios::binary);
    f.seekg(0, f.end);
    int64_t size = f.tellg();
    f.close();
    return size;
}

int main(void)
{
    /* Initialize the OSS account information. */
    std::string AccessKeyId = "yourAccessKeyId";
    std::string AccessKeySecret = "yourAccessKeySecret";
    std::string Endpoint = "yourEndpoint";
    std::string BucketName = "yourBucketName";
    std::string ObjectName = "yourObjectName";

    /* Initialize network resources. */
    InitializeSdk();

    ClientConfiguration conf;
    OssClient client(Endpoint, AccessKeyId, AccessKeySecret, conf);
    InitiateMultipartUploadRequest initUploadRequest(BucketName, ObjectName);
  
    /* Initiate the multipart upload event. */
    auto uploadIdResult = client.InitiateMultipartUpload(initUploadRequest);
    auto uploadId = uploadIdResult.result(). UploadId();
    std::string fileToUpload = "yourLocalFilename";
    int64_t partSize = 100 * 1024;
    PartList partETagList;
    auto fileSize = getFileSize(fileToUpload);
    int partCount = static_cast<int> (fileSize / partSize);
    /* Calculate the number of parts.*/
    if (fileSize % partSize ! = 0)  {
        partCount++;
    }
  
    /* Upload each part sequentially. */
    for (int i = 1; i <= partCount; i++) {
        auto skipBytes = partSize * (i - 1);
        auto size = (partSize < fileSize - skipBytes) ? partSize : (fileSize - skipBytes);
        std::shared_ptr<std::iostream> content = std::make_shared<std::fstream>(fileToUpload, std::ios::in|std::ios::binary);
        content->seekg(skipBytes, std::ios::beg);

        UploadPartRequest uploadPartRequest(BucketName, ObjectName, content);
        uploadPartRequest.setContentLength(size);
        uploadPartRequest.setUploadId(uploadId);
        uploadPartRequest.setPartNumber(i);
        auto uploadPartOutcome = client.UploadPart(uploadPartRequest);
        if (uploadPartOutcome.isSuccess()) {
            Part part(i, uploadPartOutcome.result(). ETag());
            partETagList.push_back(part);
        }
        else {
            std::cout << "uploadPart fail" <<
            ",code:" << outcome.error(). Code() <<
            ",message:" << outcome.error(). Message() <<
            ",requestId:" << outcome.error(). RequestId() << std::endl;
        }

    }
  

    /* Complete the multipart upload event. */
    CompleteMultipartUploadRequest request(BucketName, ObjectName);
    request.setUploadId(uploadId);
    request.setPartList(partETagList);
    auto outcome = client.CompleteMultipartUpload(request);
 
    if (! outcome.isSuccess()) {
        /* Handle exceptions. */
        std::cout << "CompleteMultipartUpload fail" <<
        ",code:" << outcome.error(). Code() <<
        ",message:" << outcome.error(). Message() <<
        ",requestId:" << outcome.error(). RequestId() << std::endl;
        ShutdownSdk();
        return -1;
    }
  
    /* Release network resources. */
    ShutdownSdk();
    return 0;
}
```

**Note:** Obtain the uploaded parts. ETag values of each part must be provided to call CompleteMultipartUpload.

You can use either of the follows method to obtain the ETag values:

-   First, the response of a part upload contains the ETag value of the part. You can save the values for future use.
-   You can also call the ListParts API to obtain the ETag value of the uploaded parts. In the example, the second method is used.

## List uploaded parts {#section_alv_2zy_zgb .section}

You can call ListParts to list all parts that are uploaded with the specified upload ID.

-   List uploaded parts

    **Note:** ListParts can only list a maximum of 1,000 parts simultaneously by default. To list more than 1,000 uploaded parts, list them by page.

    Use the following code to list all uploaded parts by page:

    ```
    #include <alibabacloud/oss/OssClient.h>
    using namespace AlibabaCloud::OSS;
    
    int main(void)
    {
        /* Initialize the OSS account information. */
        std::string AccessKeyId = "yourAccessKeyId";
        std::string AccessKeySecret = "yourAccessKeySecret";
        std::string Endpoint = "yourEndpoint";
        std::string BucketName = "yourBucketName";
        std::string ObjectName = "yourObjectName";
    
        /* Initialize network resources. */
        InitializeSdk();
    
        ClientConfiguration conf;
        OssClient client(Endpoint, AccessKeyId, AccessKeySecret, conf);
      
        /* List uploaded parts. A maximum of 1,000 parts can be listed by default. */
        ListPartsRequest listuploadrequest(BucketName, ObjectName);
        listuploadrequest.setUploadId(uploadId);
        do {
            auto listuploadresult = client.ListParts(listuploadrequest);
            if (! listUploadResult.isSuccess()) {
                /* Handle exceptions. */
                std::cout << "ListParts fail" <<
                ",code:" << listuploadresult.error(). Code() <<
                ",message:" << listuploadresult.error(). Message() <<
                ",requestId:" << listuploadresult.error(). RequestId() << std::endl;
                break;
            }
            else {
                for (const auto& part : listuploadresult.result(). PartList()) {
                    std::cout << "part"<<
                    ",name:" << part.PartNumber() <<
                    ",size:" << part.Size() <<
                    ",etag:" << part.ETag() <<
                    ",lastmodify time:" << part.LastModified() << std::endl;
                }
            }
            listuploadrequest.setPartNumberMarker(listuploadresult.result(). NextPartNumberMarker());
        } while (listuploadresult.result(). IsTruncated());
        
        /* Release network resources. */
        ShutdownSdk();
        return 0;
    }
    ```

-   List all uploaded parts by page

    Use the following code to specify the maximum number of parts displayed on each page:

    ```
    #include <alibabacloud/oss/OssClient.h>
    using namespace AlibabaCloud::OSS;
    
    int main(void)
    {
        /* Initialize the OSS account information. */
        std::string AccessKeyId = "yourAccessKeyId";
        std::string AccessKeySecret = "yourAccessKeySecret";
        std::string Endpoint = "yourEndpoint";
        std::string BucketName = "yourBucketName";
        std::string ObjectName = "yourObjectName";
    
        /* Initialize network resources. */
        InitializeSdk();
    
        ClientConfiguration conf;
        OssClient client(Endpoint, AccessKeyId, AccessKeySecret, conf);
      
        /* List all uploaded parts by page. */
        /* Set the maximum number of parts that can be uploaded on each page. */
        ListPartsRequest listuploadrequest(BucketName, ObjectName);
        listuploadrequest.setMaxParts(50);
        listuploadrequest.setUploadId(uploadId);
        do {
            listuploadresult = client.ListParts(listuploadrequest);
            if (! listUploadResult.isSuccess()) {
                /* Handle exceptions. */
                std::cout << "ListParts fail" <<
                ",code:" << listuploadresult.error(). Code() <<
                ",message:" << listuploadresult.error(). Message() <<
                ",requestId:" << listuploadresult.error(). RequestId() << std::endl;
                break;
            }
            else {
                for (const auto& part : listuploadresult.result(). PartList()) {
                    std::cout << "part"<<
                    ",name:" << part.PartNumber() <<
                    ",size:" << part.Size() <<
                    ",etag:" << part.ETag() <<
                    ",lastmodify time:" << part.LastModified() << std::endl;
                }
            }  
            listuploadrequest.setPartNumberMarker(listuploadresult.result(). NextPartNumberMarker());     
        } while (listuploadresult.result(). IsTruncated());
    
        /* Release network resources. */
        ShutdownSdk();
        return 0;
    }
    ```


## List multipart upload events {#section_dln_21z_zgb .section}

You can call ListMultipartUploads to list all ongoing multipart upload events that have been initiated but not completed or have been cancelled.

-   List multipart upload events

    **Note:** ListMultipartUploads can be used to list only a maximum of 1,000 multipart upload events simultaneously by default. If the number of events is greater than 1,000, use the following pagination to list all upload events.

    Use the following code to list all multipart upload events:

    ```
    #include <alibabacloud/oss/OssClient.h>
    using namespace AlibabaCloud::OSS;
    
    int main(void)
    {
        /* Initialize the OSS account information. */
        std::string AccessKeyId = "yourAccessKeyId";
        std::string AccessKeySecret = "yourAccessKeySecret";
        std::string Endpoint = "yourEndpoint";
        std::string BucketName = "yourBucketName";
    
        /* Initialize network resources. */
        InitializeSdk();
    
        ClientConfiguration conf;
        OssClient client(Endpoint, AccessKeyId, AccessKeySecret, conf);
      
        /* List uploaded events. A maximum of 1,000 multipart upload events can be listed. */
        ListMultipartUploadsRequest listmultiuploadrequest(BucketName);
        do {
            auto listresult = client.ListMultipartUploads(listmultiuploadrequest);
            if (! listresult.isSuccess()) {
                /* Handle exceptions. */
                std::cout << "ListMultipartUploads fail" <<
                ",code:" << listresult.error(). Code() <<
                ",message:" << listresult.error(). Message() <<
                ",requestId:" << listresult.error(). RequestId() << std::endl;
                break;
            }
            else {
                for (const auto& part : listresult.result(). MultipartUploadList()) {
                    std::cout << "part"<<
                    ",name:" << part.Key <<
                    ",uploadid:" << part.UploadId <<
                    ",initiated time:" << part.Initiated << std::endl;
                }
            }
            listmultiuploadrequest.setKeyMarker(listresult.result(). NextKeyMarker()); 
            listmultiuploadrequest.setUploadIdMarker(listresult.result(). NextUploadIdMarker()); 
        } while (listresult.result(). IsTruncated());
        
        /* Release network resources. */
        ShutdownSdk();
        return 0;
    }
    ```

-   List all upload events by page

    Use the following code to list all upload events by page:

    ```
    #include <alibabacloud/oss/OssClient.h>
    using namespace AlibabaCloud::OSS;
    
    int main(void)
    {
        /* Initialize the OSS account information. */
        std::string AccessKeyId = "yourAccessKeyId";
        std::string AccessKeySecret = "yourAccessKeySecret";
        std::string Endpoint = "yourEndpoint";
        std::string BucketName = "yourBucketName";
    
        /* Initialize network resources. */
        InitializeSdk();
    
        ClientConfiguration conf;
        OssClient client(Endpoint, AccessKeyId, AccessKeySecret, conf);
      
        /* List all upload events by page. */
        /* Set the maximum number of upload events that can be displayed on each page. */
        ListMultipartUploadsRequest  listmultiuploadrequest(BucketName);
        listmultiuploadrequest.setMaxUploads(50);
        do {
            listresult = client.ListMultipartUploads(listmultiuploadrequest);
            if (! listresult.isSuccess()) {
                /* Handle exceptions. */
                std::cout << "ListMultipartUploads fail" <<
                ",code:" << listresult.error(). Code() <<
                ",message:" << listresult.error(). Message() <<
                ",requestId:" << listresult.error(). RequestId() << std::endl;
                break;
            }
            else {
                for (const auto& part : listresult.result(). MultipartUploadList()) {
                    std::cout << "part"<<
                    ",name:" << part.Key <<
                    ",uploadid:" << part.UploadId <<
                    ",initiated time:" << part.Initiated << std::endl;
                }
            }  
            listmultiuploadrequest.setKeyMarker(listresult.result(). NextKeyMarker()); 
            listmultiuploadrequest.setUploadIdMarker(listresult.result(). NextUploadIdMarker()); 
        } while (listresult.result(). IsTruncated());
    
        /* Release network resources. */
        ShutdownSdk();
        return 0;
    }
    ```


## Cancel a multipart upload event {#section_k2y_tzx_kfb .section}

Use the following code to cancel a multipart upload event:

```
#include <alibabacloud/oss/OssClient.h>
using namespace AlibabaCloud::OSS;

int main(void)
{
    /* Initialize the OSS account information. */
    std::string AccessKeyId = "yourAccessKeyId";
    std::string AccessKeySecret = "yourAccessKeySecret";
    std::string Endpoint = "yourEndpoint";
    std::string BucketName = "yourBucketName";
    std::string ObjectName = "yourObjectName";

    /* Initialize network resources. */
    InitializeSdk();

    ClientConfiguration conf;
    OssClient client(Endpoint, AccessKeyId, AccessKeySecret, conf);
    InitiateMultipartUploadRequest initUploadRequest(BucketName, ObjectName);
  
    /* Initiate the multipart upload event. */
    auto uploadIdResult = client.InitiateMultipartUpload(initUploadRequest);
    auto uploadId = uploadIdResult.result(). UploadId();
  
    /* Cancel the multipart upload task. */
    AbortMultipartUploadRequest  abortUploadRequest(BucketName, key, uploadId);
    auto abortUploadIdResult = client.AbortMultipartUpload(abortUploadRequest);
  
    if (! abortUploadIdResult.isSuccess()) {
        /* Handle exceptions. */
        std::cout << "AbortMultipartUpload fail" <<
        ",code:" << outcome.error(). Code() <<
        ",message:" << outcome.error(). Message() <<
        ",requestId:" << outcome.error(). RequestId() << std::endl;
        ShutdownSdk();
        return -1;
    }
  
    /* Release network resources. */
    ShutdownSdk();
    return 0;
}
```

**Note:** After a multipart upload event is cancelled, you cannot use the same upload ID to perform any operations and the parts uploaded with that upload ID are also deleted.

