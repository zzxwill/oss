# Quick start {#concept_32058_zh .concept}

This topic describes how to get started with the iOS SDK.

For more information, see the following examples and cases of this project:

-   [iOS demo](https://github.com/aliyun/aliyun-oss-ios-sdk/tree/master/Example/AliyunOSSSDK-iOS-Example) 
-   [Mac demo](https://github.com/aliyun/aliyun-oss-ios-sdk/tree/master/Example/AliyunOSSSDK-OSX-Example) 
-   [Swift demo](https://github.com/aliyun/aliyun-oss-ios-sdk/tree/master/OSSSwiftDemo) 
-   [Test cases](https://github.com/aliyun/AliyunOSSiOS/tree/master/AliyunOSSiOSTests) 

You can also directly run the git clone [https://github.com/aliyun/aliyun-oss-ios-sdk](https://github.com/aliyun/aliyun-oss-ios-sdk) command and configure the following parameters:

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/22537/155719516713693_en-US.png)

Run the project demo as follows:

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/22537/155719516713694_en-US.png)

## Examples {#section_nxk_spj_lfb .section}

The following code shows you how to upload and download files on the iOS and Mac platforms.

-   iOS platform
    1.  Add the reference.

        ```language-objc
        #import <AliyunOSSiOS/OSSService.h>
        
        ```

    2.  Initialize the OSSClient instance.

        To initialize an OSSClient instance, you must configure the Endpoint field, authentication mode, and Client parameters. Three authentication modes are available: plaintext setting, self-signed, and STS authentication. For more information about how to use the STS authentication mode and learn about the basic knowledge of RAM, see [Authorized access](reseller.en-US/SDK Reference/iOS/Authorized access.md#). The following code assumes that you have activated RAM and have basic knowledge of it, such as how to obtain the AccessKey ID, AccessKey Secret, and role ID of a RAM user.

        Set AccessKeyId，SecretKeyId, and RoleArn parameters, as shown in [Script files](https://github.com/aliyun/aliyun-oss-android-sdk/blob/master/app/sts_local_server/python/sts.py). You can use the Python SDK to start a local HTTP service. Use the client code to access the local service and obtain the StsToken.AccessKeyId, StsToken.SecretKeyId, and StsToken.SecurityToken field values.

        For more information, see the "[STS instructions](https://github.com/aliyun/aliyun-oss-ios-sdk/tree/master/Example)" section in the sample.

        ```language-objc
        NSString *endpoint = "https://oss-cn-hangzhou.aliyuncs.com";
        
        // We recommend that you use STS to initialize the OSSClient instance if you use the mobile device. For more information, see the "STS instructions" section in the sample, as shown in https://github.com/aliyun/aliyun-oss-ios-sdk/tree/master/DemoByOC.
        id<OSSCredentialProvider> credential = [[OSSStsTokenCredentialProvider alloc] initWithAccessKeyId:@"AccessKeyId" secretKeyId:@"AccessKeySecret" securityToken:@"SecurityToken"];
        
        client = [[OSSClient alloc] initWithEndpoint:endpoint credentialProvider:credential];
        
        
        ```

        The use of the OSSClient instance to initiate upload and download requests is thread-safe. You can run multiple tasks concurrently.

    3.  Upload a file.

        Assume that you have created a bucket in the console. An `OSSTask` is returned for each SDK-relevant operation. You can configure a continuation for the task to be completed asynchronously or call `waitUntilFinished` to wait for the task to be complete.

        ```
        OSSPutObjectRequest * put = [OSSPutObjectRequest new];
        put.bucketName = @"<bucketName>";
        put.objectKey = @"<objectKey>";
        put.uploadingData = <NSData *>; // Directly upload NSData.
        put.uploadProgress = ^(int64_t bytesSent, int64_t totalByteSent, int64_t totalBytesExpectedToSend) {
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
        // Wait until the task is complete.
        // [putTask waitUntilFinished];
        ```

    4.  Download the specified object.

        Use the following code to download the specified object as `NSData`:

        ```
        OSSGetObjectRequest * request = [OSSGetObjectRequest new];
        request.bucketName = @"<bucketName>";
        request.objectKey = @"<objectKey>";
        request.downloadProgress = ^(int64_t bytesWritten, int64_t totalBytesWritten, int64_t totalBytesExpectedToWrite) {
            NSLog(@"%lld, %lld, %lld", bytesWritten, totalBytesWritten, totalBytesExpectedToWrite);
        };
        OSSTask * getTask = [client getObject:request];
        [getTask continueWithBlock:^id(OSSTask *task) {
            if (! task.error) {
                NSLog(@"download object success!") ;
                OSSGetObjectResult * getResult = task.result;
                NSLog(@"download result: %@", getResult.downloadedData);
            } else {
                NSLog(@"download object failed, error: %@" ,task.error);
            }
            return nil;
        }];
        // Wait until the task is complete.
        // [task waitUntilFinished];
        ```

-   MAC platform

    Aside from the import method, all operations on the MAC platform are the same as those on the iOS platform.

    ```language-objc
    import <AliyunOSSOSX/AliyunOSSiOS.h>
    
    ```


