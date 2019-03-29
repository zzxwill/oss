# Simple upload {#concept_bts_m54_mfb .concept}

This topic describes how to upload a file in the simple upload method.

## Upload a local file {#section_dtn_fv4_mfb .section}

-   Upload a local file by calling the synchronous interface:

    ```
    // Construct an upload request.
    PutObjectRequest put = new PutObjectRequest("<bucketName>", "<objectName>", "<uploadFilePath>");
    
    // The setting of the object metadata is optional.
    // ObjectMetadata metadata = new ObjectMetadata();
    // metadata.setContentType("application/octet-stream"); // Specify the content-type.
    // metadata.setContentMD5(BinaryUtil.calculateBase64Md5(uploadFilePath)); // Verify the MD5 value.
    // put.setMetadata(metadata);
    
    try {
        PutObjectResult putResult = oss.putObject(put);
    
        Log.d("PutObject", "UploadSuccess");
        Log.d("ETag", putResult.getETag());
        Log.d("RequestId", putResult.getRequestId());
    } catch (ClientException e) {
        // A local exception (such as network exception) occurs.
        e.printStackTrace();
    } catch (ServiceException e) {
        // A service error occurs.
        Log.e("RequestId", e.getRequestId());
        Log.e("ErrorCode", e.getErrorCode());
        Log.e("HostId", e.getHostId());
        Log.e("RawMessage", e.getRawMessage());
    }
    ```

    **Note:** In OSS Android SDK, you can call the synchronous interface only in child threads but not UI treads. Otherwise, an exception occurs. If you want to upload the file in a UI tread, call the asynchronous interface.

-   Upload a local file by calling the asynchronous interface:

    ```
    // Construct an upload request.
    PutObjectRequest put = new PutObjectRequest("<bucketName>", "<objectName>", "<uploadFilePath>");
    
    // You can set progress callback when uploading a file by calling the asynchronous interface.
    put.setProgressCallback(new OSSProgressCallback<PutObjectRequest>() {
        @Override
        public void onProgress(PutObjectRequest request, long currentSize, long totalSize) {
            Log.d("PutObject", "currentSize: " + currentSize + " totalSize: " + totalSize);
        }
    });
    
    OSSAsyncTask task = oss.asyncPutObject(put, new OSSCompletedCallback<PutObjectRequest, PutObjectResult>() {
        @Override
        public void onSuccess(PutObjectRequest request, PutObjectResult result) {
            Log.d("PutObject", "UploadSuccess");
            Log.d("ETag", result.getETag());
            Log.d("RequestId", result.getRequestId());
        }
    
        @Override
        public void onFailure(PutObjectRequest request, ClientException clientExcepion, ServiceException serviceException) {
            // A request exception occurs.
            if (clientExcepion != null) {
                // A local exception (such as network exception) occurs.
                clientExcepion.printStackTrace();
            }
            if (serviceException != null) {
                // A service exception occurs.
                Log.e("ErrorCode", serviceException.getErrorCode());
                Log.e("RequestId", serviceException.getRequestId());
                Log.e("HostId", serviceException.getHostId());
                Log.e("RawMessage", serviceException.getRawMessage());
            }
        }
    });
    // task.cancel(); // You can cancel the task.
    // task.waitUntilFinished(); // You can also wait until the task is complete.
    ```


## Upload a binary byte\[\] array {#section_pbp_nv4_mfb .section}

Run the following code to upload a binary byte\[\] array:

```
byte[] uploadData = new byte[100 * 1024];
new Random().nextBytes(uploadData);

// Construct an upload request.
PutObjectRequest put = new PutObjectRequest("<bucketName>", "<objectName>", uploadData);

try {
    PutObjectResult putResult = oss.putObject(put);

    Log.d("PutObject", "UploadSuccess");
    Log.d("ETag", putResult.getETag());
    Log.d("RequestId", putResult.getRequestId());
} catch (ClientException e) {
    // A local exception (such as network exception) occurs.
    e.printStackTrace();
} catch (ServiceException e) {
    // A service exception occurs.
    Log.e("RequestId", e.getRequestId());
    Log.e("ErrorCode", e.getErrorCode());
    Log.e("HostId", e.getHostId());
    Log.e("RawMessage", e.getRawMessage());
}
```

