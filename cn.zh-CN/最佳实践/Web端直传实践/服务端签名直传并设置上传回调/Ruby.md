# Ruby {#concept_fqm_tjz_2fb .concept}

本文以Ruby语言为例，讲解在服务端通过Ruby代码完成签名，并且设置上传回调，然后通过表单直传数据到OSS。

## 前提条件 {#section_crt_vjz_2fb .section}

-   应用服务器对应的域名可通过公网访问。
-   确保应用服务器已经安装`Ruby 2.0`以上版本（执行命令`ruby -v`进行查看）。
-   确保PC端浏览器支持JavaScript。

## 步骤一：配置应用服务器 {#section_xcf_l2k_gfb .section}

下载应用服务器源码\(Ruby版本\)：[aliyun-oss-appserver-ruby-master.zip](http://docs-aliyun.cn-hangzhou.oss.aliyun-inc.com/assets/attach/86983/APP_zh/1537974391908/aliyun-oss-appserver-ruby-master.zip)

将源码下载到应用服务器的硬盘，本示例中以`Ubuntu 16.04`为例，放置到`/home/aliyun/aliyun-oss-appserver-ruby`目录下。进入该目录，打开源码文件`appserver.rb`，修改如下代码片段：

```
# 请填写您的AccessKeyId。
$access_key_id = '<yourAccessKeyId>'

# 请填写您的AccessKeySecret。
$access_key_secret = '<yourAccessKeySecret>'

# $host的格式为 bucketname.endpoint ，请替换为您的真实信息。
$host = 'http://bucket-name.oss-cn-hangzhou.aliyuncs.com';

# $callbackUrl为 上传回调服务器的URL，请将下面的IP和Port配置为您自己的真实信息。
$callback_url = "http://88.88.88.88:8888";

# 用户上传文件时指定的前缀。
$upload_dir = 'user-dir-prefix/'
```

-   $access\_key\_id ： 设置您的AccessKeyId。
-   $access\_key\_secret ： 设置您的AessKeySecret。
-   $host ： 格式为`bucketname.endpoint`，例如`bucket-name.oss-cn-hangzhou.aliyuncs.com`。关于Endpoint的介绍，请参见[Endpoint访问域名](../../../../cn.zh-CN/开发指南/基本概念介绍.md#section_t3j_nmt_tdb)。
-   $callback\_url ： 设置上传回调URL，即回调服务器地址，用于处理应用服务器与OSS之前的通信。OSS会在文件上传完成后，把文件上传信息通过此回调URL发送给应用服务器。本例中修改为：

    ```
    `$callback_url="http://11.22.33.44:1234";`
    ```

-   $upload\_dir ： 设置上传到OSS文件的前缀，以便区别于其他文件从而避免冲突，您也可以填写空值。

## 步骤二：配置客户端 {#section_gcw_s2k_gfb .section}

下载客户端源码：[aliyun-oss-appserver-js-master.zip](http://docs-aliyun.cn-hangzhou.oss.aliyun-inc.com/assets/attach/86983/APP_zh/1537971352825/aliyun-oss-appserver-js-master.zip)

下载客户端源码到PC侧的本地硬盘。本例中以`D:\aliyun\aliyun-oss-appserver-js`目录为例。

进入该目录，打开`upload.js`文件，找到下面的代码语句：

```
// serverUrl是 用户获取 '签名和Policy' 等信息的应用服务器的URL，请将下面的IP和Port配置为您自己的真实信息。
serverUrl = 'http://88.88.88.88:8888'
```

将变量`severUrl`改成应用服务器的地址，客户端可以通过它获取签名直传Policy等信息。如本例中可修改为：

```
// serverUrl是 用户获取 '签名和Policy' 等信息的应用服务器的URL，请将下面的IP和Port配置为您自己的真实信息。
serverUrl = 'http://11.22.33.44:1234'
```

## 步骤三： 修改CORS {#section_mtw_y2k_gfb .section}

从浏览器向OSS发出的请求消息带有`Origin`的消息头，OSS对带有`Origin`头的请求消息会首先进行`跨域规则`的验证。

即客户端进行表单直接上传到OSS会产生跨域请求，需要为Bucket设置跨域规则（CORS），支持Post方法。

具体操作步骤请参见[设置跨域访问](../../../../cn.zh-CN/控制台用户指南/管理存储空间/设置跨域访问.md#)。

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/21672/153797454412308_zh-CN.png)

## 步骤四：体验上传回调 {#section_epk_vkz_2fb .section}

-   启动应用服务器

    在`/home/aliyun/aliyun-oss-appserver-ruby`目录下，执行以下命令启动应用服务器：

    ```
    ruby appserver.rb 11.22.33.44 1234
    ```

-   启动客户端

    在PC侧的客户端源码目录中，打开`index.html` 文件。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/21672/153797454412306_zh-CN.png)

    单击**选择文件**，选择指定类型的文件后，单击**开始上传**。上传成功后，显示回调服务器返回的内容。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/21672/153797454412309_zh-CN.png)


## 应用服务器核心代码解析 {#section_vr5_r4j_gfb .section}

应用服务器源码包含了`签名直传服务`和`上传回调服务`两个功能。

-   签名直传服务

    `签名直传服务`响应客户端发送给应用服务器的`GET`消息，代码片段如下：

    ```
    def get_token()
        expire_syncpoint = Time.now.to_i + $expire_time
    	
        expire = Time.at(expire_syncpoint).utc.iso8601()
        response.headers['expire'] = expire
    	
        policy_dict = {}
        condition_arrary = Array.new
        array_item = Array.new
        array_item.push('starts-with')
        array_item.push('$key')
        array_item.push($upload_dir)
        condition_arrary.push(array_item)
        policy_dict["conditions"] = condition_arrary
        policy_dict["expiration"] = expire
        policy = hash_to_jason(policy_dict)
        policy_encode = Base64.strict_encode64(policy).chomp;
        h = OpenSSL::HMAC.digest('sha1', $access_key_secret, policy_encode)
        hs = Digest::MD5.hexdigest(h)
        sign_result = Base64.strict_encode64(h).strip()
    	
        callback_dict = {}
        callback_dict['callbackBodyType'] = 'application/x-www-form-urlencoded';
        callback_dict['callbackBody'] = 'filename=${object}&size=${size}&mimeType=${mimeType}&height=${imageInfo.height}&width=${imageInfo.width}';
        callback_dict['callbackUrl'] = $callback_url;
        callback_param = hash_to_jason(callback_dict)
        base64_callback_body = Base64.strict_encode64(callback_param);
    	
        token_dict = {}
        token_dict['accessid'] = $access_key_id
        token_dict['host'] = $host
        token_dict['policy'] = policy_encode
        token_dict['signature'] = sign_result 
        token_dict['expire'] = expire_syncpoint
        token_dict['dir'] = $upload_dir
        token_dict['callback'] = base64_callback_body
        response.headers["Access-Control-Allow-Methods"] = "POST"
        response.headers["Access-Control-Allow-Origin"] = "*"
        result = hash_to_jason(token_dict)
        
        result
    end
    
    get '/*' do
      puts "********************* GET "
      get_token()
    end
    ```

-   上传回调服务

    `上传回调服务`响应OSS发送给应用服务器的`POST`消息，代码片段如下：

    ```
    post '/*' do
      puts "********************* POST"
      pub_key_url = Base64.decode64(get_header('x-oss-pub-key-url'))
      pub_key = get_public_key(pub_key_url)
      rsa = OpenSSL::PKey::RSA.new(pub_key)
    
      authorization = Base64.decode64(get_header('authorization'))
      req_body = request.body.read
      if request.query_string.empty? then
        auth_str = CGI.unescape(request.path) + "\n" + req_body
      else
        auth_str = CGI.unescape(request.path) + '?' + request.query_string + "\n" + req_body
      end
    
      valid = rsa.public_key.verify(
        OpenSSL::Digest::MD5.new, authorization, auth_str)
    
      if valid
        #body({'Status' => 'OK'}.to_json)
        body(hash_to_jason({'Status' => 'OK'}))
      else
        halt 400, "Authorization failed!"
      end
    end
    ```


