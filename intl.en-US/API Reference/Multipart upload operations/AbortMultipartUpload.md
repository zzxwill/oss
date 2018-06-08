# AbortMultipartUpload {#reference_txp_bvx_wdb .reference}

AbortMultipartUpload can be used to stop a Multipart Upload event based on the Upload ID you provide.

When a Multipart Upload event is aborted, you cannot use this Upload ID to perform any operations and the uploaded data parts are deleted.

## Request syntax {#section_w35_2vx_wdb .section}

```
DELETE /ObjectName? uploadId=UploadId HTTP/1.1
Host: BucketName.oss-cn-hangzhou.aliyuncs.com
Date: GMT Date
Authorization: Signature
```

## Detail analysis {#section_e4l_yvx_wdb .section}

-   When you stop a Multipart Upload event, parts still being uploaded are not deleted. Therefore, if concurrent accesses exist, you must call the AbortMultipartUpload interface several times to completely release the space of OSS.
-   If the entered Upload ID does not exist, OSS returns an error 404 with the error code: NoSuchUpload.

## Example {#section_lwt_cwx_wdb .section}

**Request example:**

```
Delete /multipart.data? &uploadId=0004B9895DBBB6EC98E HTTP/1.1
Host: oss-example.oss-cn-hangzhou.aliyuncs.com
Date: Wed, 22 Feb 2012 08:32:21 GMT
Authorization: OSS qn6qrrqxo2oawuk53otfjbyc:J/lICfXEvPmmSW86bBAfMmUmWjI=
```

**Response example:**

```
HTTP/1.1 204 
Server: AliyunOSS
Connection: keep-alive
x-oss-request-id: 059a22ba-6ba9-daed-5f3a-e48027df344d
Date: Wed, 22 Feb 2012 08:32:21 GMT
```

