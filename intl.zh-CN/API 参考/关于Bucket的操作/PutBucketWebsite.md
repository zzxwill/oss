# PutBucketWebsite {#reference_hwb_yr5_tdb .reference}

PutBucketWebsite接口用于将一个bucket设置成静态网站托管模式，以及设置跳转规则。

## website {#section_bfq_shc_pfb .section}

website接口主要有两个功能：

-   设置默认主页和默认404页。
-   设置RoutingRule。RoutingRule用来指定3xx跳转规则以及镜像回源规则。

    **说明：** 镜像回源支持公共云、金融云。


website字段样例：

```
<WebsiteConfiguration>
  <IndexDocument>
    <Suffix>index.html</Suffix>
  </IndexDocument>
  <ErrorDocument>
    <Key>error.html</Key>
  </ErrorDocument>
  <RoutingRules>
    <RoutingRule>
      <RuleNumber>1</RuleNumber>
      <Condition>
        <KeyPrefixEquals>abc/</KeyPrefixEquals>
        <HttpErrorCodeReturnedEquals>404</HttpErrorCodeReturnedEquals>
      </Condition>
      <Redirect>
        <RedirectType>Mirror</RedirectType>
        <PassQueryString>true</PassQueryString>
        <MirrorURL>http://www.test.com/</MirrorURL>
        <MirrorPassQueryString>true</MirrorPassQueryString>
        <MirrorFollowRedirect>true</MirrorFollowRedirect>
        <MirrorCheckMd5>false</MirrorCheckMd5>
        <MirrorHeaders>
          <PassAll>true</PassAll>
          <Pass>myheader-key1</Pass>
          <Pass>myheader-key2</Pass>
          <Remove>myheader-key3</Remove>
          <Remove>myheader-key4</Remove>
          <Set>
            <Key>myheader-key5</Key>
            <Value>myheader-value5</Value>
          </Set>
        </MirrorHeaders>
      </Redirect>
    </RoutingRule>
    <RoutingRule>
      <RuleNumber>2</RuleNumber>
      <Condition>
        <KeyPrefixEquals>abc/</KeyPrefixEquals>
        <HttpErrorCodeReturnedEquals>404</HttpErrorCodeReturnedEquals>
        <IncludeHeader>
          <Key>host</Key>
          <Equals>test.oss-cn-beijing-internal.aliyuncs.com</Equals>
        </IncludeHeader>
      </Condition>
      <Redirect>
        <RedirectType>AliCDN</RedirectType>
        <Protocol>http</Protocol>
        <HostName>www.test.com</HostName>
        <PassQueryString>false</PassQueryString>
        <ReplaceKeyWith>prefix/${key}.suffix</ReplaceKeyWith>
        <HttpRedirectCode>301</HttpRedirectCode>
      </Redirect>
    </RoutingRule>
  </RoutingRules>
</WebsiteConfiguration>
```

## 请求语法 {#section_o3z_sn2_xdb .section}

```
PUT /?website HTTP/1.1
Date: GMT Date
Content-Length：ContentLength
Content-Type: application/xml
Host: BucketName.oss-cn-hangzhou.aliyuncs.com
Authorization: SignatureValue

<?xml version="1.0" encoding="UTF-8"?>
<WebsiteConfiguration>
    <IndexDocument>
        <Suffix>index.html</Suffix>
    </IndexDocument>
    <ErrorDocument>
        <Key>errorDocument.html</Key>
    </ErrorDocument>
</WebsiteConfiguration>
```

## 请求元素（Request Elements） {#section_d4t_vn2_xdb .section}

|名称|类型|描述|是否必须|
|WebsiteConfiguration|容器| 根节点

 父节点：无

 |是|
|IndexDocument|容器| 默认主页的容器

 父节点：WebsiteConfiguration

 |有条件，IndexDocument、ErrorDocument、RoutingRules三个容器至少指定一个。|
|Suffix|字符串| 默认主页

 如果设置，则访问以正斜线（/）结尾的object时都会返回此默认主页。

 父节点：IndexDocument

 |有条件，父节点IndexDocument指定时必须指定。|
|ErrorDocument|容器| 404页面的容器

 父节点：WebsiteConfiguration

 |有条件，IndexDocument、ErrorDocument、RoutingRules三个容器至少指定一个。|
|Key|容器| 404页面

 如果指定，则访问的object不存在时则返回此404页面。

 父节点：ErrorDocument

 |有条件，父节点ErrorDocument指定时必须指定。|
