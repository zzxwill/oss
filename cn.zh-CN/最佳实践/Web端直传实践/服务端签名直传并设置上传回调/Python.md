# Python {#concept_ynl_hky_2fb .concept}

本文以Python语言为例，讲解在服务端通过Python代码完成签名，并且设置上传回调，然后通过表单直传数据到OSS。

## 前提条件 {#section_qvk_sky_2fb .section}

-   应用服务器对应的域名可通过公网访问。
-   确保应用服务器已经安装`Python 2.6`以上版本（执行命令`python --version`进行查看）。
-   确保PC端浏览器支持JavaScript。

## 步骤一：配置应用服务器 {#section_rlc_nvj_gfb .section}

下载应用服务器源码\(Python版本\)：[aliyun-oss-appserver-python-master.zip](http://docs-aliyun.cn-hangzhou.oss.aliyun-inc.com/assets/attach/86983/APP_zh/1537974140965/aliyun-oss-appserver-python-master.zip) 。

将源码下载到应用服务器的硬盘，本示例中以`Ubuntu 16.04`为例，放置到`/home/aliyun/aliyun-oss-appserver-python`目录下。进入该目录，打开源码文件`appserver.py`，修改如下的代码片段：

```
# 请填写您的AccessKeyId。
access_key_id = '<yourAccessKeyId>'
# 请填写您的AccessKeySecret。
access_key_secret = '<yourAccessKeySecret>'
# host的格式为 bucketname.endpoint ，请替换为您的真实信息。
host = 'http://bucket-name.oss-cn-hangzhou.aliyuncs.com';
# callback_url为 上传回调服务器的URL，请将下面的IP和Port配置为您自己的真实URL信息。
callback_url = "http://88.88.88.88:8888";
# 用户上传文件时指定的前缀。
upload_dir = 'user-dir-prefix/'
```

-   access\_key\_id ： 设置您的AccessKeyId。
-   access\_key\_secret ： 设置您的AessKeySecret。
-   host ： 格式为`bucketname.endpoint`，例如`bucket-name.oss-cn-hangzhou.aliyuncs.com`。关于Endpoint的介绍，请参见[Endpoint访问域名](../../../../cn.zh-CN/开发指南/基本概念介绍.md#section_t3j_nmt_tdb)。
-   callback\_url ： 设置上传回调URL，即回调服务器地址，用于处理应用服务器与OSS之前的通信。OSS会在文件上传完成后，把文件上传信息通过此回调URL发送给应用服务器。本例中修改为：``

    ```
    `var callbackUrl string ="http://11.22.33.44:1234";`
    ```

-   upload\_dir ： 指定上传文件的前缀，以便区别于其他文件以免发生冲突，您也可以填写空值。

## 步骤二：配置客户端 {#section_mpy_2wj_gfb .section}

下载客户端源码：[aliyun-oss-appserver-js-master.zip](http://docs-aliyun.cn-hangzhou.oss.aliyun-inc.com/assets/attach/86983/APP_zh/1537971352825/aliyun-oss-appserver-js-master.zip) 。

下载客户端源码到PC侧的本地硬盘。本例中以`D:\aliyun\aliyun-oss-appserver-js`目录为例。

进入该目录，打开`upload.js`文件，找到下面的代码语句：

```
// serverUrl是 用户获取 '签名和Policy' 等信息的应用服务器的URL，请将下面的IP和Port配置为您自己的真实信息。
serverUrl = 'http://88.88.88.88:8888'
```

将变量`severUrl`改成应用服务器的地址，客户端可以通过它可以获取签名直传Policy等信息。如本例中可修改为：

```
// serverUrl是 用户获取 '签名和Policy' 等信息的应用服务器的URL，请将下面的IP和Port配置为您自己的真实信息。
serverUrl = 'http://11.22.33.44:1234'
```

## 步骤三： 修改CORS {#section_hk5_cpy_2fb .section}

从浏览器向OSS发出的请求消息带有`Origin`的消息头，OSS对带有`Origin`头的请求消息会首先进行`跨域规则`的验证。

即客户端进行表单直接上传到OSS会产生跨域请求，需要为Bucket设置跨域规则（CORS），支持Post方法。

具体操作步骤请参见[设置跨域访问](../../../../cn.zh-CN/控制台用户指南/管理存储空间/设置跨域访问.md#)。

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/21672/153797433212308_zh-CN.png)

## 步骤四：体验上传回调 {#section_c5r_cpy_2fb .section}

-   启动应用服务器

    在`/home/aliyun-oss-appserver-python`目录下，执行命令启动应用服务器：

    ```
    python appserver.py 11.22.33.44 1234
    ```

-   启动客户端

    在PC侧的客户端源码目录中，打开`index.html` 文件。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/21672/153797433212306_zh-CN.png)

    单击**选择文件**，选择指定类型的文件后，单击**开始上传**。上传成功后，显示回调服务器返回的内容。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/21672/153797433212309_zh-CN.png)


## 应用服务器核心代码解析 {#section_bvh_gbk_gfb .section}

应用服务器源码包含了`签名直传服务`和`上传回调服务`两个功能。

-   签名直传服务

    `签名直传服务`响应客户端发送给应用服务器的`GET`消息，代码片段如下：

    ```
    def do_GET(self):
            print "********************* do_GET "
            token = get_token()
            self.send_response(200)
            self.send_header('Access-Control-Allow-Methods', 'POST')
            self.send_header('Access-Control-Allow-Origin', '*')
            self.send_header('Content-Type', 'text/html; charset=UTF-8')
            self.end_headers()
            self.wfile.write(token)
    ```

-   上传回调服务

    `上传回调服务`响应OSS发送给应用服务器的`POST`消息，代码片段如下：

    ```
    def do_POST(self):
            print "********************* do_POST "
            # get public key
            pub_key_url = ''
            try:
                pub_key_url_base64 = self.headers['x-oss-pub-key-url']
                pub_key_url = pub_key_url_base64.decode('base64')
                url_reader = urllib2.urlopen(pub_key_url)
                pub_key = url_reader.read()
            except:
                print 'pub_key_url : ' + pub_key_url
                print 'Get pub key failed!'
                self.send_response(400)
                self.end_headers()
                return
    
            # get authorization
            authorization_base64 = self.headers['authorization']
            authorization = authorization_base64.decode('base64')
    
            # get callback body
            content_length = self.headers['content-length']
            callback_body = self.rfile.read(int(content_length))
    
            # compose authorization string
            auth_str = ''
            pos = self.path.find('?')
            if -1 == pos:
                auth_str = self.path + '\n' + callback_body
            else:
                auth_str = urllib2.unquote(self.path[0:pos]) + self.path[pos:] + '\n' + callback_body
    
            # verify authorization
            auth_md5 = md5.new(auth_str).digest()
            bio = BIO.MemoryBuffer(pub_key)
            rsa_pub = RSA.load_pub_key_bio(bio)
            try:
                result = rsa_pub.verify(auth_md5, authorization, 'md5')
            except e:
                result = False
    
            if not result:
                print 'Authorization verify failed!'
                print 'Public key : %s' % (pub_key)
                print 'Auth string : %s' % (auth_str)
                self.send_response(400)
                self.end_headers()
                return
    
            # do something accoding to callback_body
    
            # response to OSS
            resp_body = '{"Status":"OK"}'
            self.send_response(200)
            self.send_header('Content-Type', 'application/json')
            self.send_header('Content-Length', str(len(resp_body)))
            self.end_headers()
            self.wfile.write(resp_body)
    ```


