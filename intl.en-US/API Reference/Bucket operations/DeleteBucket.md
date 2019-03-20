# DeleteBucket {#reference_o1j_rrw_tdb .reference}

Deletes a bucket.

**Note:** 

-   Only the owner of a bucket can delete the bucket.
-   To prevent accidental deletion, users are not allowed to delete a bucket that is not empty.

## Request syntax {#section_ign_23w_bz .section}

```
DELETE / HTTP/1.1
Host: BucketName.oss-cn-hangzhou.aliyuncs.com
Date: GMT Date
Authorization: SignatureValue
```

## Examples {#section_md1_g3w_bz .section}

-   Delete a bucket normally.

    Request example:

    ```
    DELETE / HTTP/1.1
    Host: test.oss-cn-hangzhou.aliyuncs.com
    Accept-Encoding: identity
    User-Agent: aliyun-sdk-python/2.6.0(Windows/7/AMD64;3.7.0)
    Accept: */*
    Connection: keep-alive
    date: Tue, 15 Jan 2019 08:19:04 GMT
    authorization: OSS qn6qrrqxo2oawuk53otfjbyc:ceOEyZavKY4QcjoUWYSpYbJ3naA=
    Content-Length: 0
    ```

    Response example:

    ```
    HTTP/1.1 204 No Content
    Server: AliyunOSS
    Date: Tue, 15 Jan 2019 08:19:04 GMT
    Content-Length: 0
    Connection: keep-alive
    x-oss-request-id: 5C3D9778CC1C2AEDF85BD9B7
    x-oss-server-time: 190
    ```

-   The bucket to be deleted does not exist.

    Request example:

    ```
    DELETE / HTTP/1.1
    Host: test.oss-cn-hangzhou.aliyuncs.com
    Accept-Encoding: identity
    User-Agent: aliyun-sdk-python/2.6.0(Windows/7/AMD64;3.7.0)
    Accept: */*
    Connection: keep-alive
    date: Tue, 15 Jan 2019 07:53:24 GMT
    authorization: OSS qn6qrrqxo2oawuk53otfjbyc:ceOEyZavKY4QcjoUWYSpYbJ3naA=
    Content-Length: 0
    ```

    Response example:

    ```
    HTTP/1.1 404 Not Found
    Server: AliyunOSS
    Date: Tue, 15 Jan 2019 07:53:25 GMT
    Content-Type: application/xml
    Content-Length: 288
    Connection: keep-alive
    x-oss-request-id: 5C3D9175B6FC201293AD4890
    
    <? xml version="1.0" encoding="UTF-8"? >
    <Error>
      <Code>NoSuchBucket</Code> 
      <Message>The specified bucket does not exist. </Message> 
      <RequestId>5C3D9175B6FC201293AD4890</RequestId>
      <HostId>test.oss-cn-hangzhou.aliyuncs.com</HostId>
      <BucketName>test</BucketName>
    </Error>
    ```

-   The bucket to be deleted is not empty.

    Request example:

    ```
    DELETE / HTTP/1.1
    Host: test.oss-cn-hangzhou.aliyuncs.com
    Accept-Encoding: identity
    User-Agent: aliyun-sdk-python/2.6.0(Windows/7/AMD64;3.7.0)
    Accept: */*
    Connection: keep-alive
    date: Tue, 15 Jan 2019 07:35:06 GMT
    authorization: OSS qn6qrrqxo2oawuk53otfjbyc:ceOEyZavKY4QcjoUWYSpYbJ3naA=
    Content-Length: 0
    ```

    Response example:

    ```
    HTTP/1.1 409 Conflict
    Server: AliyunOSS
    Date: Tue, 15 Jan 2019 07:35:06 GMT
    Content-Type: application/xml
    Content-Length: 296
    Connection: keep-alive
    x-oss-request-id: 5C3D8D2A0ACA54D87B43C048
    x-oss-server-time: 16
    
    <? xml version="1.0" encoding="UTF-8"? >
    <Error>
      <Code>BucketNotEmpty</Code>
      <Message>The bucket you tried to delete is not empty. </Message>
      <RequestId>5C3D8D2A0ACA54D87B43C048</RequestId>
      <HostId>test.oss-cn-hangzhou.aliyuncs.com</HostId>
      <BucketName>test</BucketName>
    </Error>
    ```


## SDK {#section_egl_m2c_5gb .section}

The SDKs of this API are as follows:

-   [Java](../../../../../intl.en-US/SDK Reference/Java/Manage a bucket.md)
-   [Python](../../../../../intl.en-US/SDK Reference/Python/Bucket.md)
-   [PHP](../../../../../intl.en-US/SDK Reference/PHP/Bucket.md)
-   [Go](../../../../../intl.en-US/SDK Reference/Go/Bucket.md)
-   [C](../../../../../intl.en-US/SDK Reference/C/Bucket.md)
-   [.NET](../../../../../intl.en-US/SDK Reference/. NET/Manage a bucket.md)
-   [Android](../../../../../intl.en-US/SDK Reference/Android/Manage buckets.md)
-   [iOS](../../../../../intl.en-US/SDK Reference/iOS/Manage buckets.md)
-   [Node.js](../../../../../intl.en-US/SDK Reference/Node. js/Manage a bucket.md)
-   [Ruby](../../../../../intl.en-US/SDK Reference/Ruby/Manage buckets.md)

## Error codes {#section_dsv_grs_qgb .section}

|Error code|HTTP status code|Description|
|:---------|:---------------|:----------|
|AccessDenied|403 Forbidden|You do not have the permission to delete the bucket. Only the owner of a bucket can delete the bucket.|

