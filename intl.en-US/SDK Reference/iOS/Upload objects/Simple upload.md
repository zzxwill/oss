# Simple upload {#concept_vp1_tbg_4fb .concept}

This topic describes how to use simple upload.

## Upload a file from memory or upload a local file {#section_bg5_g5j_lfb .section}

You can upload OSSData directly or use NSURL to upload a file:

```
OSSPutObjectRequest * put = [OSSPutObjectRequest new];
// Configure required fields.
put.bucketName = @"<bucketName>";
put.objectKey = @"<objectKey>";
put.uploadingFileURL = [NSURL fileURLWithPath:@"<filepath>"];
// put.uploadingData = <NSData *>; // Directly upload NSData.
// Configure optional fields.
put.uploadProgress = ^(int64_t bytesSent, int64_t totalByteSent, int64_t totalBytesExpectedToSend) {
    // The length of the segment being uploaded, the total length of the segments uploaded, and the total length of the segments to be uploaded.
    NSLog(@"%lld, %lld, %lld", bytesSent, totalByteSent, totalBytesExpectedToSend);
};
// For more information about the descriptions of optional fields, see https://docs.aliyun.com/#/pub/oss/api-reference/object&PutObject.
// put.contentType = @"";
// put.contentMd5 = @"";
// put.contentEncoding = @"";
// put.contentDisposition = @"";
// put.objectMeta = [NSMutableDictionary dictionaryWithObjectsAndKeys:@"value1", @"x-oss-meta-name1", nil]; // You can configure Object Meta or other HTTP headers for the upload.
OSSTask * putTask = [client putObject:put];
[putTask continueWithBlock:^id(OSSTask *task) {
    if (! task.error) {
        NSLog(@"upload object success!") ;
    } else {
        NSLog(@"upload object failed, error: %@" , task.error);
    }
    return nil;
}];
// [putTask waitUntilFinished];
// [put cancel];
```

## Upload a file to a directory {#section_wbq_lqy_lfb .section}

OSS does not use folders. All elements are stored as objects. To simulate a folder in the OSS console, you can create an object whose name ends with a forward slash \(/\). This object can be uploaded and downloaded. The OSS console displays an object whose name ends with a forward slash \(/\) as a folder.

When you upload a file, if you write ObjectKey as `"folder/subfolder/file"`, you are simulating the upload of the file to the `file` object in the `folder/subfolder/` directory. Note that the default path is the root directory, and does not need to start with a forward slash \(/\).

## Configure the Content-Type field and enable MD5 verification during an upload {#section_irf_nqy_lfb .section}

You can explicitly specify the Content-Type field during the upload. If it is not specified, the SDK automatically determines its value based on the file name or the uploaded ObjectKey field value. Additionally, if the Content-MD5 field is set when an object is uploaded, OSS uses the Content-MD5 field value to check whether the received object content is consistent with the sent object content. The SDK provides methods to calculate the Base64 and MD5 values.

```
OSSPutObjectRequest * put = [OSSPutObjectRequest new];
// Configure required fields.
put.bucketName = @"<bucketName>";
put.objectKey = @"<objectKey>";
put.uploadingFileURL = [NSURL fileURLWithPath:@"<filepath>"];
// put.uploadingData = <NSData *>; // Directly upload NSData.
// Optional. Set the Content-Type field.
put.contentType = @"application/octet-stream";
// Optional. Set the MD5 verification.
put.contentMd5 = [OSSUtil base64Md5ForFilePath:@"<filePath>"]; // Set the MD5 value of the file path.
// put.contentMd5 = [OSSUtil base64Md5ForData:<NSData *>]; // Set the MD5 value for the binary data.
// Optional. Set the upload progress.
put.uploadProgress = ^(int64_t bytesSent, int64_t totalByteSent, int64_t totalBytesExpectedToSend) {
    // The length of the segment being uploaded, the total length of the segments uploaded, and the total length of the segments to be uploaded.
    NSLog(@"%lld, %lld, %lld", bytesSent, totalByteSent, totalBytesExpectedToSend);
};
OSSTask * putTask = [client putObject:put];
[putTask continueWithBlock:^id(OSSTask *task) {
    if (! task.error) {
        NSLog(@"upload object success!") ;
    } else {
        NSLog(@"upload object failed, error: %@" , task.error);
    }
    return nil;
}];
// [putTask waitUntilFinished];
// [put cancel];
```

