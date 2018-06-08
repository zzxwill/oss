# DeleteObject {#reference_iqc_mqv_wdb .reference}

The DeleteObject operation is used to delete an object.

## Request syntax {#section_dcz_1rv_wdb .section}

```
DELETE /ObjectName HTTP/1.1
Host: BucketName.oss-cn-hangzhou.aliyuncs.com
Date: GMT Date
Authorization: SignatureValue
```

## Detail analysis {#section_lxj_2rv_wdb .section}

-   To delete an object with Delete Object, you must have the write permission to this object.
-   If the object to be deleted does not exist, OSS returns the 204 No Content status code.
-   If the bucket of the object does not exist, the system returns 404 Not Found.
-   If the file type is **symbolic link**, only the symbolic links are deleted.

## Example {#section_prc_gsv_wdb .section}

**Request example:**

```
DELETE /copy_oss.jpg HTTP/1.1
Host: oss-example.oss-cn-hangzhou.aliyuncs.com
Date: Fri, 24 Feb 2012 07:45:28 GMT
Authorization: OSS qn6qrrqxo2oawuk53otfjbyc:zUglwRPGkbByZxm1+y4eyu+NIUs=
```

**Response example:**

```
HTTP/1.1 204 NoContent
x-oss-request-id: 559CC9BDC755F95A64485981
Date: Fri, 24 Feb 2012 07:45:28 GMT
Content-Length: 0
Connection: keep-alive
Server: AliyunOSS
```

