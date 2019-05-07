# Multipart upload {#concept_ch3_ffg_4fb .concept}

This topic describes how to use multipart upload.

## Initialize a multipart upload task {#section_ztm_gcj_lfb .section}

Use the following code to initialize a multipart upload task:

```
__block NSString * uploadId = nil;
__block NSMutableArray * partInfos = [NSMutableArray new];
NSString * uploadToBucket = @"<bucketName>";
NSString * uploadObjectkey = @"<objectKey>";
OSSInitMultipartUploadRequest * init = [OSSInitMultipartUploadRequest new];
init.bucketName = uploadToBucket;
init.objectKey = uploadObjectkey;
// init.contentType = @"application/octet-stream";
OSSTask * initTask = [client multipartUploadInit:init];
[initTask waitUntilFinished];
if (! initTask.error) {
    OSSInitMultipartUploadResult * result = initTask.result;
    uploadId = result.uploadId;
} else {
    NSLog(@"multipart upload failed, error: %@", initTask.error);
    return;
}
```

**Note:** 

-   OSSInitMultipartUploadRequest is used to specify the name of the object you want to upload and the name of the bucket to which the object belongs.
-   The response to multipartUploadInit contains the upload ID. The upload ID is the unique identifier of the multipart upload task.

## Upload parts {#section_xcc_mgg_4fb .section}

Use the following code to upload parts:

```language-objc
NSString * filePath = [docDir stringByAppendingPathComponent:@"***"];
//The object you want to upload.
int chuckCount = *;
//The number of parts.
uint64_t offset = fileSie/chuckCount;
//The size of each part.
for (int i = 1; i <= chuckCount; i++) {
	OSSUploadPartRequest * uploadPart = [OSSUploadPartRequest new];
	uploadPart.bucketName = uploadToBucket;
	uploadPart.objectkey = uploadObjectkey;
	uploadPart.uploadId = uploadId;
	uploadPart.partNumber = i; // part number start from 1

	NSFileHandle* readHandle = [NSFileHandle fileHandleForReadingAtPath:filePath];
        [readHandle seekToFileOffset:offset * (i -1)];
        
        NSData* data = [readHandle readDataOfLength:offset];
        uploadPart.uploadPartData = data;

	OSSTask * uploadPartTask = [client uploadPart:uploadPart];

	[uploadPartTask waitUntilFinished];

	if (! uploadPartTask.error) {
		OSSUploadPartResult * result = uploadPartTask.result;
		uint64_t fileSize = [[[NSFileManager defaultManager] attributesOfItemAtPath:uploadPart.uploadPartFileURL.absoluteString error:nil] fileSize];
		[partInfos addObject:[OSSPartInfo partInfoWithPartNum:i eTag:result.eTag size:fileSize]];
	} else {
		NSLog(@"upload part error: %@", uploadPartTask.error);
		return;
	}
}

```

The preceding code uses uploadPart to upload each part.

**Note:** 

-   An upload ID and a part number must be specified for each multipart upload request.
-   If you use uploadPart, each part must be larger than 100 KB aside from the last part. However, the UploadPart API does not check the uploaded part size immediately. This API checks the part size only when multipart upload is complete.
-   The part number ranges from 1 to 10,000. If a part number is beyond this range, OSS returns the InvalidArgument error code.
-   When each part is uploaded, the stream is directed to the starting position of the uploaded part.
-   After each part is uploaded, OSS returns a response that contains the ETag value of the part. If the ETag value and the MD5 value of the part are the same, you must combine the ETag value with the part number into PartETag and save the PartETag for subsequent upload of parts.

## Complete the multipart upload {#section_bdc_mgg_4fb .section}

In the following code, the partInfos field value indicates the list of PartETags saved during the multipart upload. OSS verifies the validity of each part after OSS receives the partInfos field value. When all data parts are verified to be valid, OSS combines them into a complete object.

