# Resumable upload {#concept_32063_zh .concept}

This topic describes how to use resumable upload.

In a wireless network environment, it takes a long time to upload a large file. The upload may fail due to poor network connectivity or network switches. In this case, the entire file needs to be uploaded again. To address this problem, the SDK provides resumable upload.

**Note:** 

-   Currently, this feature allows you to upload only local files.
-   Resumable upload is inapplicable to small- and medium-sized files from a mobile device. Resumable upload adopts the multipart upload method. This method is inefficient because it initiates multiple network requests for uploading a single file.

Before you upload a file, you can specify the folder for the file that stores resumable upload records. Resumable upload takes effect only for the current upload task. If you do not specify the folder for the file that stores resumable upload records, a part of a large file may fail to be uploaded due to network errors. To upload the entire file, large amounts of time and traffic are required to make retry attempts. If you specify the folder for the file that stores resumable upload records and the upload fails, the reupload starts from the recorded position. For more information, see the subsequent example.

When a resumable upload task fails and is not resumed for a long time, the uploaded parts may become unnecessary in OSS. In this case, you can set the lifecycle rule for the bucket to clear parts periodically. For more information, see [Lifecycle management](../../../../reseller.en-US/Console User Guide/Manage buckets/Set lifecycle.md#).

To manage parts, if the current task is cancelled during resumable upload, the parts already uploaded to the server are cleared synchronously by default. If you want to save the resumable upload record, you must specify a folder of a file that stores the resumable upload record and modify the deleteUploadIdOnCancelling parameter. If the resumable upload record is saved on the local device for a long time but the parts uploaded to the server are deleted based on the lifecycle rule configured for the bucket, the records on the server and the mobile device may be inconsistent.

**Note:** 

-   Resumable upload depends on `InitMultipartUpload/UploadPart/ListParts/CompleteMultipartUpload/AbortMultipartUpload`. If you use the STS authentication mode, add the permissions required to use the APIs.
-   Resumable upload supports post-upload callback notifications that can be used in the same way as the preceding typical upload callback notifications.
-   MD5 verification is automatically enabled for each multipart upload task during resumable upload. You do not need to set the `Content-Md5` field in the request header.

-   The invocation method \(not specified by default\) to store resumable upload records permanently in the local device:

    ```language-objc
    OSSResumableUploadRequest * resumableUpload = [OSSResumableUploadRequest new];
    resumableUpload.bucketName = OSS_BUCKET_PRIVATE;
    //...
    NSString *cachesDir = [NSSearchPathForDirectoriesInDomains(NSCachesDirectory, NSUserDomainMask, YES) firstObject];
    resumableUpload.recordDirectoryPath = cachesDir;
    
    ```

-   Implementation of resumable upload

    ```language-objc
    // Obtain an upload ID to upload the file. If the task fails and can be resumed, you can use the same upload ID to upload the same file to the same bucket.
    OSSResumableUploadRequest * resumableUpload = [OSSResumableUploadRequest new];
    resumableUpload.bucketName = <bucketName>;
    resumableUpload.objectKey = <objectKey>;
    resumableUpload.partSize = 1024 * 1024;
    resumableUpload.uploadProgress = ^(int64_t bytesSent, int64_t totalByteSent, int64_t totalBytesExpectedToSend) {
        NSLog(@"%lld, %lld, %lld", bytesSent, totalByteSent, totalBytesExpectedToSend);
    };
    NSString *cachesDir = [NSSearchPathForDirectoriesInDomains(NSCachesDirectory, NSUserDomainMask, YES) firstObject];
    //Configure the checkpoint file.
    resumableUpload.recordDirectoryPath = cachesDir;
    //Set the resumableUpload.deleteUploadIdOnCancelling field to No. When a task is cancelled, the checkpoint file is not deleted. If you retain default value Yes, the checkpoint file is deleted when a task is cancelled, and the entire file needs to be uploaded again when the task restarts.
    resumableUpload.deleteUploadIdOnCancelling = NO;
    
    resumableUpload.uploadingFileURL = [NSURL fileURLWithPath:<your file path>];
    OSSTask * resumeTask = [client resumableUpload:resumableUpload];
    [resumeTask continueWithBlock:^id(OSSTask *task) {
        if (task.error) {
            NSLog(@"error: %@", task.error);
            if ([task.error.domain isEqualToString:OSSClientErrorDomain] && task.error.code == OSSClientErrorCodeCannotResumeUpload) {
                // The task cannot be resumed. You must obtain a new upload ID to upload the file.
            }
        } else {
            NSLog(@"Upload file success");
        }
        return nil;
    }];
    
    // [resumeTask waitUntilFinished];
    
    // [resumableUpload cancel];
    
    ```


