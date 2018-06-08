# GetSymlink {#reference_s3d_s1x_wdb .reference}

The GetSymlink operation is used to obtain a symbolic link of which you must have the read permission.

## Request syntax {#section_czv_w1x_wdb .section}

```
GET /ObjectName? symlink HTTP/1.1
Host: BucketName.oss-cn-hangzhou.aliyuncs.com
Date: GMT Date
Authorization: SignatureValue
```

## Response header {#section_pwz_y1x_wdb .section}

|Name|Type|Description|
|:---|:---|:----------|
|x-oss-symlink-target|String|The target file that the symbolic link points to. |

## Detail analysis {#section_iq4_fbx_wdb .section}

If the symbolic link does not exist, the system returns the 404 Not Found error. Error code: NoSuchKey.

## Example {#section_jcp_hbx_wdb .section}

**Request example:**

```
GET /link-to-oss.jpg? symlink HTTP/1.1
Host: oss-example.oss-cn-hangzhou.aliyuncs.com
Date: Fri, 24 Feb 2012 06:38:30 GMT
Authorization: OSS qn6qrrqxo2oawuk53otfjbyc:UNQDb7GapEgJCZkcde6OhZ9Jfe8=
```

**Response example:**

```
HTTP/1.1 200 OK
Server: AliyunOSS
Date: Fri, 24 Feb 2012 06:38:30 GMT
Last-Modified: Fri, 24 Feb 2012 06:07:48 GMT
Content-Length: 0
Connection: keep-alive
x-oss-request-id: 5650BD72207FB30443962F9A
x-oss-symlink-target: oss.jpg
ETag: "A797938C31D59EDD08D86188F6D5B872"
```

