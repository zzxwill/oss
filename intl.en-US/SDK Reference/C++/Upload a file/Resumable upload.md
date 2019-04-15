# Resumable upload {#concept_dzd_b3n_vgb .concept}

You can use resumable upload to split a file into several parts and upload them separately. After you have uploaded all parts, you can combine these parts into a complete object. In this way, you can complete uploading the entire object.

For more information about resumable upload, see the "[Resumable upload](../../../../../reseller.en-US/Developer Guide/Upload files/Multipart upload.md#)" section in OSS Developer Guide. For the complete code, see [GitHub](https://github.com/aliyun/aliyun-oss-cpp-sdk/blob/f7ef0efa45e17da815020a18919551973cc98089/sample/src/object/ObjectSample.cc#L318).

You can use OSSClient. ResumableUploadObject to implement resumable upload. The following table describes the parameters contained in UploadObjectRequest.

|Name|Description|Required|Default value|Configuration method|
|:---|:----------|:-------|:------------|:-------------------|
|bucket|The name of the bucket.|Yes|None|Constructor|
|key|The name of the object.|Yes|None|Constructor|
|filePath|The path to the local file. The file located in this path is uploaded to OSS.|No|The path to the local file|Constructor|
|partSize|The size of each part. Valid values: \[100 KB, 5 GB\].|No|8 MB|setPartSize|
|threadNum|The number of parts you need to upload simultaneously.|No|3|Constructor or setThreadNum|
|checkpointDir|The directory that records the upload results of each local part. This parameter must be configured to enable resumable upload. This file stores information about upload progress. If you fail to upload a part, the part upload continues based on the recorded progress. After the entire local file is uploaded, this file is deleted.|No|The same directory as that of DownloadFile|Constructor or setCheckpointDir|

Use the following code for resumable upload:

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
    std::string UploadFilePath = "yourUploadfilePath";
    std::string CheckpointFilePath = "yourCheckpointFilepath"
 
    /* Initialize network resources. */
    InitializeSdk();

    ClientConfiguration conf;
    OssClient client(Endpoint, AccessKeyId, AccessKeySecret, conf);
 
    /* Start resumable upload. */
    UploadObjectRequest request(BucketName, ObjectName, UploadFilePath, CheckpointFilePath);
    auto outcome = client.ResumableUploadObject(request);
  
    if (! outcome.isSuccess()) {
        /* Handle exceptions. */
        std::cout << "ResumableUploadObject fail" <<
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

