# RTMP推流地址及签名 {#reference_tkt_5bd_xdb .reference}

RTMP推流地址形如：rtmp://your-bucket.oss-cn-hangzhou.aliyuncs.com/live/test-channel

其组成规则为: `rtmp://${bucket}.${host}/live/${channel}?${params}`

-   live为RTMP协议的app名称，OSS固定使用live。
-   params为推流的参数，格式与HTTP请求的query string相同，即形如”varA=valueA&varB=valueB“。
-   BucketAcl非public-read-write时，推流地址需要签名才可以使用；签名方法类似OSS的Url签名，但有一些细节上的不同，后文会描述具体的规则。

## RTMP推流支持的url参数 {#section_psn_zbd_xdb .section}

|名称|描述|
|:-|:-|
|playlistName|用来指定生成的m3u8文件名称，其值覆盖LiveChannel中的配置。注意：生成的m3u8名称仍然会被添加”$\{channel\_name\}/“前缀。|

## 推流地址的签名规则 {#section_vzf_bcd_xdb .section}

一个带签名的推流地址形如：`rtmp://${bucket}.${host}/live/${channel}?OSSAccessKeyId=xxx&Expires=yyy&Signature=zzz&${params}`

|参数名称|描述|
|:---|:-|
|OSSAccessKeyId|意义同OSS的HTTP签名的AccessKeyId|
|Expires|过期时间戳，格式采用Unit timestamp|
|Signature|签名字符串，后文会描述其计算方法|
|params|其他参数，所有的参数都需要参与签名|

Signature的计算规则如下：

```
base64(hmac-sha1(AccessKeySecret,
    + Expires + "\n"
    + CanonicalizedParams
    + CanonicalizedResource))
```

|名称|描述|
|:-|:-|
|CanonicalizedResource|格式为 “/BucketName/ChannelName”|
|CanonicalizedParams|按照param key字典序拼接”key:value\\n”,将所有的参数拼起来，如果参数个数为0，那么这一项为空。参数中不包含SecurityToken、OSSAccessKeyId和Expire以及Signature。每一个param key只能出现一次。|

