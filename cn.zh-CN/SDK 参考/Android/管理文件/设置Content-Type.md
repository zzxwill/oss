# 设置Content-Type {#concept_og3_b32_qgb .concept}

在Web服务中Content-Type用于设定文件的类型，决定以哪种形式、什么编码读取这个文件。

-   某些情况下，对于上传的文件需要设置Content-Type，否则文件不能以需要的形式和编码来读取。如果使用SDK上传文件时没有指定Content-Type，SDK会帮您根据后缀自动添加Content-Type。

    ```language-java
    // 构造上传请求
    PutObjectRequest put = new PutObjectRequest("<bucketName>", "<objectKey>", "<uploadFilePath>");
    
    ObjectMetadata metadata = new ObjectMetadata();
    // 指定Content-Type
    metadata.setContentType("application/octet-stream");
    // user自定义metadata
    metadata.addUserMetadata("x-oss-meta-name1", "value1");
    put.setMetadata(metadata);
    
    OSSAsyncTask task = oss.asyncPutObject(put, new OSSCompletedCallback<PutObjectRequest, PutObjectResult>() {
    	...
    });
    
    ```


