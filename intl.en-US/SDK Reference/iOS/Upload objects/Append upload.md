# Append upload {#concept_svd_kfg_4fb .concept}

This topic describes how to use append upload.

You can use AppendObject to append objects. The object that is created by using AppendObject is an Appendable object, while the object that is uploaded by using PutObject is a Normal object.

Use the following code for append upload:

```
OSSAppendObjectRequest * append = [OSSAppendObjectRequest new];
// Configure required fields.
append.bucketName = @"<bucketName>";
append.objectKey = @"<objectKey>";
append.appendPosition = 0; // Specify the starting position to append the object.
NSString * docDir = [self getDocumentDirectory];
append.uploadingFileURL = [NSURL fileURLWithPath:@"<filepath>"];
// Configure optional fields.
append.uploadProgress = ^(int64_t bytesSent, int64_t totalByteSent, int64_t totalBytesExpectedToSend) {
    NSLog(@"%lld, %lld, %lld", bytesSent, totalByteSent, totalBytesExpectedToSend);
};
// For more information about the descriptions of optional fields, see https://docs.aliyun.com/#/pub/oss/api-reference/object&AppendObject.
// append.contentType = @"";
// append.contentMd5 = @"";
// append.contentEncoding = @"";
// append.contentDisposition = @"";
OSSTask * appendTask = [client appendObject:append];
[appendTask continueWithBlock:^id(OSSTask *task) {
    NSLog(@"objectKey: %@", append.objectKey);
    if (! task.error) {
        NSLog(@"append object success!") ;
        OSSAppendObjectResult * result = task.result;
        NSString * etag = result.eTag;
        long nextPosition = result.xOssNextAppendPosition;
    } else {
        NSLog(@"append object failed, error: %@" , task.error);
    }
    return nil;
}];
```

