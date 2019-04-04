# Download an object to a local file {#concept_jzd_hhg_qgb .concept}

This topic describes how to download an object to a local file.

Run the following code to download a specified object to a local file:

```
public void testDownloadObjectToFile() throws Exception {
        // Upload a file.
        PutObjectRequest put = new PutObjectRequest(mBucketName, "file1m",
                OSSTestConfig.FILE_DIR + "file1m");
        OSSTestConfig.TestPutCallback putCallback = new OSSTestConfig.TestPutCallback();
        String srcFileBase64Md5 = BinaryUtil.toBase64String(BinaryUtil.calculateMd5(OSSTestConfig.FILE_DIR + "file1m"));
        OSSAsyncTask putTask = oss.asyncPutObject(put, putCallback);
        putTask.waitUntilFinished();
        assertEquals(200, putCallback.result.getStatusCode());

        // Download an object to a local file.
        GetObjectRequest get = new GetObjectRequest(mBucketName, "file1m");
        OSSTestConfig.TestGetCallback getCallback = new OSSTestConfig.TestGetCallback();
        OSSAsyncTask getTask = oss.asyncGetObject(get, getCallback);
        getTask.waitUntilFinished();
        assertEquals(200, getCallback.result.getStatusCode());
        long length = getCallback.result.getContentLength();
        byte[] buffer = new byte[(int) length];
        int readCount = 0;
        while (readCount < length) {
            readCount += getCallback.result.getObjectContent().read(buffer, readCount, (int) length - readCount);
        }
        try {
            FileOutputStream fout = new FileOutputStream(OSSTestConfig.FILE_DIR + "download_file1m");
            fout.write(buffer);
            fout.close();
        } catch (Exception e) {
            OSSLog.logInfo(e.toString());
        }
        String downloadFileBase64Md5 = BinaryUtil.toBase64String(BinaryUtil.calculateMd5(OSSTestConfig.FILE_DIR + "download_file1m"));
        assertEquals(srcFileBase64Md5, downloadFileBase64Md5);

    }
```

