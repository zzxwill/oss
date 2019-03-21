# PutBucketWebsite {#reference_hwb_yr5_tdb .reference}

Sets a bucket to static website hosting mode and sets routing rules.

## Website {#section_bfq_shc_pfb .section}

The Website interface provides the following two features:

-   Sets the default home page and the default 404 page.
-   Sets the RoutingRule. The RoutingRule is used to specify the 3xx routing rules and mirroring back-to-origin rules.

    **Note:** Mirroring back-to-origin is supported in Alibaba Cloud and Finance Cloud.


The following example shows the fields of website:

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

## Request syntax {#section_o3z_sn2_xdb .section}

```
PUT /? website HTTP/1.1
Date: GMT Date
Content-Length：ContentLength
Content-Type: application/xml
Host: BucketName.oss-cn-hangzhou.aliyuncs.com
Authorization: SignatureValue

<? xml version="1.0" encoding="UTF-8"? >
<WebsiteConfiguration>
    <IndexDocument>
        <Suffix>index.html</Suffix>
    </IndexDocument>
    <ErrorDocument>
        <Key>errorDocument.html</Key>
    </ErrorDocument>
</WebsiteConfiguration>
```

## Request elements {#section_d4t_vn2_xdb .section}

|Element|Type|Description|Required|
|WebsiteConfiguration|Container| Root node

 Parent element: None

 |Yes|
|IndexDocument|Container| Specifies the container for the default home page.

 Parent element: WebsiteConfiguration

 |Conditionally required. You must specify at least one of the following containers: IndexDocument, ErrorDocument, and RoutingRules.|
|Suffix|String| Specifies the default home page.

 If this element is configured, access to an object with a slash \(/\) at the end of its name is redirected to the default home page.

 Parent element: IndexDocument

 |Conditionally required. This element must be specified when its parent element IndexDocument is specified.|
|ErrorDocument|Container| Specifies the container for the 404 page.

 Parent element: WebsiteConfiguration

 |Conditionally required. You must specify at least one of the following containers: IndexDocument, ErrorDocument, and RoutingRules.|
|Key|Container| 404 page

 If this element is specified, access to an object that does not exist is redirected to the 404 page.

 Parent element: ErrorDocument

 |Conditionally required. This element must be specified when its parent element ErrorDocument is specified.|
|RoutingRules|Container| Specifies the container for the RoutingRule.

 Parent element: WebsiteConfiguration

 |Conditionally required. You must specify at least one of the following containers: IndexDocument, ErrorDocument, and RoutingRules.|
|RoutingRule|Container| Specifies routing rules or mirroring back-to-origin rules. You can specify a maximum of five RoutingRules.

 Parent element: RoutingRules

 |No|
|RuleNumber|Positive integer| Specifies the sequence number used to match and execute routing rules. Routing rules are matched according to the sequence numbers. If a routing rule matches the number, the rule is executed and the following rules are not executed.

 Parent element: RoutingRule

 |Conditionally required. This element must be specified when its parent element RoutingRule is specified.|
|Condition|Container| Specifies the matching conditions.

 If a routing rule meets all the conditions, it is executed. The elements in the bucket are in the AND relationship, that is, a routing rule must meet all the conditions before it can be considered matched.

 Parent element: RoutingRule

 |Conditionally required. This element must be specified when its parent element RoutingRule is specified.|
|KeyPrefixEquals|String| Indicates that only objects that match the prefix can match the rule.

 Parent element: Condition

 |No|
|HttpErrorCodeReturnedEquals|HTTP status code| Indicates that the rule can be matched only when the object returns the specified status code when being accessed. If the routing rule is a mirroring back-to-source rule, this status code must be 404.

 Parent element: Condition

 |No|
|IncludeHeader|Container| Indicates that the routing rule can be matched only when the specified header is included in the request and the header value equals the specified value. You can specify a maximum five of the same container.

 Parent element: Condition

 |No|
|Key|String| Indicates that the rule is matched only when this header is included in the request and the header value equals the value specified by Equals.

 Parent element: IncludeHeader

 |Conditionally required. This element must be specified when its parent element IncludeHeader is specified.|
|Equals|String| Indicates that the rule can be matched only when the header specified by Key is included in the request and the header value equals to the specified value.

 Parent element: IncludeHeader

 |Conditionally required. This element must be specified when its parent element IncludeHeader is specified.|
|Redirect|Container| Specifies the actions to perform after the rule is matched.

 Parent element: RoutingRule

 |Conditionally required. This element must be specified when its parent element RoutingRule is specified.|
|RedirectType|String| Specifies the redirecting type, which has the following available values:

 -   Mirror \(mirroring back-to-origin\)
