# DeleteBucket {#reference_o1j_rrw_tdb .reference}

DeleteBucket用于删除某个Bucket。

## 请求语法 {#section_ign_23w_bz .section}

```
DELETE / HTTP/1.1
Host: BucketName.oss-cn-hangzhou.aliyuncs.com
Date: GMT Date
Authorization: SignatureValue
```

## 细节分析 {#section_t13_f3w_bz .section}

-   如果Bucket不存在，返回404 no content错误。错误码：NoSuchBucket。
-   为了防止误删除的发生，OSS不允许用户删除一个非空的Bucket。
-   如果试图删除一个不为空的Bucket，返回409 Conflict错误，错误码：BucketNotEmpty。
-   只有Bucket的拥有者才能删除这个Bucket。如果试图删除一个没有对应权限的Bucket，返回403 Forbidden错误。错误码：AccessDenied。

## 示例 {#section_md1_g3w_bz .section}

**请求示例：**

```
DELETE / HTTP/1.1
Host: oss-example.oss-cn-hangzhou.aliyuncs.com  
Date: Fri, 24 Feb 2012 05:31:04 GMT  
Authorization: OSS qn6qrrqxo2oawuk53otfjbyc:ceOEyZavKY4QcjoUWYSpYbJ3naA=

```

**返回示例：**

```
HTTP/1.1 204 No Content 
x-oss-request-id: 534B371674E88A4D8906008B
Date: Fri, 24 Feb 2012 05:31:04 GMT
Connection: keep-alive
Content-Length: 0  
Server: AliyunOSS
```

