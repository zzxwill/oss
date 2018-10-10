# Java {#concept_ahk_rfz_2fb .concept}

本文以Java语言为例，讲解在服务端通过Java代码完成签名，并且设置上传回调，然后通过表单直传数据到OSS。

## 前提条件 {#section_ikk_hgz_2fb .section}

-   应用服务器对应的域名可通过公网访问。
-   确保应用服务器已经安装`Java 1.6`以上版本（执行命令`java -version`进行查看）。
-   确保PC端浏览器支持JavaScript。

## 步骤一：配置应用服务器 {#section_q1n_ptj_gfb .section}

下载应用服务器源码\(Java版本\)：[aliyun-oss-appserver-java-master.zip](http://docs-aliyun.cn-hangzhou.oss.aliyun-inc.com/assets/attach/86983/APP_zh/1537973714934/aliyun-oss-appserver-java-master.zip)

将源码下载到应用服务器的硬盘，本示例中以`Ubuntu 16.04`为例，放置到`/home/aliyun/aliyun-oss-appserver-java`目录下。进入该目录，找到并打开源码文件`CallbackServer.java`，修改如下的代码片段：

```
String accessId = "<yourAccessKeyId>";      // 请填写您的AccessKeyId。
String accessKey = "<yourAccessKeySecret>"; // 请填写您的AccessKeySecret。
String endpoint = "oss-cn-hangzhou.aliyuncs.com"; // 请填写您的 endpoint。
String bucket = "bucket-name";                    // 请填写您的 bucketname 。
String host = "http://" + bucket + "." + endpoint; // host的格式为 bucketname.endpoint

// callbackUrl为 上传回调服务器的URL，请将下面的IP和Port配置为您自己的真实信息。
String callbackUrl = "http://88.88.88.88:8888";
String dir = "user-dir-prefix/"; // 用户上传文件时指定的前缀。
```

-   accessId ： 设置您的AccessKeyId。
-   accessKey ： 设置您的AessKeySecret。
-   host： 格式为`bucketname.endpoint`，例如`bucket-name.oss-cn-hangzhou.aliyuncs.com`。关于Endpoint的介绍，请参见[Endpoint访问域名](../../../../cn.zh-CN/开发指南/基本概念介绍.md#section_t3j_nmt_tdb)。
-   callbackUrl： 设置上传回调URL，即回调服务器地址，用于处理应用服务器与OSS之前的通信。OSS会在文件上传完成后，把文件上传信息通过此回调URL发送给应用服务器。本例中修改为：

    ```
    `String callbackUrl ="http://11.22.33.44:1234";`
    ```

-   dir： 设置上传到OSS文件的前缀，以便区别于其他文件从而避免冲突，您也可以填写空值。

## 步骤二：配置客户端 {#section_ez2_b5j_gfb .section}

下载客户端源码：[aliyun-oss-appserver-js-master.zip](http://docs-aliyun.cn-hangzhou.oss.aliyun-inc.com/assets/attach/86983/APP_zh/1537971352825/aliyun-oss-appserver-js-master.zip)

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

## 步骤三： 修改CORS {#section_bwk_f5j_gfb .section}

从浏览器向OSS发出的请求消息带有`Origin`的消息头，OSS对带有`Origin`头的请求消息会首先进行`跨域规则`的验证。

即客户端进行表单直接上传到OSS会产生跨域请求，需要为Bucket设置跨域规则（CORS），支持Post方法。

具体操作步骤请参见[设置跨域访问](../../../../cn.zh-CN/控制台用户指南/管理存储空间/设置跨域访问.md#)。

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/21672/153913847512308_zh-CN.png)

## 步骤四：体验上传回调 {#section_hsr_jhz_2fb .section}

-   启动应用服务器

    在`/home/aliyun/aliyun-oss-appserver-java`目录下，执行`mvn package`命令编译打包，然后执行命令启动应用服务器：

    ```
    `java -jar target/appservermaven-1.0.0.jar 1234`
    ```

    也可以在PC端使用`Eclipse/Intellij IDEA`等IDE工具导出`jar包`，然后将`jar包`拷贝到应用服务器，再执行`jar包`启动应用服务器。

-   启动客户端

    在PC侧的客户端源码目录中，打开`index.html` 文件。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/21672/153913847512306_zh-CN.png)

    单击**选择文件**，选择指定类型的文件，单击**开始上传**。上传成功后，显示回调服务器返回的内容。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/21672/153913847512309_zh-CN.png)


## 应用服务器核心代码解析 {#section_hjf_v5j_gfb .section}

应用服务器源码包含了`签名直传服务`和`上传回调服务`两个功能。

-   签名直传服务

    `签名直传服务`响应客户端发送给应用服务器的`GET`消息，代码片段如下：

    ```
    protected void doGet(HttpServletRequest request, HttpServletResponse response)
    			throws ServletException, IOException {
    
    		String accessId = "<yourAccessKeyId>"; // 请填写您的AccessKeyId。
    		String accessKey = "<yourAccessKeySecret>"; // 请填写您的AccessKeySecret。
    		String endpoint = "oss-cn-hangzhou.aliyuncs.com"; // 请填写您的 endpoint。
    		String bucket = "bucket-name"; // 请填写您的 bucketname 。
    		String host = "http://" + bucket + "." + endpoint; // host的格式为 bucketname.endpoint
    		// callbackUrl为 上传回调服务器的URL，请将下面的IP和Port配置为您自己的真实信息。
    		String callbackUrl = "http://88.88.88.88:8888";
    		String dir = "user-dir-prefix/"; // 用户上传文件时指定的前缀。
    
    		OSSClient client = new OSSClient(endpoint, accessId, accessKey);
    		try {
    			long expireTime = 30;
    			long expireEndTime = System.currentTimeMillis() + expireTime * 1000;
    			Date expiration = new Date(expireEndTime);
    			PolicyConditions policyConds = new PolicyConditions();
    			policyConds.addConditionItem(PolicyConditions.COND_CONTENT_LENGTH_RANGE, 0, 1048576000);
    			policyConds.addConditionItem(MatchMode.StartWith, PolicyConditions.COND_KEY, dir);
    
    			String postPolicy = client.generatePostPolicy(expiration, policyConds);
    			byte[] binaryData = postPolicy.getBytes("utf-8");
    			String encodedPolicy = BinaryUtil.toBase64String(binaryData);
    			String postSignature = client.calculatePostSignature(postPolicy);
    
    			Map<String, String> respMap = new LinkedHashMap<String, String>();
    			respMap.put("accessid", accessId);
    			respMap.put("policy", encodedPolicy);
    			respMap.put("signature", postSignature);
    			respMap.put("dir", dir);
    			respMap.put("host", host);
    			respMap.put("expire", String.valueOf(expireEndTime / 1000));
    			// respMap.put("expire", formatISO8601Date(expiration));
    
    			JSONObject jasonCallback = new JSONObject();
    			jasonCallback.put("callbackUrl", callbackUrl);
    			jasonCallback.put("callbackBody",
    					"filename=${object}&size=${size}&mimeType=${mimeType}&height=${imageInfo.height}&width=${imageInfo.width}");
    			jasonCallback.put("callbackBodyType", "application/x-www-form-urlencoded");
    			String base64CallbackBody = BinaryUtil.toBase64String(jasonCallback.toString().getBytes());
    			respMap.put("callback", base64CallbackBody);
    
    			JSONObject ja1 = JSONObject.fromObject(respMap);
    			// System.out.println(ja1.toString());
    			response.setHeader("Access-Control-Allow-Origin", "*");
    			response.setHeader("Access-Control-Allow-Methods", "GET, POST");
    			response(request, response, ja1.toString());
    
    		} catch (Exception e) {
    			// Assert.fail(e.getMessage());
    			System.out.println(e.getMessage());
    		}
    	}
    ```

-   上传回调服务

    `上传回调服务`响应OSS发送给应用服务器的`POST`消息，代码片段如下：

    ```
    protected boolean VerifyOSSCallbackRequest(HttpServletRequest request, String ossCallbackBody)
    			throws NumberFormatException, IOException {
    		boolean ret = false;
    		String autorizationInput = new String(request.getHeader("Authorization"));
    		String pubKeyInput = request.getHeader("x-oss-pub-key-url");
    		byte[] authorization = BinaryUtil.fromBase64String(autorizationInput);
    		byte[] pubKey = BinaryUtil.fromBase64String(pubKeyInput);
    		String pubKeyAddr = new String(pubKey);
    		if (!pubKeyAddr.startsWith("http://gosspublic.alicdn.com/")
    				&& !pubKeyAddr.startsWith("https://gosspublic.alicdn.com/")) {
    			System.out.println("pub key addr must be oss addrss");
    			return false;
    		}
    		String retString = executeGet(pubKeyAddr);
    		retString = retString.replace("-----BEGIN PUBLIC KEY-----", "");
    		retString = retString.replace("-----END PUBLIC KEY-----", "");
    		String queryString = request.getQueryString();
    		String uri = request.getRequestURI();
    		String decodeUri = java.net.URLDecoder.decode(uri, "UTF-8");
    		String authStr = decodeUri;
    		if (queryString != null && !queryString.equals("")) {
    			authStr += "?" + queryString;
    		}
    		authStr += "\n" + ossCallbackBody;
    		ret = doCheck(authStr, authorization, retString);
    		return ret;
    	}
    
    	protected void doPost(HttpServletRequest request, HttpServletResponse response)
    			throws ServletException, IOException {
    		String ossCallbackBody = GetPostBody(request.getInputStream(),
    				Integer.parseInt(request.getHeader("content-length")));
    		boolean ret = VerifyOSSCallbackRequest(request, ossCallbackBody);
    		System.out.println("verify result : " + ret);
    		// System.out.println("OSS Callback Body:" + ossCallbackBody);
    		if (ret) {
    			response(request, response, "{\"Status\":\"OK\"}", HttpServletResponse.SC_OK);
    		} else {
    			response(request, response, "{\"Status\":\"verdify not ok\"}", HttpServletResponse.SC_BAD_REQUEST);
    		}
    	}
    ```

    详情请参见API文档[Callback–回调签名](../../../../cn.zh-CN/API 参考/关于Object操作/Callback.md#section_btz_phx_wdb)。


