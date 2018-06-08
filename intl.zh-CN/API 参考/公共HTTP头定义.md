# 公共HTTP头定义 {#reference_mhp_zdy_wdb .reference}

## 公共请求头（Common Request Headers） {#section_eq1_b2y_wdb .section}

OSS的RESTful接口中使用了一些公共请求头。这些请求头可以被所有的OSS请求所使用，其详细定义如下：

|名称|类型|描述|
|:-|:-|:-|
|Authorization|字符串|用于验证请求合法性的认证信息。默认值：无

使用场景：非匿名请求

|
|Content-Length|字符串|[RFC2616](https://www.ietf.org/rfc/rfc2616.txt)中定义的HTTP请求内容长度。 默认值：无

使用场景：需要向OSS提交数据的请求

|
|Content-Type|字符串|[RFC2616](https://www.ietf.org/rfc/rfc2616.txt)中定义的HTTP请求内容类型。默认值：无

使用场景：需要向OSS提交数据的请求

|
|Date|字符串|HTTP 1.1协议中规定的GMT时间，例如：Wed, 05 Sep. 2012 23:00:00 GMT 默认值：无

|
|Host|字符串|访问Host值，格式为：`<bucketname>.oss-cn-hangzhou.aliyuncs.com`。默认值：无

|

## 公共响应头（Common Response Headers） {#section_hcd_m2y_wdb .section}

OSS的RESTful接口中使用了一些公共响应头。这些响应头可以被所有的OSS请求所使用，其详细定义如下：

|名称|类型|描述|
|:-|:-|:-|
|Content-Length|字符串|[RFC2616](https://www.ietf.org/rfc/rfc2616.txt)中定义的HTTP请求内容长度。默认值：无

使用场景：需要向OSS提交数据的请求

|
|Connection|枚举|标明客户端和OSS服务器之间的链接状态。有效值：open、close

默认值：无

|
|Date|字符串|HTTP 1.1协议中规定的GMT时间，例如：Wed, 05 Sep. 2012 23:00:00 GMT默认值：无

 |
|ETag|字符串|ETag \(entity tag\) 在每个Object生成的时候被创建，用于标示一个Object的内容。对于Put Object请求创建的Object，ETag值是其内容的MD5值；对于其他方式创建的Object，ETag值是其内容的UUID。ETag值可以用于检查Object内容是否发生变化。默认值：无

|
|Server|字符串|生成Response的服务器。 默认值：AliyunOSS

|
|x-oss-request-id|字符串|x-oss-request-id是由Aliyun OSS创建，并唯一标识这个response的UUID。如果在使用OSS服务时遇到问题，可以凭借该字段联系OSS工作人员，快速定位问题。默认值：无

|

