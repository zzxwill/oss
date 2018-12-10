# PHP {#concept_nhs_ldt_2fb .concept}

本文以PHP语言为例，讲解在服务端通过PHP代码完成签名，并且设置上传回调，然后通过表单直传数据到OSS。

## 前提条件 {#section_c5l_ndt_2fb .section}

-   Web服务器已部署。
-   Web服务器对应的域名可通过公网访问。
-   Web服务器能够解析PHP（执行命令`php -v`进行查看）。
-   PC端浏览器支持JavaScript。

## 步骤 1：配置Web服务器 {#section_rcg_mpj_gfb .section}

本文档以Ubuntu16.04和Apache2.4.18为例。本示例中的环境配置如下：

-   Web服务器外网IP地址为`11.22.33.44`。您可以在配置文件`/etc/apache2/apache2.conf`中增加`ServerName 11.22.33.44`来进行修改。
-   Web服务器的监听端口为`8080`。您可以在配置文件`/etc/apache2/ports.conf`中进行修改相关内容`Listen 8080`。
-   确保Apache能够解析PHP文件：`sudo apt-get install libapache2-mod-php5`（其他平台请根据实际情况进行安装配置）。

您可以根据自己的实际环境修改IP地址和监听端口。更新配置后，需要重启Apache服务器，重启命令为/etc/init.d/apache2 restart。

## 步骤 2：配置应用服务器 {#section_hgm_52t_2fb .section}

