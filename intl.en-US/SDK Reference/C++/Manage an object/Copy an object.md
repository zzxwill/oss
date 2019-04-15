# Copy an object {#concept_fbl_yql_ngb .concept}

OSS allows you to copy objects of any sizes.

## Copy a small object {#section_acv_tnz_xgb .section}

You can use OssClient::CopyObject to copy an object smaller than 1 GB from a bucket \(source bucket\) to another bucket \(destination bucket\) within the same region. The configuration method is as follows:

|Configuration method|Description|
|:-------------------|:----------|
|CopyObjectOutcome OssClient::CopyObject\(const CopyObjectRequest &request\)|Allows you to specify destination Object Meta and copy conditions. If the URLs of the source object and destination object for the COPY operation are the same, the source Object Meta is directly replaced.|

**Note:** Object copy rules are as follows:

-   You must have read and write permissions on the source object.
-   You cannot copy the object from a region to another region. For example, you cannot copy an object in a bucket from China \(Hangzhou\) to China \(Qingdao\).
-   The object size is limited to a maximum of 1 GB.

Use the following code to copy a small object:

```
#include <alibabacloud/oss/OssClient.h>
using namespace AlibabaCloud::OSS;

int main(void)
{
     /* Initialize the OSS account information. */
    std::string AccessKeyId = "yourAccessKeyId";
    std::string AccessKeySecret = "yourAccessKeySecret";
    std::string Endpoint = "yourEndpoint";
    std::string SourceBucketName = "yourSourceBucketName";
    std::string CopyBucketName = "yourCopyBucketName";
    std::string SourceObjectName = "yourSourceObjectName";
    std::string CopyObjectName = "yourCopyObjectName";

    /* Initialize network resources. */
    InitializeSdk();

    ClientConfiguration conf;
    OssClient client(Endpoint, AccessKeyId, AccessKeySecret, conf);

    CopyObjectRequest request(CopyBucketName, CopyObjectName);
    request.setCopySource(SourceBucketName, SourceObjectName);

    /* Copy the object. */
    auto outcome = client.CopyObject(request);

    if (! outcome.isSuccess()) {
        /* Handle exceptions. */
        std::cout << "CopyObject fail" <<
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

## Copy a large object {#section_rnf_xnz_xgb .section}

To copy an object larger than 1 GB, you must use UploadPartCopy. In other words, you must split the object into multiple parts and copy each of them sequentially. To enable UploadPartCopy, perform the following steps: Use ossClient.initiateMultipartUpload to initiate an UploadPartCopy task. Use ossClient.uploadPartCopy to perform the multipart copy task. Aside from the last part, all parts must be larger than 100 KB. Use $ossClient-\>completeMultipartUpload to submit the multipart copy task.

**Note:** Large objects cannot be copied from one region to another region. For example, you cannot copy an object in a bucket from China \(Hangzhou\) to China \(Qingdao\).

Use the following code to copy a large object:

```
#include <alibabacloud/oss/OssClient.h>
using namespace AlibabaCloud::OSS;

int main(void)
{
    /* Initialize the OSS account information. */
    std::string AccessKeyId = "yourAccessKeyId";
    std::string AccessKeySecret = "yourAccessKeySecret";
    std::string Endpoint = "yourEndpoint";
    std::string SourceBucketName = "yourSourceBucketName";
    std::string CopyBucketName = "yourCopyBucketName";
    std::string SourceObjectName = "yourSourceObjectName";
    std::string CopyObjectName = "yourCopyObjectName";

    /* Initialize network resources. */
    InitializeSdk();

    ClientConfiguration conf;
    OssClient client(Endpoint, AccessKeyId, AccessKeySecret, conf);
  
    auto getObjectMetaReq = GetObjectMetaRequest(SourceBucketName, SourceObjectName);
    auto getObjectMetaResult = GetObjectMeta(getObjectMetaReq);
    if (! getObjectMetaResult.isSuccess()) {
        std::cout << "GetObjectMeta fail" <<
        ",code:" << outcome.error(). Code() <<
        ",message:" << outcome.error(). Message() <<
        ",requestId:" << outcome.error(). RequestId() << std::endl;
        ShutdownSdk();
        return -1;
    }
    /* Obtain the size of the object you want to copy. */
    auto objectSize = getObjectMetaResult.result(). ContentLength();

    /* Copy the large object. */
    InitiateMultipartUploadRequest initUploadRequest(CopyBucketName, CopyObjectName);
  
    /* Initiate an UploadPartCopy task. */
    auto uploadIdResult = client.InitiateMultipartUpload(initUploadRequest);
    auto uploadId = uploadIdResult.result(). UploadId();
    int64_t partSize = 100 * 1024;
    PartList partETagList;
    int partCount = static_cast<int>(objectSize / partSize);
    /* Calculate the number of parts.*/
    if (objectSize % part Size ! = 0) {
        partCount++;
    }
  
    /* Copy each part sequentially. */
    for (int i = 1; i <= partCount; i++) {
        auto skipBytes = partSize * (i - 1);
        auto size = (partSize < objectSize - skipBytes) ? partSize : (objectSize - skipBytes);
        auto uploadPartCopyReq = UploadPartCopyRequest(CopyBucketName, CopyObjectName, SourceBucketName, SourceObjectName,uploadId, i + 1);
        uploadPartCopyReq.setCopySourceRange(skipBytes, skipBytes + size -1);
        auto uploadPartOutcome = client.UploadPartCopy(uploadPartCopyReq);
        if (uploadPartOutcome.isSuccess()) {
            Part part(i, uploadPartOutcome.result(). ETag());
            partETagList.push_back(part);
        }
        else {
            std::cout << "UploadPartCopy fail" <<
            ",code:" << outcome.error(). Code() <<
            ",message:" << outcome.error(). Message() <<
            ",requestId:" << outcome.error(). RequestId() << std::endl;
        }

    }
    
    /* Complete the multipart copy task. */
    CompleteMultipartUploadRequest request(CopyBucketName, CopyObjectName, partETagList, uploadId);
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

