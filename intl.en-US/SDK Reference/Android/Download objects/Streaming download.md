# Streaming download {#concept_ryz_dhg_qgb .concept}

If you have a large object to download or it is time-consuming to download the entire object at a time, you can use the streaming download method. Streaming download enables you to download a part of an object each time until you download the entire object.

You can obtain the input stream of an object if you download the object in the streaming download method. You must have the write permission on the object to perform streaming download.

Run the following code to perform a streaming download task synchronously:

```language-java
// Construct an object download request.
GetObjectRequest get = new GetObjectRequest("<bucketName>", "<objectKey>");

// Configure the download progress callback function.
get.setProgressListener(new OSSProgressCallback<GetObjectRequest>() {
            @Override
            public void onProgress(GetObjectRequest request, long currentSize, long totalSize) {
                OSSLog.logDebug("getobj_progress: " + currentSize+"  total_size: " + totalSize, false);
            }
        });

try {
	// Execute the download request synchronously and return the result.
	GetObjectResult getResult = oss.getObject(get);

	Log.d("Content-Length", "" + getResult.getContentLength());

	// Obtain the input stream of the object.
	InputStream inputStream = getResult.getObjectContent();

	byte[] buffer = new byte[2048];
	int len;

	while ((len = inputStream.read(buffer)) != -1) {
		// Process the downloaded data. For example, display images or write data to files.
	}

	// You can view the metadata of the download object.
	ObjectMetadata metadata = getResult.getMetadata();
	Log.d("ContentType", metadata.getContentType());


} catch (ClientException e) {
	// Catch local exceptions, such as network exceptions.
	e.printStackTrace();
} catch (ServiceException e) {
	// Catch service exceptions.
	Log.e("RequestId", e.getRequestId());
	Log.e("ErrorCode", e.getErrorCode());
	Log.e("HostId", e.getHostId());
	Log.e("RawMessage", e.getRawMessage());
} catch (IOException e) {
	e.printStackTrace();
}

```

Run the following code to perform a streaming download tasks asynchronously:

```language-java
GetObjectRequest get = new GetObjectRequest("<bucketName>", "<objectKey>");
// Configure the download progress callback function.
get.setProgressListener(new OSSProgressCallback<GetObjectRequest>() {
            @Override
            public void onProgress(GetObjectRequest request, long currentSize, long totalSize) {
                OSSLog.logDebug("getobj_progress: " + currentSize+"  total_size: " + totalSize, false);
            }
        });
OSSAsyncTask task = oss.asyncGetObject(get, new OSSCompletedCallback<GetObjectRequest, GetObjectResult>() {
	@Override
	public void onSuccess(GetObjectRequest request, GetObjectResult result) {
		// The request is successful.
		InputStream inputStream = result.getObjectContent();

		byte[] buffer = new byte[2048];
		int len;

		try {
			while ((len = inputStream.read(buffer)) != -1) {
				// Process the downloaded data.
			}
		} catch (IOException e) {
			e.printStackTrace();
		}
	}

	@Override
	public void onFailure(GetObjectRequest request, ClientException clientExcepion, ServiceException serviceException) {
		// Handle the exceptions returned for the request.
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

```

