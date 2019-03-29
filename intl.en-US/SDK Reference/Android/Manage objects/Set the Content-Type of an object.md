# Set the Content-Type of an object {#concept_og3_b32_qgb .concept}

In Web services, the Content-Type of an object specifies the type of the object and determines the method and encoding used to read the object.

-   In some cases, you must specify the Content-Type of an object to be uploaded. Otherwise, the object cannot be read in the appropriate method. If you do not specify the Content-Type when using OSS Android SDK to upload an object, the SDK automatically specifies it according to the suffix of the object.

    ```language-java
    // Construct an upload request.
    PutObjectRequest put = new PutObjectRequest("<bucketName>", "<objectKey>", "<uploadFilePath>");
    
    ObjectMetadata metadata = new ObjectMetadata();
    // Set the Content-Type of the object to be uploaded.
    metadata.setContentType("application/octet-stream");
    // Customize the user metadata.
    metadata.addUserMetadata("x-oss-meta-name1", "value1");
    put.setMetadata(metadata);
    
    OSSAsyncTask task = oss.asyncPutObject(put, new OSSCompletedCallback<PutObjectRequest, PutObjectResult>() {
    	...
    });
    
    ```


