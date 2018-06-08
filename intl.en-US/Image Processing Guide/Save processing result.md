# Save processing result {#concept_bf1_ssc_wdb .concept}

We provide the “saveas” operation for data processing. With this feature, you can save the processing result to a designated bucket as resources and assign it with a specified key. After the resource is saved, you can visit the resource directly by specifying the bucket  to speed up resource download. This feature applies to ultra-large image cropping or other high-latency operations.

## Request syntax {#section_pwj_xsc_wdb .section}

```
POST /ObjectName? x-oss-process HTTP/1.1
Content-Length：ContentLength
Content-Type: ContentType
Host: BucketName.oss-cn-hangzhou.aliyuncs.com
Date: GMT Date
Authorization: SignatureValue
x-oss-process=image/resize,w_100|sys/saveas,o_dGVzdC5qcGc,b_dGVzdA
```

The Post interface is used to call the Image Processing Service. Parameters are passed in the body. The saveas operation is added to support saving the image as an OSS object.  Specifically, the parameters following “x-oss-process” are the same as those for calling image processing features using queryString.

## List of saveas parameters {#section_g54_x5c_wdb .section}

|Parameter|Meaning|
|---------|-------|
|o|Name of the target object. The parameter must be encoded in URL Safe Base64.|
|b|Name of the target bucket. The parameter must be encoded in URL Safe Base64. If this parameter is not specified, the image is saved to the current bucket by default.|

## Detail analysis {#section_stc_cvc_wdb .section}

-   The saveas operation requires that the caller has the permission for writing data to the target bucket and object. Otherwise, 403 is returned.
-   The bucket and object names in the saveas parameter must conform to the bucket and object naming conventions of the OSS. Otherwise, 400 is returned.
-   The bucket specified for the saveas operation must be in the same region as the current bucket. Otherwise, 400 is returned.
-   The saveas operation is only valid in the Post operation and not in the Get operation. Otherwise, 400 is returned.\`

## Examples {#section_s42_dvc_wdb .section}

**Request example**

```
POST /? x-oss-process HTTP/1.1
Host: oss-example.oss.aliyuncs.com
Content-Length: 247
Date: Frey, 04 2012 03:21:12 GMT
Authorization: OSS qn6qrrqxo2oawuk53otfjbyc:KU5h8YMUC78M30dXqf3JxrTZHiA=
x-oss-process=image/resize,w_100|sys/saveas,o_dGVzdC5qcGc,b_dGVzdA
```

In the sample, the parameters indicate to save the zoomed target image to the bucket named “test”, and the object name is test.jpg.

**Response sample**

```
HTTP/1.1 200 OK
x-oss-request-id: 534B371674E88A4D8906008B
Date: Fri, 04 May 2012 03:21:12 GMT
Content-Length: 0
Connection: keep-alive
Server: AliyunOSS
```

