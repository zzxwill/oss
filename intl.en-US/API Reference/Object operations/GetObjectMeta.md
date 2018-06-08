# GetObjectMeta {#reference_sg4_k2w_wdb .reference}

GetObjectMeta is used to obtain the basic meta information of an object in a bucket, but it does not return the content. The meta information includes the Etag, Size \(the file size\), and LastModified.

## Request syntax {#section_nvn_42w_wdb .section}

```
GET /ObjectName? objectMeta HTTP/1.1
Host: BucketName.oss-cn-hangzhou.aliyuncs.com
Date: GMT Date
Authorization: SignatureValue
```

## Detail analysis {#section_b3b_q2w_wdb .section}

-   After the Get Object Meta request is sent, no message body is returned no matter whether the system returns the OK message or an error message.
-   Get Object Meta must contain the parameters of the ObjectMeta request; otherwise, it indicates a Get Object request.
-   If the file does not exist, the system returns the 404 Not Found error.
-   Get Object Meta is more lightweight than Header Object. Only some basic meta information of an object in a bucket is returned. The meta information includes the Etag, Size \(the file size\), and LastModified. The Size is measured with the value of the Content-Length header.
-   If the file type is symbolic link, only the information of the symbolic link itself is returned.

## Example {#section_wkh_52w_wdb .section}

**Request example:**

```
GET /oss.jpg? objectMeta HTTP/1.1
Host: oss-example.oss-cn-hangzhou.aliyuncs.com
Date: Wed, 29 Apr 2015 05:21:12 GMT
Authorization: OSS qn6qrrqxo2oawuk53otfjbyc:CTkuxpLAi4XZ+WwIfNm0FmgbrQ0=
```

**Request example:**

```
HTTP/1.1 200 OK
x-oss-request-id: 559CC9BDC755F95A64485981
Date: Wed, 29 Apr 2015 05:21:12 GMT
ETag: "5B3C1A2E053D763E1B002CC607C5A0FE"
Last-Modified: Fri, 24 Feb 2012 06:07:48 GMT
Content-Length: 344606
Connection: keep-alive
Server: AliyunOSS
```

