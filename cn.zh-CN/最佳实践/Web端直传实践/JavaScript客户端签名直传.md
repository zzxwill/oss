# JavaScript客户端签名直传 {#concept_frd_4gy_5db .concept}

## 背景 {#section_gj5_pgy_5db .section}

客户端用JavaScript直接签名，然后上传到OSS。请参考 [Web端直传实践](cn.zh-CN/最佳实践/Web端直传实践/Web端直传实践简介.md#) 中的背景介绍。

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4404/1460_zh-CN.png)

示例

下面将介绍用[plupload](https://www.plupload.com/) 在JavaScript端签名然后直传数据到OSS的例子。用户电脑浏览器测试样例：[点击这里打开示例](http://oss-demo.aliyuncs.com/oss-h5-upload-js-direct/index.html)

用手机测试该上传是否有效。二维码：可以用手机（微信，QQ，手机浏览器等）扫一扫试试（这个不是广告，只是上述网址的二维码，为了让大家看一下这个实现能在手机端完美运行。）

文件上传是上传到一个测试的公共 bucket , 会定时清理，所以不要传一些敏感及重要数据。

代码下载

点击这里：[oss-h5-upload-js-direct.zip](https://docs-aliyun.cn-hangzhou.oss.aliyun-inc.com/internal/oss/0.0.4/assets/sample/oss-h5-upload-js-direct.zip)

原理

-   本例子的功能

    -   采用plupload 直接提交表单数据（即PostObject）到OSS；
    -   支持html5,flash,silverlight,html4 等协议上传；
    -   可以运行在PC浏览器、手机浏览器、微信等；
    -   可以选择多文件上传；
    -   显示上传进度条；
    -   可以控制上传文件的大小；
    -   可以设置上传到指定目录和设置上传文件名字是随机文件名还是本地文件名。
    OSS的PostObject API细节可以参照[PostObject](../cn.zh-CN/API 参考/关于Object操作/PostObject.md#)。

-   plupload

    [plupload](https://www.plupload.com/)是一款简单易用且功能强大的文件上传工具， 支持多种上传方式，包括html5, flash, silverlight, html4。会智能检测当前环境，选择最适合的方式，并且会优先采用Html5。

-   关键代码

    因为OSS支持POST协议。所以只要将plupload在发送POST请求时，带上OSS签名即可。核心代码如下：

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
            'success_action_status' : '200', //让服务端返回200,不然，默认会返回204
            'signature': signature,
        },
    　....
    }
    ```

    在这里有一点请注意一下，就是’Filename’: ‘$\{filename\}’， 这一段代码的作用是表示上传后保持原来的文件文字。如果您想上传到特定目录如abc下，文件名保持成原来的文件名，那么应该这样写：

    ```
    multipart_params: {
            'Filename': 'abc/' + '${filename}',
            'key' : '${filename}',
            'policy': policyBase64,
            'OSSAccessKeyId': accessid,
            'success_action_status' : '200', //让服务端返回200，不然，默认会返回204
            'signature': signature,
        },
    ```

-   设置成随机文件名

    有时候要把用户上传的文件，设置成随机文件名，后缀保持跟客户端文件一致。例子里面，通过两个radio来区分, 如果想在上传时就固定设置成随机文件名，可以将函数改成如下：

    ```
    function check_object_radio() {
        g_object_name_type = 'random_name';
    }
    ```

    如果想在上传时，固定设置成用户的文件，可以将函数改成：

    ```
    function check_object_radio() {
        g_object_name_type = 'local_name';
    }
    ```

-   设置上传目录

    可以将文件上传到指定目录下面， 目录相关设置可以在例子中体验，如果想让代码上传到固定目录如abc, 可以按如下代码改造，注意要以’/‘ 结尾。

    ```
    function get_dirname()
    {
        g_dirname = "abc/"; 
    }
    ```

-   上传签名

    签名signature主要是对policyText进行签名，最简单的例子如下：

    ```
    var policyText = {
        "expiration": "2020-01-01T12:00:00.000Z", //       设置该Policy的失效时间，超过这个失效时间之后，就没有办法通过这个policy上传文件了
        "conditions": [
        ["content-length-range", 0, 1048576000] // 设置上传文件的大小限制,如果超过了这个大小，文件上传到OSS会报错的
        ]
    }
    ```

-   跨域CORS

    **说明：** 一定要保证bucket属性CORS设置支持POST方法。因为这个HTML直接上传到OSS，会产生跨域请求。必须在bucket属性里面设置允许跨域。

    设置如下图：

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4404/1469_zh-CN.png)

    **说明：** 在IE低版本浏览器，plupload会以flash方式执行。必须设置crossdomain.xml ，设置方法可以参考：[点击这里](https://yq.aliyun.com/articles/3198)

-   注意

    把AccesssKeyID 和AccessKeySecret写在代码里面有泄露的风险。建议采用后端签名上传的方案：[服务端签名后网页直传](cn.zh-CN/最佳实践/Web端直传实践/服务端签名后直传.md#)


