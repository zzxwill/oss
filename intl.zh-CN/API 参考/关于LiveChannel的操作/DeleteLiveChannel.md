# DeleteLiveChannel {#reference_d4g_k12_xdb .reference}

DeleteLiveChannel接口用来删除指定的LiveChannel。

## 请求语法 {#section_mg2_m12_xdb .section}

```
DELETE /ChannelName?live HTTP/1.1
Date: GMT date
Host: BucketName.oss-cn-hangzhou.aliyuncs.com
Authorization: SignatureValue
```

## 细节分析 {#section_u3x_n12_xdb .section}

-   当有客户端正在向LiveChannel推流时，删除请求会失败。
-   本接口只会删除LiveChannel本身，不会删除推流生成的文件。

## 示例 {#section_syz_p12_xdb .section}

**请求示例**

```
DELETE /test-channel?live HTTP/1.1
Date: Thu, 25 Aug 2016 07:32:26 GMT
Host: test-bucket.oss-cn-hangzhou.aliyuncs.com
Authorization: OSS YJjHKOKWDWINLKXv:ZbfvQ3XwmYEE8O9CX8kwVQYNbzQ=
```

**返回示例**

```
HTTP/1.1 204
content-length: 0
server: AliyunOSS
connection: close
x-oss-request-id: 57BE9F0AB92475920B0023E0
date: Thu, 25 Aug 2016 07:32:26 GMT
```

