# Go {#concept_mhj_zzt_2fb .concept}

本文以Golang语言为例，讲解在服务端通过Golang代码完成签名，并且设置上传回调，然后通过表单直传数据到OSS。

## 前提条件 {#section_d5p_n15_2fb .section}

-   应用服务器对应的域名可通过公网访问。
-   应用服务器已经安装Golang 1.6以上版本（执行命令`go version`进行验证）。
-   PC端浏览器支持JavaScript。

## 步骤 1：配置应用服务器 {#section_a5z_3mj_gfb .section}

1.  下载应用服务器源码（Go版本）到应用服务器的目录下。下载地址： [aliyun-oss-appserver-go-master.zip](http://docs-aliyun.cn-hangzhou.oss.aliyun-inc.com/assets/attach/86983/APP_zh/1538029675267/aliyun-oss-appserver-go-master.zip)
2.  以Ubuntu 16.04为例，将源码放置到`/home/aliyun/aliyun-oss-appserver-go`目录下。
3.  进入该目录，打开源码文件`appserver.go`，修改以下代码片段：

```
// 请填写您的AccessKeyId。
var accessKeyId string = "<yourAccessKeyId>"

// 请填写您的AccessKeySecret。
var accessKeySecret string = "<yourAccessKeySecret>"

// host的格式为 bucketname.endpoint，请替换为您的真实信息。
var host string = "http://bucket-name.oss-cn-hangzhou.aliyuncs.com'"

// callbackUrl 为上传回调服务器的URL，请将下面的IP和Port配置为您自己的真实信息。
var callbackUrl string = "http://88.88.88.88:8888";

// 上传文件时指定的前缀。
var upload_dir string = "user-dir-prefix/"

// 上传策略Policy的失效时间，单位为秒。
var expire_time int64 = 30
```

-   accessKeyId：设置您的AccessKeyId。
-   accessKeySecret：设置您的AessKeySecret。
-   host：格式为`bucketname.endpoint`，例如`bucket-name.oss-cn-hangzhou.aliyuncs.com`。关于Endpoint的介绍，请参见[Endpoint访问域名](../../../../cn.zh-CN/开发指南/基本概念介绍.md#section_t3j_nmt_tdb)。
-   callbackUrl：设置上传回调URL，即回调服务器地址，用于处理应用服务器与OSS之前的通信。OSS会在文件上传完成后，把文件上传信息通过此回调URL发送给应用服务器。本例中修改为：

    ```
    `var callbackUrl string="http://11.22.33.44:1234";`
    ```

-   upload\_dir：指定上传文件的前缀，以免与其他文件发生冲突，您也可以填写空值。

## 步骤 2：配置客户端 {#section_nrb_mnj_gfb .section}

1.  下载客户端源码到PC侧的本地目录。下载地址：[aliyun-oss-appserver-js-master.zip](http://docs-aliyun.cn-hangzhou.oss.aliyun-inc.com/assets/attach/86983/APP_zh/1537971352825/aliyun-oss-appserver-js-master.zip)
2.  以`D:\aliyun\aliyun-oss-appserver-js`目录为例。进入该目录，打开`upload.js`文件，找到下面的代码语句：

    ```
    // serverUrl是用户获取 '签名和Policy' 等信息的应用服务器的URL，请将下面的IP和Port配置为您自己的真实信息。
    serverUrl ='http://88.88.88.88:8888'
    ```

3.  将变量`severUrl`修改为应用服务器的地址。如本例中修改为：

    ```
    // serverUrl是用户获取 '签名和Policy' 等信息的应用服务器的URL，请将下面的IP和Port配置为您自己的真实信息。
    serverUrl ='http://11.22.33.44:1234'
    ```


## 步骤 3： 修改CORS {#section_wtf_df5_2fb .section}

客户端进行表单直传到OSS时，会从浏览器向OSS发送带有`Origin`的请求消息。OSS对带有`Origin`头的请求消息会进行跨域规则（CORS）的验证。因此需要为Bucket设置跨域规则以支持Post方法。

具体操作步骤请参见[设置跨域访问](../../../../cn.zh-CN/控制台用户指南/管理存储空间/设置跨域访问.md#)。

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/21672/153913844612308_zh-CN.png)

## 步骤 4：体验上传回调 {#section_upf_cf5_2fb .section}

1.  启动应用服务器。

    在`/home/aliyun/aliyun-oss-appserver-go`目录下，执行Go命令：

    go run appserver.go 11.22.33.44 1234

2.  启动客户端。
    1.  在PC侧的客户端源码目录中，打开`index.html` 文件。

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/21672/153913844612306_zh-CN.png)

    2.  单击**选择文件**，选择指定类型的文件之后，单击**开始上传**。上传成功后，显示回调服务器返回的内容。

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/21672/153913844612309_zh-CN.png)


## 应用服务器核心代码解析 {#section_vr5_r4j_gfb .section}

应用服务器源码包含了签名直传服务和上传回调服务两个功能。

-   签名直传服务

    签名直传服务响应客户端发送给应用服务器的GET消息，代码片段如下：

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

    上传回调服务响应OSS发送给应用服务器的POST消息，代码片段如下：

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

    详情请参见API文档[Callback–回调签名](../../../../cn.zh-CN/API 参考/关于Object操作/Callback.md#section_btz_phx_wdb)。


