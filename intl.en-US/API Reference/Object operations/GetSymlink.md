# GetSymlink {#reference_s3d_s1x_wdb .reference}

Obtains a symbol link. To perform GetSymlink operations, you must have the read permission on the symbol link.

## Request syntax {#section_czv_w1x_wdb .section}

```
GET /ObjectName?symlink HTTP/1.1
Host: BucketName.oss-cn-hangzhou.aliyuncs.com
Date: GMT Date
Authorization: SignatureValue
```

## Response headers {#section_pwz_y1x_wdb .section}

|Header|Type|Description|
|:-----|:---|:----------|
|x-oss-symlink-target|String|Indicates the target object that the symbol link directs to.|

## Examples {#section_jcp_hbx_wdb .section}

Request example:

```
GET /link-to-oss.jpg? symlink HTTP/1.1
Host: oss-example.oss-cn-hangzhou.aliyuncs.com
Date: Fri, 24 Feb 2012 06:38:30 GMT
Authorization: OSS qn6qrrqxo2oawuk53otfjbyc:UNQDb7GapEgJCZkcde6OhZ9Jfe8=
```

Response example:

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

## SDK {#section_egl_m2c_5gb .section}

The SDKs of this API are as follows:

-   [Java](../../../../../intl.en-US/SDK Reference/Java/Manage objects/Manage a symbolic link.md)
-   [Python](../../../../../intl.en-US/SDK Reference/Python/Manage objects/Manage a symbolic link.md)
-   [PHP](../../../../../intl.en-US/SDK Reference/PHP/Manage objects/Manage a symbolic link.md)
-   [Go](../../../../../intl.en-US/SDK Reference/Go/Manage objects/Manage a symbolic link.md)
-   [C](../../../../../intl.en-US/SDK Reference/C/Manage objects/Manage a symbolic link.md)
-   [.NET](../../../../../intl.en-US/SDK Reference/. NET/Manage objects/Manage a symbolic link.md)

## Error codes {#section_nfp_nfc_5gb .section}

|Error code|HTTP status code|Description|
|:---------|:---------------|:----------|
|NoSuchKey|404|The requested symbol link does not exist.|