```language-objc
OSSCompleteMultipartUploadRequest * complete = [OSSCompleteMultipartUploadRequest new];
complete.bucketName = uploadToBucket;
complete.objectKey = uploadObjectkey;
complete.uploadId = uploadId;
complete.partInfos = partInfos;

OSSTask * completeTask = [client completeMultipartUpload:complete];

[[completeTask continueWithBlock:^id(OSSTask *task) {
	if (! task.error) {
		OSSCompleteMultipartUploadResult * result = task.result;
		// ...
	} else {
		// ...
	}
	return nil;
}] waitUntilFinished];

```

You can set the Server Callback parameter to complete the multipart upload request. A callback request is sent to the specified server address after the multipart upload task is complete. You can view the servercallback result in result.serverReturnJsonString of the response.

```
OSSCompleteMultipartUploadRequest * complete = [OSSCompleteMultipartUploadRequest new];
complete.bucketName = @"<bucketName>"; 
complete.objectKey = @"<objectKey>";
complete.uploadId = uploadId;
complete.partInfos = partInfos;
complete.callbackParam = @{
                          @"callbackUrl": @"<server address>",
                          @"callbackBody": @"<test>"
                          };
complete.callbackVar = @{
                        @"var1": @"value1",
                        @"var2": @"value2"
                        };
OSSTask * completeTask = [client completeMultipartUpload:complete];
[[completeTask continueWithBlock:^id(OSSTask *task) {
    if (! task.error) {
        OSSCompleteMultipartUploadResult * result = task.result;
        NSLog(@"server call back return : %@", result.serverReturnJsonString);
    } else {
        // ...
    }
    return nil;
}] waitUntilFinished];
```

**Note:** To check whether the object uploaded to OSS is consistent with the local file, attach the Content-MD5 field value to the upload request. The OSS server helps you with the MD5 verification. The upload can succeed only when the MD5 value of the object received by the OSS server is the same as the Content-MD5 field value. This method can ensure the consistency of uploaded object.

Use the following code to configure MD5 verification:

```language-java
OSSUploadPartRequest * uploadPart = [OSSUploadPartRequest new];
uploadPart.bucketName = TEST_BUCKET;
uploadPart.uploadId = uploadId;
....
uploadPart.contentMd5 = [OSSUtil fileMD5String:filepath];

```

## List parts {#section_fdc_mgg_4fb .section}

Use the following code to call listParts to obtain all uploaded parts of an upload event:

```language-objc
OSSListPartsRequest * listParts = [OSSListPartsRequest new];
listParts.bucketName = @"<bucketName>";
listParts.objectKey = @"<objectkey>";
listParts.uploadId = @"<uploadid>";

OSSTask * listPartTask = [client listParts:listParts];

[listPartTask continueWithBlock:^id(OSSTask *task) {
	if (! task.error) {
		NSLog(@"list part result success!") ;
		OSSListPartsResult * listPartResult = task.result;
		for (NSDictionary * partInfo in listPartResult.parts) {
			NSLog(@"each part: %@", partInfo);
		}
	} else {
		NSLog(@"list part result error: %@", task.error);
	}
	return nil;
}];

```

**Note:** If a bucket contains more than 1,000 parts that are uploaded by using multipart upload, OSS returns the first 1,000 parts by default. In the response, the IsTruncated field is set to false and NextPartNumberMarker is returned to indicate the next starting position to continue reading data.

## Cancel a multipart upload event {#section_ddc_mgg_4fb .section}

Use the following code to cancel a multipart upload request that is mapped to the specified upload ID:

```language-objc
OSSAbortMultipartUploadRequest * abort = [OSSAbortMultipartUploadRequest new];
abort.bucketName = @"<bucketName>";
abort.objectKey = @"<objectKey>";
abort.uploadId = uploadId;

OSSTask * abortTask = [client abortMultipartUpload:abort];

[abortTask waitUntilFinished];

if (! abortTask.error) {
	OSSAbortMultipartUploadResult * result = abortTask.result;
	uploadId = result.uploadId;
} else {
	NSLog(@"multipart upload failed, error: %@", abortTask.error);
	return;
}

```

