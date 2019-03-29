# Append upload {#concept_frp_1bp_mfb .concept}

This topic describes how to upload a file in the append upload method, in which the uploaded file is appended to an existing object.

An object created by the AppendObject operation is an appendable object, and an object created by the PutObject operation is a normal object.

Run the following code to upload a file in the append upload method:

```
AppendObjectRequest append = new AppendObjectRequest("<bucketName>", "<objectName>", "<uploadFilePath>");

ObjectMetadata metadata = new ObjectMetadata();
metadata.setContentType("application/octet-stream");
append.setMetadata(metadata);

// Specify the position where the uploaded data is appended.
append.setPosition(0);

// Set progress callback.
append.setProgressCallback(new OSSProgressCallback<AppendObjectRequest>() {
    @Override
    public void onProgress(AppendObjectRequest request, long currentSize, long totalSize) {
        Log.d("AppendObject", "currentSize: " + currentSize + " totalSize: " + totalSize);
    }
});
// Perform append upload by calling the asynchronous interface.
OSSAsyncTask task = oss.asyncAppendObject(append, new OSSCompletedCallback<AppendObjectRequest, AppendObjectResult>() {
    @Override
    public void onSuccess(AppendObjectRequest request, AppendObjectResult result) {
        Log.d("AppendObject", "AppendSuccess");
        Log.d("NextPosition", "" + result.getNextPosition());
    }

    @Override
    public void onFailure(AppendObjectRequest request, ClientException clientExcepion, ServiceException serviceException) {
        // Handle the exceptions
    }
});
```

Set the position parameter properly as follows when uploading a file in the appended upload method.

-   Set the value of position to 0 when you create a new appendable object by performing the AppendObject operation.
-   When you append the uploaded data to an appendable object, set the value of position to the current size of the object.

    You can get the size of an object in the following two methods:

    -   Get the object size from the response to the upload request.
    -   Get the object size by initiating a HeadObject request.

