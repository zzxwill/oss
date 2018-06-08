# OSS跨域资源共享（CORS）错误及排除 {#concept_ry5_l1j_wdb .concept}

跨域资源共享（Cross Origin Resource Sharing，简称 CORS）的介绍及配置请参看[跨域资源共享最佳实践](../intl.zh-CN/最佳实践/存储空间管理/跨域资源共享（CORS）.md#)。

## 配置项 {#section_a4y_n1j_wdb .section}

CORS配置有以下几项：

-   来源（AllowedOrigin）

    允许跨域请求的来源，可以同时指定多个。配置时需带上完整的域信息，例如`http://10.100.100.100:8001`或`https://www.aliyun.com`。注意， 不要遗漏了协议名http或https ，如果端口不是默认的`80`，还需要带上端口。如果不能确定的域名，可以打开浏览器的调试功能，查看header中的`Origin`。域名支持通配符`*`，每个域名中允许最多使用一个`*`，例如`https://*.aliyun.com`。如果来源指定为`*`，则表示允许所有来源的跨域请求。

-   Method

    按照需求开通对应的方法即可，调试时可以全部选择。

-   Allow Header

    允许的跨域请求header。允许配置多条匹配规则，以回车间隔。在Access-Control-Request-Headers中指定的每个header，都必须在Allowed Header中有对应项。Header容易遗漏，没有特殊需求的情况下，建议设置为`*`，表示允许所有。大小写不敏感。

-   Expose Header

    暴露给浏览器的header列表，即用户从应用程序中访问的响应头（例如一个Javascript的XMLHttpRequest对象）。不允许使用通配符。具体的配置需要根据应用的需求确定，只暴露需要使用的header。如果不需要暴露可以不填。大小写不敏感。该项是可选配置项。

-   缓存时间（MaxAgeSeconds）

    浏览器对特定资源的预取请求（OPTIONS请求）返回结果的缓存时间，单位为秒。如果没有特殊情况可以稍大一点，比如60秒。该项是可选配置项。


CORS的配置方法一般是针对每个访问来源单独配置规则，不将多个来源混到一个规则，多个规则之间不要有覆盖冲突。其它的选项只开放需要的权限即可。

## 错误排除 {#section_zvm_q1j_wdb .section}

报错

CORS配置错误会报如下错误：

-   浏览器报类似如下错误：

    ```
    OPTIONS http://bucket.oss-cn-beijing.aliyuncs.com/
    XMLHttpRequest cannot load http://bucket.oss-cn-beijing.aliyuncs.com/. Response to preflight request doesn't pass access control check: No 'Access-Control-Allow-Origin' header is present on the requested resource. Origin '{yourwebsiet}' is therefore not allowed access. The response had HTTP status code 403.
    ```

-   OSS报如下错误：

    ```
    <Code>AccessForbidden</Code>
    <Message>CORSResponse: This CORS request is not allowed. This is usually because the evalution of Origin, request method / Access-Control-Request-Method or Access-Control-Requet-Headers are not whitelisted by the resource's CORS spec.</Message>
    ```


**说明：** 

-   CORS报错一般是站点类应用导致，浏览器中可以查看请求详情。如Chrome，按 `F12` 打开 `开发者工具` ，在 `Network` 中查看相应元素；
-   OSS返回的错误可以通过抓包获取。如使用Wireshark，筛选器可以指定为 `host bucket-name.oss-cn-beijing.aliyuncs.com` 。
-   OSS返回的错误也可以通过CORS的调试程序 [oss-h5-upload-js-direct](https://docs-aliyun.cn-hangzhou.oss.aliyun-inc.com/internal/oss/0.0.4/assets/sample/oss-h5-upload-js-direct.zip) 界面提示获取。

其它错误请参看[OSS 403错误及排除](intl.zh-CN/常见错误排除/OSS 403错误及排查.md#)。

排错

CORS可能错误如下：

-   来源（AllowedOrigin）配置不正确
-   Method（AllowedMethod）配置错误
-   Allow Header 配置错误
-   Expose Header 配置错误

调试方法：

-   将 来源（AllowedOrigin）设置成`*`，确认该配置项无误。如果设置成`*`后可以成功上传，说明是来源（AllowedOrigin）配置错误，请根据规则认真检查。
-   选择 Method（AllowedMethod） 的全部选项（GET、PUT、DELETE、POST、HEAD），确认该配置项目无误。
-   将 Allow Header 配置成`*`，确认该配置无误。
-   将 Expose Header 设置为指定值或者不填，确认该项配置无误。

**说明：** 在OSS控制台，选择Bucket后，通过**Bucket属性** \> **跨域设置**配置上述选项。

