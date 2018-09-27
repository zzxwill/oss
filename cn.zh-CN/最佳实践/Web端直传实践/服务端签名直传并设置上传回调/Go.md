# Go {#concept_mhj_zzt_2fb .concept}

本文以Golang语言为例，讲解在服务端通过Golang代码完成签名，并且设置上传回调，然后通过表单直传数据到OSS。

## 前提条件 {#section_d5p_n15_2fb .section}

-   应用服务器对应的域名可通过公网访问。
-   应用服务器已经安装`Golang 1.6`以上版本（执行命令`go version`进行验证）。
-   确保PC端浏览器支持JavaScript。

## 步骤一：配置应用服务器 {#section_a5z_3mj_gfb .section}

下载应用服务器源码\(Go版本\)： [aliyun-oss-appserver-go-master.zip](http://docs-aliyun.cn-hangzhou.oss.aliyun-inc.com/assets/attach/86983/APP_zh/1538029675267/aliyun-oss-appserver-go-master.zip)

将源码下载到应用服务器的硬盘，本示例以`Ubuntu 16.04`为例，放置到`/home/aliyun/aliyun-oss-appserver-go`目录下。进入该目录，打开源码文件`appserver.go`，修改以下代码片段：

```
// 请填写您的AccessKeyId。
var accessKeyId string = "<yourAccessKeyId>"

// 请填写您的AccessKeySecret。
var accessKeySecret string = "<yourAccessKeySecret>"

// host的格式为 bucketname.endpoint ，请替换为您的真实信息。
var host string = "http://bucket-name.oss-cn-hangzhou.aliyuncs.com'"

// callbackUrl 为 上传回调服务器 的URL，请将下面的IP和Port配置为您自己的真实URL信息。
var callbackUrl string = "http://88.88.88.88:8888";

// 用户上传文件时指定的前缀。
var upload_dir string = "user-dir-prefix/"
```

-   accessKeyId ： 设置您的AccessKeyId。
-   accessKeySecret ： 设置您的AessKeySecret。
-   host ： 格式为`bucketname.endpoint`，例如`bucket-name.oss-cn-hangzhou.aliyuncs.com`。关于Endpoint的介绍，请参见[Endpoint访问域名](../../../../cn.zh-CN/开发指南/基本概念介绍.md#section_t3j_nmt_tdb)。
-   callbackUrl ： 设置上传回调URL，即回调服务器地址，用于处理应用服务器与OSS之前的通信。OSS会在文件上传完成后，把文件上传信息通过此回调URL发送给应用服务器。本例中修改为：

    ```
    `var callbackUrl string="http://11.22.33.44:1234";`
    ```

-   upload\_dir ： 指定上传文件的前缀，以便区别于其他文件以免发生冲突，您也可以填写空值。

## 步骤二：配置客户端 {#section_nrb_mnj_gfb .section}

下载客户端源码：[aliyun-oss-appserver-js-master.zip](http://docs-aliyun.cn-hangzhou.oss.aliyun-inc.com/assets/attach/86983/APP_zh/1537971352825/aliyun-oss-appserver-js-master.zip) 

下载客户端源码到PC侧的本地硬盘。本例中以`D:\aliyun\aliyun-oss-appserver-js`目录为例。

进入该目录，打开`upload.js`文件，找到下面的代码语句：

```
// serverUrl是 用户获取 '签名和Policy' 等信息的应用服务器的URL，请将下面的IP和Port配置为您自己的真实信息。
serverUrl ='http://88.88.88.88:8888'
```

将变量`severUrl`改成应用服务器的地址，客户端可以通过它来获取签名直传Policy等信息。如本例中可修改为：

```
// serverUrl是 用户获取 '签名和Policy' 等信息的应用服务器的URL，请将下面的IP和Port配置为您自己的真实信息。
serverUrl ='http://11.22.33.44:1234'
```

## 步骤三： 修改CORS {#section_wtf_df5_2fb .section}

从浏览器向OSS发出的请求消息带有`Origin`的消息头，OSS对带有`Origin`头的请求消息会首先进行`跨域规则`的验证。

即客户端进行表单直接上传到OSS会产生跨域请求，需要为Bucket设置跨域规则（CORS），支持Post方法。

具体操作步骤请参见[设置跨域访问](../../../../cn.zh-CN/控制台用户指南/管理存储空间/设置跨域访问.md#)。

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/21672/153803039212308_zh-CN.png)

## 步骤四：体验上传回调 {#section_upf_cf5_2fb .section}

-   启动应用服务器

    在`/home/aliyun/aliyun-oss-appserver-go`目录下，执行最简单的Go命令启动应用服务器：

    ```
    `go run appserver.go 11.22.33.44 1234`
    ```

-   启动客户端

    在PC侧的客户端源码目录中，打开`index.html` 文件。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/21672/153803039212306_zh-CN.png)

    单击**选择文件**，选择指定类型的文件之后，单击**开始上传**。上传成功后，显示回调服务器返回的内容。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/21672/153803039312309_zh-CN.png)


## 应用服务器核心代码解析 {#section_vr5_r4j_gfb .section}

应用服务器源码包含了`签名直传服务`和`上传回调服务`两个功能。

-   签名直传服务

    `签名直传服务`响应客户端发送给应用服务器的`GET`消息，代码片段如下：

    ```
    func handlerRequest(w http.ResponseWriter, r *http.Request) {   
            if (r.Method == "GET") {
                    response := get_policy_token()
                    w.Header().Set("Access-Control-Allow-Methods", "POST")
                    w.Header().Set("Access-Control-Allow-Origin", "*")
                    io.WriteString(w, response)
    		}
    ```

-   上传回调服务

    `上传回调服务`响应OSS发送给应用服务器的`POST`消息，代码片段如下：

    ```
    if (r.Method == "POST") {
                    fmt.Println("\nHandle Post Request ... ")
    
                    // Get PublicKey bytes
                    bytePublicKey, err := getPublicKey(r)
                    if (err != nil) {
                            responseFailed(w)
                            return
                    }
    
                    // Get Authorization bytes : decode from Base64String
                    byteAuthorization, err := getAuthorization(r)
                    if (err != nil) {
                            responseFailed(w)
                            return
                    }
    
                    // Get MD5 bytes from Newly Constructed Authrization String. 
                    byteMD5, err := getMD5FromNewAuthString(r)
                    if (err != nil) {
                            responseFailed(w)
                            return
                    }
    
                    // verifySignature and response to client 
                    if (verifySignature(bytePublicKey, byteMD5, byteAuthorization)) {
                            // do something you want accoding to callback_body ...
    
                            responseSuccess(w)  // response OK : 200  
                    } else {
                            responseFailed(w)   // response FAILED : 400 
                    }
            }
    ```


