# Upload callback {#concept_crq_tfg_4fb .concept}

This topic describes how to use upload callback.

When an upload is complete, the OSS instance can send a callback to the application server. To implement the callback, you need only to add the relevant callback parameters to the request that is sent to OSS. Compared with simple upload, the upload callback notification mechanism requires the client to wait for a longer time to process a callback request and response.

For more information, see [Callback](../../../../reseller.en-US/API Reference/Object operations/Callback.md#).

Use the following code for upload callback:

```
OSSPutObjectRequest * request = [OSSPutObjectRequest new];
request.bucketName = @"<bucketName>";
request.objectKey = @"<objectKey>";
request.uploadingFileURL = [NSURL fileURLWithPath:@<filepath>"];
// Set callback parameters.
request.callbackParam = @{
                          @"callbackUrl": @"<your server callback address>",
                          @"callbackBody": @"<your callback body>"
                          };
// Set custom variables.
request.callbackVar = @{
                        @"<var1>": @"<value1>",
                        @"<var2>": @"<value2>"
                        };
request.uploadProgress = ^(int64_t bytesSent, int64_t totalByteSent, int64_t totalBytesExpectedToSend) {
    NSLog(@"%lld, %lld, %lld", bytesSent, totalByteSent, totalBytesExpectedToSend);
};
OSSTask * task = [client putObject:request];
[task continueWithBlock:^id(OSSTask *task) {
    if (task.error) {
        OSSLogError(@"%@", task.error);
    } else {
        OSSPutObjectResult * result = task.result;
        NSLog(@"Result - requestId: %@, headerFields: %@, servercallback: %@",
              result.requestId,
              result.httpResponseHeaderFields,
              result.serverReturnJsonString);
    }
    return nil;
}];
```

