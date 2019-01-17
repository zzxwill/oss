# DeleteBucket {#reference_o1j_rrw_tdb .reference}

DeleteBucket用于删除某个Bucket。

## 请求语法 {#section_ign_23w_bz .section}

```
DELETE / HTTP/1.1
Host: BucketName.oss-cn-hangzhou.aliyuncs.com
Date: GMT Date
Authorization: SignatureValue
```

## 示例 {#section_md1_g3w_bz .section}

-   正常删除

    请求示例：

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

    返回示例：

    ```
    HTTP/1.1 204 No Content
    Server: AliyunOSS
    Date: Tue, 15 Jan 2019 08:19:04 GMT
    Content-Length: 0
    Connection: keep-alive
    x-oss-request-id: 5C3D9778CC1C2AEDF85BD9B7
    x-oss-server-time: 190
    ```

-   要删除的Bucket不存在

    请求示例：

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

    返回示例：

    ```
    HTTP/1.1 404 Not Found
    Server: AliyunOSS
    Date: Tue, 15 Jan 2019 07:53:25 GMT
    Content-Type: application/xml
    Content-Length: 288
    Connection: keep-alive
    x-oss-request-id: 5C3D9175B6FC201293AD4890
    
    <?xml version="1.0" encoding="UTF-8"?>
    <Error>
      <Code>NoSuchBucket</Code>
      <Message>The specified bucket does not exist.</Message>
      <RequestId>5C3D9175B6FC201293AD4890</RequestId>
      <HostId>test.oss-cn-hangzhou.aliyuncs.com</HostId>
      <BucketName>test</BucketName>
    </Error>
    ```

-   要删除的Bucket非空

    请求示例：

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

    返回示例：

    ```
    HTTP/1.1 409 Conflict
    Server: AliyunOSS
    Date: Tue, 15 Jan 2019 07:35:06 GMT
    Content-Type: application/xml
    Content-Length: 296
    Connection: keep-alive
    x-oss-request-id: 5C3D8D2A0ACA54D87B43C048
    x-oss-server-time: 16
    
    <?xml version="1.0" encoding="UTF-8"?>
    <Error>
      <Code>BucketNotEmpty</Code>
      <Message>The bucket you tried to delete is not empty.</Message>
      <RequestId>5C3D8D2A0ACA54D87B43C048</RequestId>
      <HostId>test.oss-cn-hangzhou.aliyuncs.com</HostId>
      <BucketName>test</BucketName>
    </Error>
    ```


**说明：** 

-   为了防止误删除的发生，OSS不允许用户删除一个非空的Bucket。
-   只有Bucket的拥有者才能删除这个Bucket。如果试图删除一个没有相应权限的Bucket，返回403 Forbidden错误。错误码：AccessDenied。

