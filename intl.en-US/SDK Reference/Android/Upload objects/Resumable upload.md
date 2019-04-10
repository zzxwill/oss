# Resumable upload {#concept_32050_zh .concept}

This topic describes how to perform resumable uploads.

In a wireless network, it takes a long time to upload a large file. The upload may fail due to poor network connection or network switching. In this case, the entire file needs to be uploaded again. OSS Android SDK provides the resumable upload feature to address this problem.

**Note:** 

-   Currently, resumable upload is applicable only to local files.
-   We recommend you do not use resumable upload when uploading small-sized and medium-sized files from a mobile device. Resumable upload adopts the multipart upload method, which initiates multiple network requests for uploading a single file, reducing the upload efficiency.

Before uploading a file to OSS using the resumable upload method, you can specify the folder used to store resumable upload records.

-   The resuamble upload method is applied only in the current upload task. In scenarios where the folder used to store resumable upload records is not specified, if a part of a large file fails to be uploaded due to network problems, it takes a large amount of time and traffic to upload the entire file again.
-   If you specify the folder used to store resumable upload records, a failed resumable upload task can be restarted from the position recorded.

When a resumable upload task fails and is not resumed for a long time, the uploaded parts become useless and occupy the storage space of OSS. In this case, you can set lifecycle rules for the bucket to clear useless parts in a timely manner. For more information, see [Lifecycle Management](../../../../../reseller.en-US/Console User Guide/Manage buckets/Set lifecycle.md#).

**Note:** 

-   Resumable upload is performed based on the following APIs: InitMultipartUpload, UploadPart, ListParts, CompleteMultipartUpload, and AbortMultipartUpload. If you use the STS authentication mode to perform resumable upload tasks, make sure that you can access the preceding APIs.
-   Resumable upload supports the upload callback function, which is used in the same way as it is used in normal upload tasks.
-   The MD5 value of each part is verified in a resumable upload task by default. Therefore, you do not have to specify the `Content-Md5` header in the request.

-   The following code uses a calling method in which resumable upload records are not permanently stored on the local device:

    ```language-java
    // Create a resumable upload request.
    ResumableUploadRequest request = new ResumableUploadRequest("<bucketName>", "<objectKey>", "<uploadFilePath>");
    // Configure the progress callback function for the upload task.
    request.setProgressCallback(new OSSProgressCallback<ResumableUploadRequest>() {
    	@Override
    	public void onProgress(ResumableUploadRequest request, long currentSize, long totalSize) {
    		Log.d("resumableUpload", "currentSize: " + currentSize + " totalSize: " + totalSize);
    	}
    });
    // Call the asynchronously resumable upload interface.
    OSSAsyncTask resumableTask = oss.asyncResumableUpload(request, new OSSCompletedCallback<ResumableUploadRequest, ResumableUploadResult>() {
    	@Override
    	public void onSuccess(ResumableUploadRequest request, ResumableUploadResult result) {
    		Log.d("resumableUpload", "success!") ;
    	}
    
    	@Override
    	public void onFailure(ResumableUploadRequest request, ClientException clientExcepion, ServiceException serviceException) {
    		// Handle exceptions.
    	}
    });
    
    // resumableTask.waitUntilFinished(); // Wait until the task is complete.
    
    ```

-   The following code uses a calling method in which resumable upload records are permanently stored on the local device:

    ```language-java
    String recordDirectory = Environment.getExternalStorageDirectory().getAbsolutePath() + "/oss_record/";
    
    File recordDir = new File(recordDirectory);
    
    // The directory must exist. Otherwise, it will be automatically created.
    if (! recordDir.exists()) {
    	recordDir.mkdirs();
    }
    
    // Create a resumable upload request with a parameter indicating the absolute path of the folder for storing resumable upload records.
    ResumableUploadRequest request = new ResumableUploadRequest("<bucketName>", "<objectKey>", "<uploadFilePath>", recordDirectory);
    // Set the progress callback function for the upload task..
    request.setProgressCallback(new OSSProgressCallback<ResumableUploadRequest>() {
    	@Override
    	public void onProgress(ResumableUploadRequest request, long currentSize, long totalSize) {
    		Log.d("resumableUpload", "currentSize: " + currentSize + " totalSize: " + totalSize);
    	}
    });
    
    
    OSSAsyncTask resumableTask = oss.asyncResumableUpload(request, new OSSCompletedCallback<ResumableUploadRequest, ResumableUploadResult>() {
    	@Override
    	public void onSuccess(ResumableUploadRequest request, ResumableUploadResult result) {
    		Log.d("resumableUpload", "success!") ;
    	}
    
    	@Override
    	public void onFailure(ResumableUploadRequest request, ClientException clientExcepion, ServiceException serviceException) {
    		//Handle exceptions.
    	}
    });
    
    // resumableTask.waitUntilFinished();
    
    ```

-   The following code implements a resumable upload task:

    ```language-java
    // Specify whether to delete the settings of resumable upload records when the OSSAsyncTask cancel() method is called
    String recordDirectory = Environment.getExternalStorageDirectory().getAbsolutePath() + "/oss_record/";
    
    File recordDir = new File(recordDirectory);
    
    // The directory must exist. Otherwise, it will be automatically created.
    if (! recordDir.exists()) {
    	recordDir.mkdirs();
    }
    
    // Create a resumable upload request with a parameter indicating the absolute path of the folder for storing resumable upload records
    ResumableUploadRequest request = new ResumableUploadRequest("<bucketName>", "<objectKey>", "<uploadFilePath>", recordDirectory);
    // If it is set to false, the resumable upload records are not deleted when the upload task is canceled. If you do not set it, the default value is true, indicating that the resumable upload records are deleted after the task is canceled and the files are uploaded again during the next upload.
    request.setDeleteUploadOnCancelling(false);
    // Set the progress callback function for the upload task.
    request.setProgressCallback(new OSSProgressCallback<ResumableUploadRequest>() {
    	@Override
    	public void onProgress(ResumableUploadRequest request, long currentSize, long totalSize) {
    		Log.d("resumableUpload", "currentSize: " + currentSize + " totalSize: " + totalSize);
    	}
    });
    
    
    OSSAsyncTask resumableTask = oss.asyncResumableUpload(request, new OSSCompletedCallback<ResumableUploadRequest, ResumableUploadResult>() {
    	@Override
    	public void onSuccess(ResumableUploadRequest request, ResumableUploadResult result) {
    		Log.d("resumableUpload", "success!") ;
    	}
    
    	@Override
    	public void onFailure(ResumableUploadRequest request, ClientException clientExcepion, ServiceException serviceException) {
    		//Handle exceptions.
    	}
    });
    
    // resumableTask.waitUntilFinished();
    
    ```


