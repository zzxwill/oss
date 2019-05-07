# Data security {#concept_cmk_hgg_4fb .concept}

The iOS SDK provides data integrity checks to secure data during uploads and downloads.

## Data integrity checks during file uploads {#section_plw_3gg_4fb .section}

Due to the complexities in mobile network environments, an error can occur when data is transmitted between the client and the server. The OSS iOS SDK provides end-to-end data integrity checks based on MD5 and CRC64 values.

-   MD5 verification

    You must provide the Content-MD5 value of the file during the upload. The OSS server performs the MD5 verification after the file is uploaded. The upload can succeed only when the MD5 value of the file received by the OSS server is equal to the MD5 value provided before the upload. In this way, the integrity of uploaded data is guaranteed.

    ```language-java
    OSSPutObjectRequest * request = [OSSPutObjectRequest new];
    request.bucketName = BUCKET_NAME;
    ...
    request.contentMd5 = [OSSUtil fileMD5String:filepath];
    
    ```

-   CRC

    Compared with MD5 values, CRC64 values can be calculated during file uploads.

    ```
    // Construct an upload request.
    OSSPutObjectRequest * request = [OSSPutObjectRequest new];
    request.bucketName = OSS_BUCKET_PRIVATE;
    ///....
    request.crcFlag = OSSRequestCRCOpen;
    // After the CRC is enabled, If data inconsistency is found during the upload, OSSClientErrorCodeInvalidCRC is thrown.
    OSSTask * task = [_client putObject:request];
    [[task continueWithBlock:^id(OSSTask *task) {
        // If the CRC fails, an error is reported.
        XCTAssertNil(task.error);
        return nil;
    }] waitUntilFinished];
    ```


## Data integrity checks during file downloads {#section_vbh_23g_4fb .section}

The OSS iOS SDK provides end-to-end data integrity checks based on CRC64 values.

When you read a downloaded data stream, the data integrity check is automatically performed if the CRC is enabled.

Use the following code to enable the CRC:

```language-java
OSSGetObjectRequest * request = [OSSGetObjectRequest new];
request.bucketName = ... ;
// Enable the CRC.
request.crcFlag = OSSRequestCRCOpen;

OSSTask * task = [testProxyClient getObject:request];

[[task continueWithBlock:^id(OSSTask *task) {
	// If the CRC is enabled and a data error occurs during transmission, OSSClientErrorCodeInvalidCRC is thrown.
    XCTAssertNil(task.error);
    return nil;
}] waitUntilFinished];

Note: If the onReceiveData block field is set, you must check the consistency of CRC64 values after the CRC is enabled. Example:
OSSGetObjectRequest * request = [OSSGetObjectRequest new];
request.bucketName = ....
request.crcFlag = OSSRequestCRCOpen;
....
    
__block uint64_t localCrc64 = 0;
	// If the onReceiveData block field is set,
NSMutableData *receivedData = [NSMutableData data];
request.onRecieveData = ^(NSData *data) {
	if (data)
	{
		NSMutableData *mutableData = [data mutableCopy];
    	void *bytes = mutableData.mutableBytes;
    	localCrc64 = [OSSUtil crc64ecma:localCrc64 buffer:bytes length:data.length];
    	[receivedData appendData:data];
    }
};
    
__block uint64_t remoteCrc64 = 0;
OSSTask * task = [_client getObject:request];
[[task continueWithBlock:^id(OSSTask *task) {
	XCTAssertNil(task.error);
   	OSSGetObjectResult *result = task.result;
    if (result.remoteCRC64ecma) 
	{
    	NSScanner *scanner = [NSScanner scannerWithString:result.remoteCRC64ecma];
		[scanner scanUnsignedLongLong:&remoteCrc64];
        if (remoteCrc64 == localCrc64)
        {
        	NSLog(@"CRC64 verification succeeded!"); ;
        }
		else
        {
        	NSLog(@"CRC64 verification failed!"); ;
        }
   }
   return nil;
}] waitUntilFinished];

```

