# Put Bucket ACL {#reference_zzr_hk5_tdb .reference}

The PutBucketACL interface is used to modify the access permissions for a bucket. 

Currently, three bucket access permissions are available: public-read-write, public-read, and private.   You can use the  “x-oss-acl” header in a Put request to set the Put Bucket ACL operation.  Only the creator of the bucket has permission to perform this operation.   If the operation succeeds, 200 is returned; otherwise, the corresponding error code and prompt message are returned.

## Request syntax {#section_ds2_2nr_bz .section}

```
PUT /? acl HTTP/1.1
x-oss-acl: Permission
Host: BucketName.oss-cn-hangzhou.aliyuncs.com
Date: GMT Date
Authorization: SignatureValue
```

## Detail analysis {#section_l4x_fnr_bz .section}

-   When a bucket already exists and is owned by the request sender and the permission in the request is different from the existing permission,  this request does not change bucket content but updates the permission.
-   If the information for user authentication is not introduced when you initiate a Put Bucket request, the message of  403  Forbidden is returned.  Error code: AccessDenied.
-   If the x-oss-acl header is unavailable in a request and the bucket already exists and belongs to the request sender, the permissions for the original bucket remain the same.

## Example {#section_nsl_hnr_bz .section}

**Request example:**

```
PUT /? acl HTTP/1.1
x-oss-acl: public-read
Host: oss-example.oss-cn-hangzhou.aliyuncs.com
Date: Fri, 24 Feb 2012 03:21:12 GMT
Authorization: OSS qn6qrrqxo2oawuk53otfjbyc:KU5h8YMUC78M30dXqf3JxrTZHiA=
```

**Response example:**

```
HTTP/1.1 200 OK
x-oss-request-id: 534B371674E88A4D8906008B
Date: Fri, 24 Feb 2012 03:21:12 GMT
Content-Length: 0
Connection: keep-alive
Server: AliyunOSS
```

If the permission for this setting does not exist, the message of 400 Bad Request is shown:

**Returned error example:**

```
HTTP/1.1 400 Bad Request
x-oss-request-id: 56594298207FB304438516F9
Date: Fri, 24 Feb 2012 03:55:00 GMT
Content-Length: 309
Content-Type: text/xml; charset=UTF-8
Connection: keep-alive
Server: AliyunOSS

<? xml version="1.0" encoding="UTF-8"? >
<Error>
  <Code>InvalidArgument</Code>
  <Message>no such bucket access control exists</Message>
  <RequestId>5***9</RequestId>
  <HostId>***-test.example.com</HostId>
  <ArgumentName>x-oss-acl</ArgumentName>
  <ArgumentValue>error-acl</ArgumentValue>
</Error>
```