-   External \(external redirection, that is, OSS returns a 3xx request which redirects the access to another IP address.\)
-   Internal \(internal redirection, that is, OSS redirects the access from object1 to object2 based on the rule. In this case, the user accesses object2 but not object1.\)
-   AliCDN \(AliCDN redirection, which is used for AliCDN. Unlike the External type, OSS adds an additional header to the request. After identifying the header, AliCDN redirects the access to the specified IP address and returns the obtained data but not the 3xx redirecting request to the user.\)

 Parent element: Redirect

 |Conditionally required. This element must be specified when its parent element Redirect is specified.|
|PassQueryString|Bool| Indicates whether the request parameter is carried when the redirection or mirroring back-to-origin is performed. The available value of the element is true or false.

 For example, if the parameter "?a=b&c=d" is carried in a request to OSS and this element is set to true, this parameter is added to the Location header when the rule is 302 redirection. For example, if the request is "Location:www.test.com?a=b&c=d” and the redirecting type is mirroring back-to-origin, the parameter is also carried in the back-to-origin request.

 Default value: false

 Parent element: Redirect

 |No|
|MirrorURL|String| Indicates the IP address of the origin site in the mirroring back-to-origin. This element takes effect only when the value of RedirectType is Mirror.

 If the MirrorURL starts with http:// or s://, it must be ended with a slash \(/\). OSS constructs the back-to-origin URL by adding the target object to the MirrorURL. For example, if MirrorURL is set to `http://www.test.com/`and the object to be accessed is "myobject", the back-to-origin URL is `http://www.test.com/dir1/myobject`. If MirrorURL is set to `http://www.test.com/dir1/`, the back-to-origin URL is `http://www.test.com/dir1/myobject`.

 Parent element: Redirect

 |Conditionally required. This element must be specified if the RedirectType is Mirror.|
|MirrorPassQueryString|Bool| This element plays the same role as PassQueryString and has a higher priority than PassQueryString. However, this element take effects only when the RedirectType is Mirror.

 Default value: false

 Parent element: Redirect

 |No|
|MirrorFollowRedirect|Bool| Indicates whether the access is redirected to the specified Location if the origin site returns a 3xx status code when receiving a back-to-origin request.

 For example, the origin site returns a 302 status code and specifies the Location when receiving a mirroring back-to-origin request. In this case, if the value of MirrorFollowRedirect is true, OSS continues to send requests to the IP address specified by the Location. \(A request can be redirected for a maximum of 10 times. If the request is redirected for more than 10 times, a mirroring back-to-origin failure message is returned.\) If the value of MirrorFollowRedirect is false, OSS returns a 302 status code and passes through the Location. This element takes effect only when the value of RedirectType is Mirror.

 Default value: true

 Parent element: Redirect

 |No|
|MirrorCheckMd5|Bool|Indicates whether OSS performs an MD5 check on the body of the response returned by the origin site.

When the value of this element is true and the response returned by the origin site includes a Content-Md5 header, OSS checks whether the MD5 checksum of the obtained data matches the header. If not, OSS does not store the data. This element takes effect only when the value of RedirectType is Mirror.

Default value: false

 Parent element: Redirect|No|
|MirrorHeaders|Container| Specifies the header carried in the response returned by the origin site. This element takes effect only when the value of RedirectType is Mirror.

 Parent element: Redirect

 |No|
|PassAll|Bool| Indicates whether OSS passes through all headers \(except for reserved headers and the headers starting with oss-/x-oss-/x-drs-\) to the origin site. This element takes effect only when the value of RedirectType is Mirror.

 Default value: false

 Parent element: MirrorHeaders

 |No|
|Pass|String| Specifies the headers that are passed through to the origin site. A maximum of 10 headers can be specified. The maximum length of a header is 1,024 bytes. The character set of this element is: 0-9, A-Z, a-z, and dash. This element takes effect only when the value of RedirectType is Mirror.

 Parent element: MirrorHeaders

 |No|
|Remove|String| Specifies the headers that cannot be passed to the origin site. A maximum of 10 headers can be specified \(including repeated headers\). This element is used together with PassAll. The maximum length of a header is 1,024 bytes. The character set of this element is the same as that of Pass. This element takes effect only when the value of RedirectType is Mirror.

 Parent element: MirrorHeaders

 |No|
|Set|Container| Specifies headers that are sent to the origin site. The specified headers are configured in the data returned by the origin site no matter whether they are carried in the request. A maximum of 10 groups of headers can be configured \(including repeated headers\). This element takes effect only when the value of RedirectType is Mirror.

 Parent element: MirrorHeaders

 |No|
|Key|String| Specifies the key of the header. The maximum length of a key is 1,024 bytes. The character set of this element is the same as that of Pass. This element takes effect only when the value of RedirectType is Mirror.

 Parent element: Set

 |Conditionally required. This element must be specified when its parent element Set is specified.|
