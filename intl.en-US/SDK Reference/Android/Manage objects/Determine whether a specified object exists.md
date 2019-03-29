# Determine whether a specified object exists {#concept_scc_qnp_mfb .concept}

OSS provides a synchronous interface for you to check whether a specified object exists in a bucket.

Run the following code to determine whether a specified object exists:

```
try {
    if (oss.doesObjectExist("<bucketName>", "<objectKey>")) {
        Log.d("doesObjectExist", "object exist.");
    } else {
        Log.d("doesObjectExist", "object does not exist.");
    }
} catch (ClientException e) {
    // Catch local exceptions, such as network exceptions.
    e.printStackTrace();
} catch (ServiceException e) {
    // Catch service exceptions.
    Log.e("ErrorCode", e.getErrorCode());
    Log.e("RequestId", e.getRequestId());
    Log.e("HostId", e.getHostId());
    Log.e("RawMessage", e.getRawMessage());
}
```

