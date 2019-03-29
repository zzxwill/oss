# Range download {#concept_uf1_ghg_qgb .concept}

If you only need a part of the data in an object, you can use the range download method to download specified content.

Run the following code to perform a range download task:

```language-java
GetObjectRequest get = new GetObjectRequest("<bucketName>", "<objectKey>");

// Set the download range.
get.setRange(new Range(0, 99)); // Download data between the starting byte value of 0 and the maximum byte value 99 inclusive (that is, a total range of 100 bytes).
// get.setRange(new Range(100, Range.INFINITE)); // Download data between the starting byte value of 100 and the end of the object.
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

