# OSS防盗链（Referer）配置及错误排除 {#concept_kh3_c3j_wdb .concept}

## 什么是Referer {#section_q1c_d3j_wdb .section}

`Referer`是HTTP Header的一部分，当浏览器向Web服务器发送请求时，一般会带上Referer，告诉服务器从哪个页面链接过来的。

## Referer的作用 {#section_bct_23j_wdb .section}

-   防盗链。比如，网站访问自己的图片服务器，图片服务器取到Referer来判断是不是自己的域名，如果是就继续访问，不是则拦截。
-   数据统计。比如，统计用户是从哪里链接过来访问的。

## Referer为空 {#section_jvc_g3j_wdb .section}

空Referer指的是HTTP请求中Referer头部的内容为空，或者HTTP请求中不包含Referer头部。

下面两种情况Referer为空：

-   当请求并不是由链接触发产生。比如，直接把地址输入地址栏里打开页面。
-   从https页面上的链接访问到非加密的http页面时，在http页面上是检查不到Referer的。

在防盗链设置中，允许空Referer和不允许空Referer有什么区别呢？

在防盗链的白名单设置中，如果指明白名单中包含空的Referer，那么通过浏览器地址栏直接访问该资源URL是可以访问到的；但如果不指名需要包含空的Referer，那么通过浏览器直接访问也是被禁止的。

## OSS防盗链 {#section_sb2_h3j_wdb .section}

OSS防盗链是通过`Referer`来实现的，所以也简称为`Refer`或`refer`，详细说明请参见[OSS防盗链](../intl.zh-CN/最佳实践/存储空间管理/防盗链.md#)。

-   OSS防盗链配置

    OSS防盗链包括:

    -   是否允许Referer字段为空的请求访问
    -   Referer字段白名单
    OSS防盗链通过在控制台或[SDK](https://www.alibabacloud.com/help/doc-detail/32021.htm)设置bucket属性来配置。

-   OSS防盗链注意点

    OSS防盗链配置中有以下注意点：

    -   用户只有通过URL签名或者匿名访问Object时，才会做防盗链验证。请求Header中有`Authorization`字段时，不会做防盗链验证。
    -   一个Bucket可以支持多个Referer参数，这些参数之间由`,`分隔。
    -   Referer参数支持通配符`*`和`?`，详细解释参见下面的通配符说明。
    -   用户可以设置`是否允许Referer字段为空`的请求访问。
    -   白名单为空时，不会检查Referer字段是否为空（不然所有的请求都会被拒绝）。
    -   白名单不为空，且设置了不允许Referer字段为空的规则，则只有Referer属于白名单的请求被允许，其它请求（包括Referer为空的请求）会被拒绝。
    -   如果白名单不为空，但设置了允许Referer字段为空的规则，则Referer为空的请求和符合白名单的请求会被允许，其它请求都会被拒绝。
    -   Bucket的三种权限（private、public-read、public-read-write）都会检查Referer字段。
    通配符说明：

    -   星号`*`：代替0个或多个字符。如果正在查找以AEW开头的一个文件，但不记得文件名其余部分，可以输入AEW，查找以AEW开头的所有文件类型的文件，如AEWT.txt、AEWU.EXE、AEWI.dll等。要缩小范围可以输入AEW.txt，查找以AEW开头的所有文件类型并.txt为扩展名的文件如AEWIP.txt、AEWDF.txt。
    -   问号`?`：代替一个字符。如果输入love?，查找以love开头的一个字符结尾文件类型的文件，如lovey、lovei等。要缩小范围可以输入love?.doc，查找以love开头的一个字符结尾文件类型并.doc为扩展名的文件如lovey.doc、loveh.doc。
-   典型配置
    -   所有请求都可以访问

        -   空Refer：允许refer为空
        -   Refer列表： 空
    -   带规定的Referer的请求才能访问

        -   空Refer：不允许refer为空
        -   Refer列表：`http://*.oss-cn-beijing.aliyuncs.com`， `http://*.aliyun.com`
    -   带规定的Referer的请求、不带Referer的请求可以访问

        -   空Refer：允许refer为空
        -   Refer列表：`http://*.oss-cn-beijing.aliyuncs.com`， `http://*.aliyun.com`

## 常见错误及排除 {#section_fxn_n3j_wdb .section}

当Referer配置错误时，HTTP状态码（http code）是403，OSS返回如下错误：

```
<Code>AccessDenied</Code>
<Message>You are denied by bucket referer policy.</Message>
```

**说明：** 

-   Referer报错一般是站点类应用，浏览器中可以查看Header的Referer。如Chrome，按 `F12` 打开 `开发者工具` ，在 `Network` 中查看相应元素的Header。
-   OSS返回的错误可以通过抓包获取。如Wireshark，筛选器可以指定为 `host bucket-name.oss-cn-beijing.aliyuncs.com` 。

可能的原因：

-   Referer为空，请求Header中没有Referer字段或者Referer字段为空。
-   Referer不在规定的Referer范围内。以下几点请注意：
    -   `http://`还是`https://`配置时请确认。
    -   a.aliyun.com和b.aliyun.com，匹配于`http://*.aliyun.com`或`http://?.aliyun.com`；
    -   domain.com匹配于`http://domain.com`，而不是`http://*.domain.com`；
-   Referer格式错误，Refer配置必须带`http://`或者`https://`，否则无效。如b.aliyun.com是无效配置。

**说明：** 

-   在 **OSS控制台** \> **Bucket** \> **Bucket属性** \> **防盗链** 中配置Referer。
-   调试时请清空浏览器缓存。
-   OSS的Refer只支持白名单， 暂时不支持黑名单。

其它错误的排除请参看[OSS 403错误及排除](intl.zh-CN/常见错误排除/OSS 403错误及排查.md#)。

## 其它问题 {#section_y3c_cjj_wdb .section}

设置防盗链后，为什么用curl还是能抓到视频？

检查是否开启了CDN、CDN的Refer设置不为空、CDN的防盗链名单与OSS一致。调试OSS的Referer时，请先排除CDN的影响。先调好OSS的Referer，再调CDN的Referer。

Referer更详细的介绍及配置请参见[防盗链](../intl.zh-CN/最佳实践/存储空间管理/防盗链.md#)。