|RoutingRules|容器| RoutingRule的容器

 父节点：WebsiteConfiguration

 |有条件，IndexDocument、ErrorDocument、RoutingRules三个容器至少指定一个。|
|RoutingRule|容器| 指定跳转规则或者镜像回源规则，最多指定5个RoutingRule。

 父节点：RoutingRules

 |否|
|RuleNumber|正整数| 匹配和执行RoutingRule的序号，匹配时会按照此序号按顺序匹配规则。如果匹配成功，则执行此规则，后续的规则不再执行。

 父节点：RoutingRule

 |有条件，父节点RoutingRule指定时必须指定。|
|Condition|容器| 匹配的条件

 如果指定的项都满足，则执行此规则。此容器下的各个节点关系是与，即需要所有条件都满足才算匹配。

 父节点：RoutingRule

 |有条件，父节点RoutingRule指定时必须指定。|
|KeyPrefixEquals|字符串| 只有匹配此前缀的object才能匹配此规则。

 父节点：Condition

 |否|
|HttpErrorCodeReturnedEquals|HTTP状态码| 访问指定object时返回此status才能匹配此规则。当跳转规则是镜像回源类型时，此字段必须为404。

 父节点：Condition

 |否|
|IncludeHeader|容器| 只有请求中包含了指定header，且值为指定值时，才能匹配此规则。此容器可以最多重复5个。

 父节点：Condition

 |否|
|Key|字符串| 只有请求中包含了此header，且值为Equals指定值时，才能匹配此规则。

 父节点：IncludeHeader

 |有条件，父节点IncludeHeader指定时必须指定。|
|Equals|字符串| 只有请求中包含了Key指定的header，且值为指定的值时，才能匹配此规则

 父节点：IncludeHeader

 |有条件，父节点IncludeHeader指定时必须指定。|
|Redirect|容器| 指定匹配此规则后执行的动作。

 父节点：RoutingRule

 |有条件，父节点RoutingRule指定时必须指定。|
|RedirectType|字符串| 指定跳转的类型，取值必须为以下几项。

 -   Mirror（镜像回源）
-   External（外部跳转，即OSS会返回一个3xx请求，指定跳转到另外一个地址。）
-   Internal（内部跳转，即OSS会根据此规则将访问的object1转换成另外一个object2，相当于用户访问的是object2。）
-   AliCDN（阿里云CDN跳转，主要用于阿里云的CDN。与External不同的是，OSS会额外添加一个header。阿里云CDN识别到此header后会主动跳转到指定的地址，返回给用户获取到的数据，而不是将3xx跳转请求返回给客户。）

 父节点：Redirect

 |有条件，父节点Redirect指定时必须指定|
|PassQueryString|bool型| 执行跳转或者镜像回源时，是否要携带发起请求的请求参数，取值true或者false。

 用户请求OSS时携带了请求参数”?a=b&c=d”，并且此项设置为true，那么如果规则是302跳转，则在跳转的Location头中会添加此请求参数。如”Location:www.test.com?a=b&c=d”，跳转类型是镜像回源，则在发起的回源请求中也会携带此请求参数。

 默认值：false

 父节点：Redirect

 |否|
|MirrorURL|字符串| 只有在RedirectType为Mirror时有意义，表示镜像回源的源站地址。

 如以http://或者https://开头，必须以正斜线（/）结尾，OSS会在此字符串的基础上加上object构成回源的url。比如指定为`http://www.test.com/`，访问的obejct是”myobject”，则回源的url为`http://www.test.com/myobject`，如果指定为`http://www.test.com/dir1/`，那么回源的url为`http://www.test.com/dir1/myobject`。

 父节点：Redirect

 |有条件，如果RedirectType为Mirror则必须指定|
|MirrorPassQueryString|bool型| 与PassQueryString作用相同，优先级比PassQueryString高，但只有在RedirectType为Mirror时生效。

 默认值：false

 父节点：Redirect

 |否|
|MirrorFollowRedirect|bool型| 如果镜像回源获取的结果是3xx，是否要继续跳转到指定的Location获取数据。

 比如发起镜像回源请求时，如果源站返回了302，并且指定了Location。如果此项为true，则oss会继续请求Location指定的地址（最多跳转10次，如果超过10次，则返回镜像回源失败）。如果为false，那么OSS就会返回302，并将Location透传出去。只有在RedirectType为Mirror时生效。

 默认值：true

 父节点：Redirect

 |否|