-   下载部署应用服务器源码

    下载应用服务器源码（PHP版本）：[aliyun-oss-appserver-php-master.zip](http://docs-aliyun.cn-hangzhou.oss.aliyun-inc.com/assets/attach/92590/APP_zh/1539603337889/aliyun-oss-appserver-php-master.zip)

    下载应用服务器源码后，将其部署到应用服务器的相应目录。本文示例中需要部署到Ubuntu16.04的`/var/www/html/aliyun-oss-appserver-php`目录下。

    在PC侧浏览器中访问应用服务器URL `http://11.22.33.44:8080/aliyun-oss-appserver-php/index.html`， 显示的页面内容与[测试样例主页](http://oss-demo.aliyuncs.com/oss-h5-upload-js-php-callback/index.html?spm=a2c4g.11186623.2.19.63f561e4APLM8H)相同则验证通过。

-   开启Apache捕获HTTP头部Authorization字段的功能

    您的应用服务器收到的回调请求有可能没有Authotization头，这是因为有些Web应用服务器会将Authorization头自行解析掉。比如Apache2，需要设置成不解析头部。

    -   打开Apache2配置文件`/etc/apache2/apache2.conf`，找到如下片段，进行相应修改：

        ```
        <Directory /var/www/>
                Options Indexes FollowSymLinks
                AllowOverride All
                Require all granted
        </Directory>
        ```

    -   在`/var/www/html/aliyun-oss-appserver-php`目录下，新建一个`.htaccess`文件，填写如下内容：

        ```
        <IfModule mod_rewrite.c>
        RewriteEngine on
        RewriteCond %{HTTP:Authorization} .
        RewriteRule .* - [E=HTTP_AUTHORIZATION:%{HTTP:Authorization}]
        </IfModule>
        ```

    其他Web服务器或其他Apache版本，请根据实际情况进行配置。

-   修改配置应用服务器

    在`/var/www/html/aliyun-oss-appserver-php/php`目录下打开文件`get.php`， 修改如下代码片段：

    ```
    $id= '<yourAccessKeyId>';          // 请填写您的AccessKeyId。
        $key= '<yourAccessKeySecret>';     // 请填写您的AccessKeySecret。
    
        // $host的格式为 bucketname.endpointx， 请替换为您的真实信息。
        $host = 'http://bucket-name.oss-cn-hangzhou.aliyuncs.com';  
    
        // $callbackUrl为上传回调服务器的URL， 请将下面的IP和Port配置为您自己的真实URL信息。
        $callbackUrl = 'http://88.88.88.88:8888/aliyun-oss-appserver-php/php/callback.php';
    
        $dir = 'user-dir-prefix/';          // 上传文件时指定的前缀。
    ```

    -   $id ： 设置您的AccessKeyId。
    -   $key ： 设置您的AccessKeySecret。
    -   $host： 格式为`BucketName.Endpoint`，例如`bucket-name.oss-cn-hangzhou.aliyuncs.com`。关于Endpoint的介绍，请参见[Endpoint访问域名](../../../../cn.zh-CN/开发指南/基本概念介绍.md#section_t3j_nmt_tdb)。
    -   $callbackUrl：设置上传回调URL，即回调服务器地址，用于处理应用服务器与OSS之前的通信。OSS会在文件上传完成后，把文件上传信息通过此回调URL发送给应用服务器。本例中修改为：

        ```
        `$callbackUrl='http://11.22.33.44:8080/aliyun-oss-appserver-php/php/callback.php';`
        ```

    -   $dir： 设置上传到OSS文件的前缀，以便区别于其他文件从而避免冲突，您也可以填写空值。

## 步骤 3：配置客户端 {#section_jhc_yrj_gfb .section}

在应用服务器的`/var/www/html/aliyun-oss-appserver-php`目录下修改文件`upload.js`：

对于PHP版本的应用服务器源码，一般不需要修改文件`upload.js`内容的，因为相对路径也是可以正常工作的。

如果确实需要修改，请找到如下的代码片段：

```
`serverUrl ='./php/get.php'`
```

将变量`severUrl`改成服务器部署的地址，用于处理浏览器和应用服务器之间的通信。

比如本示例中可以如下修改：

```
`serverUrl ='http://11.22.33.44:8080/aliyun-oss-appserver-php/php/get.php'`
```

## 步骤 4：修改CORS {#section_qqv_1sj_gfb .section}

从浏览器向OSS发出的请求消息带有`Origin`的消息头，OSS对带有`Origin`头的请求消息会首先进行`跨域规则`的验证。

即客户端进行表单直接上传到OSS会产生跨域请求，需要为Bucket设置跨域规则（CORS），支持Post方法。

具体操作步骤请参见[设置跨域访问](../../../../cn.zh-CN/控制台用户指南/管理存储空间/设置跨域访问.md#)。

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/21672/154443254212308_zh-CN.png)

**说明：** **来源**设置为 \* 是为了使用方便，不确保安全性。建议您填写自己需要的域名。

## 步骤 5：体验上传回调 {#section_fxb_5ft_2fb .section}

在PC侧的Web浏览器中输入`http://11.22.33.44:8080/aliyun-oss-appserver-php/index.html`。

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/21672/154443254312306_zh-CN.png)

单击**选择文件**，选择指定类型的文件后，单击**开始上传**。上传成功后，显示回调服务器返回的内容。

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/21672/154443254312309_zh-CN.png)

## 应用服务器核心代码解析 {#section_apl_gsj_gfb .section}

应用服务器源码包含了`签名直传服务`和`上传回调服务`两个功能。

-   签名直传服务

    `签名直传服务`响应客户端发送给应用服务器的`GET`消息，代码文件是`aliyun-oss-appserver-php/php/get.php`。代码片段如下：

    ```
    
    $response = array();
    $response['accessid'] = $id;
    $response['host'] = $host;
    $response['policy'] = $base64_policy;
    $response['signature'] = $signature;
    $response['expire'] = $end;
    $response['callback'] = $base64_callback_body;
    $response['dir'] = $dir; 
    ```

-   上传回调服务

    `上传回调服务`响应OSS发送给应用服务器的`POST`消息，代码文件是`aliyun-oss-appserver-php/php/callback.php`。

    代码片段如下：

    ```
    // 6.验证签名
    $ok = openssl_verify($authStr, $authorization, $pubKey, OPENSSL_ALGO_MD5);
    if ($ok == 1)
    {
        header("Content-Type: application/json");
        $data = array("Status"=>"Ok");
        echo json_encode($data);
    }
    ```

    详情请参见API文档[Callback–回调签名](../../../../cn.zh-CN/API 参考/关于Object操作/Callback.md#section_btz_phx_wdb)。


