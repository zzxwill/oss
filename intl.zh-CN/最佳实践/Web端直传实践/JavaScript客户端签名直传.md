# JavaScript客户端签名直传 {#concept_frd_4gy_5db .concept}

本示例讲解如何在客户端通过JavaScript代码完成签名，然后通过表单直传数据到OSS。

## Demo {#section_fgf_dqq_p2b .section}

[PC浏览器测试样例](http://oss-demo.aliyuncs.com/oss-h5-upload-js-direct/index.html)

本示例采用Plupload 直接提交表单数据（即[PostObject](../../../../intl.zh-CN/API 参考/关于Object操作/PostObject.md#)）到OSS，可以运行在PC浏览器、手机浏览器、微信等。您可以同时选择多个文件上传，并设置上传到指定目录和设置上传文件名字是随机文件名还是本地文件名。您还可以通过进度条查看上传进度。

**说明：** 文件上传到一个测试的公共Bucket，会定时清理，所以不要传一些敏感及重要数据。

## 步骤 1：下载并安装Plugload {#section_kmy_lqk_p2b .section}

Plupload是一款简单易用且功能强大的文件上传工具， 支持多种上传方式，包括html5、flash、silverlight,、html4。它会智能检测当前环境，选择最适合的上传方式，并且会优先采用Html5方式。请参见[Plupload官网](https://www.plupload.com/)进行下载和安装。

## 步骤 2：下载应用服务器代码 {#section_ugn_1ky_5db .section}

[下载地址](https://docs-aliyun.cn-hangzhou.oss.aliyun-inc.com/internal/oss/0.0.4/assets/sample/oss-h5-upload-js-direct.zip)

## 步骤 3：设置CORS {#section_jgc_3mk_p2b .section}

HTML表单直接上传到OSS会产生跨域请求。为了浏览安全，需要为Bucket设置跨域规则（CORS），支持Post方法。

具体操作步骤请参见[设置跨域访问](../../../../intl.zh-CN/控制台用户指南/管理存储空间/设置跨域访问.md#)。设置如下图所示：

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4404/15326840627868_zh-CN.png)

**说明：** 在低版本IE浏览器，Plupload会以flash方式执行。您需要设置crossdomain.xml ，设置方法请参见[OSS Web直传—使用Flash上传](https://yq.aliyun.com/articles/3198)。

## 核心代码解析 {#section_vyn_czq_p2b .section}

因为OSS支持POST协议，所以只要在Plupload发送POST请求时带上OSS签名即可。核心代码如下：

```
var uploader = new plupload.Uploader({
    runtimes : 'html5,flash,silverlight,html4',
    browse_button : 'selectfiles',
    //runtimes : 'flash',
    container: document.getElementById('container'),
    flash_swf_url : 'lib/plupload-2.1.2/js/Moxie.swf',
    silverlight_xap_url : 'lib/plupload-2.1.2/js/Moxie.xap',
    url : host,
    multipart_params: {
        'Filename': '${filename}',
        'key' : '${filename}',
        'policy': policyBase64,
        'OSSAccessKeyId': accessid,
        'success_action_status' : '200', //让服务端返回200，不设置则默认返回204
        'signature': signature,
    },
　....
}
```

上述代码中，`’Filename’: ‘${filename}’`表示上传后保持原来的文件名。如果您想上传到特定目录如abc下，且文件名不变，请修改代码如下：

```
multipart_params: {
        'Filename': 'abc/' + '${filename}',
        'key' : '${filename}',
        'policy': policyBase64,
        'OSSAccessKeyId': accessid,
        'success_action_status' : '200', //让服务端返回200，不设置则默认返回204
        'signature': signature,
    },
```

-   设置成随机文件名

    如果想在上传时固定设置成随机文件名，后缀保持跟客户端文件一致，可以将函数改为：

    ```
    function check_object_radio() {
        g_object_name_type = 'random_name';
    }
    ```

-   设置成用户的文件名

    如果想在上传时固定设置成用户的文件名，可以将函数改为：

    ```
    function check_object_radio() {
        g_object_name_type = 'local_name';
    }
    ```

-   设置上传目录

    您可以将文件上传到指定目录下。下面的代码是将上传目录改成abc/，注意目录必须以正斜线（/）结尾。

    ```
    function get_dirname()
    {
        g_dirname = "abc/"; 
    }
    ```

-   上传签名

    上传签名主要是对policyText进行签名，最简单的例子如下：

    ```
    var policyText = {
        "expiration": "2020-01-01T12:00:00.000Z", // 设置Policy的失效时间，如果超过失效时间，就无法通过此Policy上传文件
        "conditions": [
        ["content-length-range", 0, 1048576000] // 设置上传文件的大小限制，如果超过限制，文件上传到OSS会报错
        ]
    }
    ```


**说明：** 把AccesssKeyID 和AccessKeySecret写在代码里面有泄露的风险，建议采用[服务端签名后直传](intl.zh-CN/最佳实践/Web端直传实践/服务端签名后直传.md#)。