|MirrorCheckMd5|bool型|是否要检查回源body的md5。

当此项为true且源站返回的response中含有Content-Md5头时，OSS检查拉取的数据md5是否与此header匹配，如果不匹配，则不保存在oss上。只有在RedirectType为Mirror时生效。

默认值：false

 父节点：Redirect|否|
|MirrorHeaders|容器| 用于指定镜像回源时携带的header。只有在RedirectType为Mirror时生效。

 父节点：Redirect

 |否|
|PassAll|bool型| 是否透传请求中所有的header（除了保留的几个header以及以oss-/x-oss-/x-drs-开头的header）到源站。只有在RedirectType为Mirror时生效。

 默认值：false

 父节点：MirrorHeaders

 |否|
|Pass|字符串| 透传指定的header到源站，最多10个，每个header长度最多1024个字节，字符集为0-9、A-Z、a-z以及横杠。只有在RedirectType为Mirror时生效。

 父节点：MirrorHeaders

 |否|
|Remove|字符串| 禁止透传指定的header到源站，这个字段可以重复，最多10个，一般与PassAll一起使用。每个header长度最多1024个字节，字符集与Pass相同。只有在RedirectType为Mirror时生效。

 父节点：MirrorHeaders

 |否|
|Set|容器| 设置一个header传到源站，不管请求中是否携带这些指定的header，回源时都会设置这些header。该容器可以重复，最多10组。只有在RedirectType为Mirror时生效。

 父节点：MirrorHeaders

 |否|
|Key|字符串| 设置header的key，最多1024个字节，字符集与Pass相同。只有在RedirectType为Mirror时生效。

 父节点：Set

 |有条件，当父节点Set指定时必须指定|
|Value|字符串| 设置header的value，最多1024个字节，不能出现”\\r\\n” 。只有在RedirectType为Mirror时生效。

 父节点：Set

 |有条件，当父节点Set指定时必须指定。|
|Protocol|字符串| 跳转时的协议，只能取值为http或者https。比如访问的文件为test，设定跳转到`www.test.com`，而且Protocol字段为https，那么Location头为`https://www.test.com/test`。只有在RedirectType为External或者AliCDN时生效。

 父节点：Redirect

 |有条件，当RedirectType不为External或者AliCDN时需要指定。|
|HostName|字符串| 跳转时的域名，需要符合域名规范。比如访问的object为test，Protocol为https，Hostname指定为`www.test.com`，那么Location头为`https://www.test.com/test`。只有在RedirectType为External或者AliCDN时生效。

 父节点：Redirect

 |有条件，当RedirectType不为External或者AliCDN时需要指定|
|HttpRedirectCode|HTTP状态码| 跳转时返回的状态码，取值为301、302或307。只有在RedirectType为External或者AliCDN时生效。

 父节点：Redirect

 |有条件，当RedirectType不为External或者AliCDN时需要指定。|
|ReplaceKeyPrefixWith|字符串| Redirect的时候object name的前缀将替换成这个值。如果前缀为空则将这个字符串插入在object namde的最前面。ReplaceKeyWith和ReplaceKeyPrefixWith这两个节点只能共存一个。

 比如指定了KeyPrefixEquals为abc/，指定了ReplaceKeyPrefixWith为def/，那么如果访问的object为abc/test.txt那么Location头则为`http://www.test.com/def/test.txt`。只有在RedirectType为External或者AliCDN时生效。

 父节点：Redirect

 |有条件，当RedirectType不为External或者AliCDN时需要指定。|
|ReplaceKeyWith|字符串| Redirect的时候object name将替换成这个值，可以支持变量（目前支持的变量是$\{key\}，代表着该请求中的object name）。ReplaceKeyWith和ReplaceKeyPrefixWith这两个节点只能共存一个。

 比如设置ReplaceKeyWith为prefix/$\{key\}.suffix，访问的object为test，那么Location头则为`http://www.test.com/prefix/test.suffix`。只有在RedirectType为External或者AliCDN时生效。

 父节点：Redirect

 |有条件，当rectType不为External或者AliCDN时需要指定。|

## 细节分析 {#section_hms_f42_xdb .section}