|Value|String| Specifies the value of the header. The maximum length of the value is 1,024 bytes. The character "\\r\\n" is not allowed in the element. This element takes effect only when the value of RedirectType is Mirror.

 Parent element: Set

 |Conditionally required. This element must be specified when its parent element Set is specified.|
|Protocol|String| Indicates the protocol used for redirections. The available value of this element is http or https. For example, the Location header is `https://www.test.com/test` if the requested object is test, the request is redirected to `www.test.com`, and the value of Protocol is https. This element takes effect only when the value of RedirectType is External or AliCDN.

 Parent element: Redirect

 |Conditionally required. This element must be specified when the value of RedirectType is not External or AliCDN.|
|HostName|String| Indicates the domain name used for redirections, which must comply with the specifications for domain names. For example, the Location header is `https://www.test.com/test` if the requested object is test, the value of Protocol is https, and the Hostname is specified to `www.test.com`. This element takes effect only when the value of RedirectType is External or AliCDN.

 Parent element: Redirect

 |Conditionally required. This element must be specified when the value of RedirectType is not External or AliCDN.|
|HttpRedirectCode|HTTP status code| Indicates the returned status code in redirections. The available value of this element is 301, 302, or 307. This element takes effect only when the value of RedirectType is External or AliCDN.

 Parent element: Redirect

 |Conditionally required. This element must be specified when the value of RedirectType is not External or AliCDN.|
|ReplaceKeyPrefixWith|String| Indicates the string used to replace the prefix of the requested object name in redirections. If the prefix of the object name is empty, this string is added before the object name. The ReplaceKeyWith and ReplaceKeyPrefixWith elements cannot be set simultaneously.

 For example, if KeyPrefixEquals is set to abc/ and ReplaceKeyPrefixWith is set to def/, the Location header for an object named abc/test.txt is `http://www.test.com/def/test.txt`. This element takes effect only when the value of RedirectType is External or AliCDN.

 Parent element: Redirect

 |Conditionally required. This element must be specified when the value of RedirectType is not External or AliCDN.|
|ReplaceKeyWith|String| Indicates the string used to replace the requested object name in redirections. This element can be a variable. \(The $\{key\} variable indicating the object name in the request is supported.\) The ReplaceKeyWith and ReplaceKeyPrefixWith elements cannot be set simultaneously.

 For example, if ReplaceKeyWith is set to prefix/$\{key\}.suffix, the Location header for an object named test is `http://www.test.com/prefix/test.suffix`. This element takes effect only when the value of RedirectType is External or AliCDN.

 Parent element: Redirect

 |Conditionally required. This element must be specified when the value of RedirectType is not External or AliCDN.|

## Detail Analysis {#section_hms_f42_xdb .section}

-   Static websites are the websites where all Web pages are composed of static content, including scripts such as JavaScript executed on the client. OSS does not support content that needs to be processed by the server, such as PHP, JSP, and APS.NET.
-   To use your own domain name to access bucket-based static websites, you can use the CNAME. For more information about the configuration method, see [Bind custom domain names \(CNAME\)](../../../../../reseller.en-US/Developer Guide/Buckets/Attach a custom domain name.md#).
-   To set a bucket to static website hosting mode, you must specify the index page, and the error page is optional.
-   To set a bucket to static website hosting mode, the specified index page and error page are objects in the bucket.
-   After a bucket is set to static website hosting mode, OSS returns the index page for anonymous access to the root domain name of the static website, and returns the results of Get Bucket for signed access to the root domain name of the static website.
-   If you upload the Content-MD5 request header, OSS calculates the body's Content-MD5 and checks whether the two values are the same. If the two values are different, an InvalidDigest error code is returned.

## Example {#section_c33_k42_xdb .section}

**Request example:**

```
PUT /? website HTTP/1.1
Host: oss-example.oss-cn-hangzhou.aliyuncs.com
Content-Length: 209
Date: Fri, 04 May 2012 03:21:12 GMT
Authorization: OSS qn6q**************tfjbyc:KU5h8YMUC78M30dXqf3JxrTZHiA=

<? xml version="1.0" encoding="UTF-8"? >
<WebsiteConfiguration>
<IndexDocument>
<Suffix>index.html</Suffix>
</IndexDocument>
<ErrorDocument>
<Key>error.html</Key>
</ErrorDocument>
</WebsiteConfiguration>
```

**Response example:**

```
HTTP/1.1 200 OK
x-oss-request-id: 534B371674E88A4D8906008B
Date: Fri, 04 May 2012 03:21:12 GMT
Content-Length: 0
Connection: keep-alive
Server: AliyunOSS
```

**Complete code:**

```
PUT /? website HTTP/1.1
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