-   所谓静态网站是指所有的网页都由静态内容构成，包括客户端执行的脚本，例如JavaScript；OSS不支持涉及到需要服务器端处理的内容，例如PHP，JSP，APS.NET等。
-   如果你想使用自己的域名来访问基于bucket的静态网站，可以通过域名CNAME来实现。具体配置方法请参见[绑定自定义域名](../../../../../intl.zh-CN/开发指南/存储空间（Bucket）/绑定自定义域名.md#)。
-   用户将一个bucket设置成静态网站托管模式时，必须指定索引页面，错误页面则是可选的。
-   用户将一个bucket设置成静态网站托管模式时，指定的索引页面和错误页面是该bucket内的一个object。
-   在将一个bucket设置成静态网站托管模式后，对静态网站根域名的匿名访问，OSS将返回索引页面；对静态网站根域名的签名访问，OSS将返回Get Bucket结果。
-   如果用户上传了Content-MD5请求头，OSS会计算body的Content-MD5并检查一致性，如果不一致，将返回InvalidDigest错误码。

## 示例 {#section_c33_k42_xdb .section}

**请求示例：**

```
PUT /?website HTTP/1.1
Host: oss-example.oss-cn-hangzhou.aliyuncs.com
Content-Length: 209
Date: Fri, 04 May 2012 03:21:12 GMT
Authorization: OSS qn6qrrqx******k53otfjbyc:KU5h8YM******0dXqf3JxrTZHiA=

<?xml version="1.0" encoding="UTF-8"?>
<WebsiteConfiguration>
<IndexDocument>
<Suffix>index.html</Suffix>
</IndexDocument>
<ErrorDocument>
<Key>error.html</Key>
</ErrorDocument>
</WebsiteConfiguration>
```

**返回示例：**

```
HTTP/1.1 200 OK
x-oss-request-id: 534B371674E88A4D8906008B
Date: Fri, 04 May 2012 03:21:12 GMT
Content-Length: 0
Connection: keep-alive
Server: AliyunOSS
```

**完整代码：**

```
PUT /?website HTTP/1.1
Date: Fri, 27 Jul 2018 09:03:18 GMT
Content-Length: 2064
Host: test.oss-cn-hangzhou-internal.aliyuncs.com
Authorization: OSS a1nBN******QMf8u:sNKIHT6ci/z231yIT5vYnetDLu4=
User-Agent: aliyun-sdk-python-test/0.4.0

<WebsiteConfiguration>
<IndexDocument>
<Suffix>index.html</Suffix>
</IndexDocument>
<ErrorDocument>
<Key>error.html</Key>
</ErrorDocument>
<RoutingRules>
<RoutingRule>
<RuleNumber>1</RuleNumber>
<Condition>
<KeyPrefixEquals>abc/</KeyPrefixEquals>
<HttpErrorCodeReturnedEquals>404</HttpErrorCodeReturnedEquals>
</Condition>
<Redirect>
<RedirectType>Mirror</RedirectType>
<PassQueryString>true</PassQueryString>
<MirrorURL>http://www.test.com/</MirrorURL>
<MirrorPassQueryString>true</MirrorPassQueryString>
<MirrorFollowRedirect>true</MirrorFollowRedirect>
<MirrorCheckMd5>false</MirrorCheckMd5>
<MirrorHeaders>
<PassAll>true</PassAll>
<Pass>myheader-key1</Pass>
<Pass>myheader-key2</Pass>
<Remove>myheader-key3</Remove>
<Remove>myheader-key4</Remove>
<Set>
<Key>myheader-key5</Key>
<Value>myheader-value5</Value>
</Set>
</MirrorHeaders>
</Redirect>
</RoutingRule>
<RoutingRule>
<RuleNumber>2</RuleNumber>
<Condition>
<KeyPrefixEquals>abc/</KeyPrefixEquals>
<HttpErrorCodeReturnedEquals>404</HttpErrorCodeReturnedEquals>
<IncludeHeader>
<Key>host</Key>
<Equals>test.oss-cn-beijing-internal.aliyuncs.com</Equals>
</IncludeHeader>
</Condition>
<Redirect>
<RedirectType>AliCDN</RedirectType>
<Protocol>http</Protocol>
<HostName>www.test.com</HostName>
<PassQueryString>false</PassQueryString>
<ReplaceKeyWith>prefix/${key}.suffix</ReplaceKeyWith>
<HttpRedirectCode>301</HttpRedirectCode>
</Redirect>
</RoutingRule>
</RoutingRules>
</WebsiteConfiguration>

HTTP/1.1 200 OK
Server: AliyunOSS
Date: Fri, 27 Jul 2018 09:03:18 GMT
Content-Length: 0
Connection: keep-alive
x-oss-request-id: 5B5ADFD6ED3CC49176CBE29D
x-oss-server-time: 47
```

