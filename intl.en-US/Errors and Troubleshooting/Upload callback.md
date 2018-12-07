# Upload callback {#concept_tj5_cv3_wdb .concept}

## About upload callback {#section_jpf_dv3_wdb .section}

When a file is uploaded, the OSS can provide a [Callback](../../../../reseller.en-US/Developer Guide/Upload files/Upload callback.md#) to your callback server.  You can carry the relevant callback parameters in the upload request to implement the upload callback.  The APIs that support upload callback are [PutObject](../../../../reseller.en-US/API Reference/Object operations/PutObject.md#), [PostObject](../../../../reseller.en-US/API Reference/Object operations/PostObject.md#), and [CompleteMultipartUpload](../../../../reseller.en-US/API Reference/Multipart upload operations/CompleteMultipartUpload.md#). For more information, see [Developer Guide](../../../../reseller.en-US/Developer Guide/Upload files/Upload callback.md#) and [API Reference](../../../../reseller.en-US/API Reference/Object operations/Callback.md#).

**Note:** A callback server is also called a service server.

## Application scenario {#section_j5p_fv3_wdb .section}

-   Notification

    A typical application is to upload and callback by an authorized third party who specifies the callback parameters during file upload.  After the upload is complete, the OSS sends a callback request to the callback server.  When receiving the callback request, the callback server records the upload information.

-   Processing, review, and statistics

    When receiving a callback request, the callback server processes, reviews, and makes statistics on the uploaded files.


## Data stream {#section_m3w_gv3_wdb .section}

The following table describes the data streams.

|Data stream|Meaning|Description|
|:----------|:------|:----------|
|1|The client uploads a file and carries a callback parameter.\[1\]|The upload is implemented by SDK \([PutObject](../../../../reseller.en-US/API Reference/Object operations/PutObject.md#) and [CompleteMultipartUpload](../../../../reseller.en-US/API Reference/Multipart upload operations/CompleteMultipartUpload.md#), and the callback by the [PostObject](../../../../reseller.en-US/API Reference/Object operations/PostObject.md#) API.|
|2|The OSS instance stores the file and initiates a callback.|The OSS instance sends a `POST` \[2\] request to the specified  `CallbackUrl` in the upload request. The callback time-out period is five seconds \[3\].|
|3| The callback server returns the processing result.|-   The message body returned by the callback server must be in JSON format. 
-   The OSS determines that the callback fails if the returned result is not 200.\[4\]

 |
|4|The OSS returns the upload and callback result.|--   If both the upload and callback succeed,  `200` is returned. 
-   If the upload succeeds but the callback fails, `203` is returned. The value of ErrorCode is  `CallbackFailed`, and ErrorMessage indicates the error cause.

|

**Note:** 

-   \[1\] For more information about the format, see `SDK/PostObject`.
-   \[2\] For more information about the format of the callback request POST, see [Initiate a callback request](../../../../reseller.en-US/API Reference/Object operations/Callback.md#).
-   \[3\] The time-out time is fixed and cannot be configured.
-   \[4\] `40x` is returned for parameter invalidation or authentication failure, while `50x` is returned for time-out or connection failure.

## SDK/PostObject {#section_q1w_zv3_wdb .section}

During the file upload, you can set the callback parameters to specify the URL of the callback server, data to be sent to the callback server, and data format. When the callback server processes a callback, some context information, such as the `bucket` and `object`, is specified using system variables. Other context information is specified using custom variables.

Upload callback parameters

The following parameters are available for an upload callback:

|Field|Meaning|Description|
|:----|:------|:----------|
|callbackUrl|Callback server address|Required|
|callbackHost|Value of the `Host` in the callback request message header|Optional. The default value is `callbackUrl`.|
|callbackBody|Callback request message body|Required. It can hold system variables and custom variables.|
|callbackBodyType|Value of `Content-Type` in the callback request message header, that is, the `callbackBody` data format|Optional. It can be `application/x-www-form-urlencoded` \(default\) or `application/json`.|

Upload callback parameters are carried by the upload request in either of the following two ways:

-   The callback parameters are carried by `x-oss-callback` in the message header. This is a common and recommended way.
-   The callback parameters are carried by `callback` in QueryString.

Rules for generating the `x-oss-callback` or `callback`   values are as follows:

```
Callback := Base64(CallbackJson)
CallbackJson := '{' CallbackUrlItem, CallbackBodyItem [, CallbackHostItem, CallbackBodyTypeItem] '}' 
CallbackUrlItem := '"'callbackUrl'"' ':' '"'CallbackUrlValue'"'
CallbackBodyItem := '"'callbackBody'"' ':' '"'CallbackBodyValue'"'
CallbackHostItem := '"'callbackHost'"' ':' '"'CallbackHostValue'"'
CallbackBodyTypeItem := '"'callbackBodyType'"' : '"'CallbackBodyType'"'
CallbackBodyType := application/x-www-form-urlencoded | application/json
```

`CallbackJson` value examples are as follows:

```

    "callbackUrl" : "http://abc.com/test.php",
    "callbackHost" : "oss-cn-hangzhou.aliyuncs.com",
    "callbackBody" : "{\"bucket\":${mimeType}, \"object\":${object},\"size\":${size},\"mimeType\":${mimeType},\"my_var\":${x:my_var}}",
    "callbackBodyType" : "application/json"

```

or

```

    "callbackUrl" : "http://abc.com/test.php",
    "callbackBody" : "bucket=${bucket}&object=${object}&etag=${etag}&size=${size}&mimeType=${mimeType}&my_var=${x:my_var}"

```

## System variables and custom variables {#section_syh_pw3_wdb .section}

Variables for `CallbackJson`, such as `${bucket}`,  `${object}`, and `${size}`, in the `CallbackJson` example are the OSS-defined system variables. During the callback, the OSS replaces the system variables with actual values. The following table lists the OSS-defined system variables.

|Variable|Meaning|
|:-------|:------|
|$\{bucket\}|Storage space name|
|$\{object\}|File name|
|$\{etag\}|File’s etag|
|$\{size\}|File size|
|$\{mimeType\}|File type, such as image/jpeg|
|$\{imageInfo.height\}|Image height|
|$\{imageInfo.width\}|Image width|
|$\{imageInfo.format\}|Image format, such as .jpg and .png|

**Note:** 

-   The system variables are case sensitive.
-   The system variable format must be `${bucket}`.
-   imageInfo is set for images. For the non-image format, the value of imageInfo is blank.

Variables for `CallbackJson`, such as `${x:my_var}` , in the  `CallbackJson` example are the custom variables. During the callback, the OSS replaces the custom variables with custom values.  Custom variable values are defined and carried by the upload request in either of the following two ways:

-   The custom variables are carried by `x-oss-callback-var` in the message header. This is a common and recommended way.
-   The custom variables are carried by `callback-var` in QueryString.

Rules for generating the `x-oss-callback-var` or `callback-var` values are as follows:

```
CallbackVar := Base64(CallbackVarJson)
CallbackVarJson := '{' CallbackVarItem [, CallbackVarItem]* '}'
CallbackVarItem := '"''x:'VarName'"' : '"'VarValue'"'
```

`CallbackVarJson` value examples are as follows:

```

    "x:my_var1" : "value1",
    "x:my_var2" : "value2"

```

**Note:** 

-   The custom variables must start with ***x:***: They are case sensitive and in the format of  `${x:my_var}`.
-   The custom variable length is limited by the length of the message header and URL. We recommend that the number of the custom variables do not exceed 10 and the total length do not exceed 512 bytes.

## SDK usage example {#section_gn4_xw3_wdb .section}

Some SDKs, such as JAVA and JS, encapsulate the preceding steps. Some SDKs, such as Python, PHP, and C, need to use the preceding rules to generate the upload callback parameters and custom variables.  The following table lists SDK usage examples.

|SDK|Upload callback example|Description:|
|:--|:----------------------|:-----------|
|JAVA|[CallbackSample.java](https://github.com/aliyun/aliyun-oss-java-sdk/blob/master/src/samples/CallbackSample.java)|Note the escape characters in `CallbackBody`.|
|Python|[object\_callback.py](https://github.com/aliyun/aliyun-oss-python-sdk/blob/master/examples/object_callback.py)|-|
|PHP|[Callback.php](https://github.com/aliyun/aliyun-oss-php-sdk/blob/master/samples/Callback.php)|`OSS_CALLBACK` and `OSS_CALLBACK_VAR` in $options do not need to be encoded using Base64, which is implemented by the SDK.|
|C \#|[UploadCallbackSample.cs](https://github.com/aliyun/aliyun-oss-csharp-sdk/blob/master/samples/Samples/UploadCallbackSample.cs)|Use *using* to read  `to read, ` `PutObjectResult.ResponseStream` but make sure that it is disabled.|
|JS|[object.test.js](https://github.com/ali-sdk/ali-oss/blob/master/test/node/object.test.js)|-|
|C|[oss\_callback\_sample.c](https://github.com/aliyun/aliyun-oss-c-sdk/blob/master/oss_c_sdk_sample/oss_callback_sample.c)|-|
|Ruby|[callback.rb](https://github.com/aliyun/aliyun-oss-ruby-sdk/blob/master/examples/aliyun/oss/callback.rb)|-|
|iOS| [Callback notification after upload](https://partners-intl.aliyun.com/help/doc-detail/32060.htm)

 |Make sure that `<var1>` the format of`var1` is x:var1.|
|Andriod| [Callback notification after upload](https://partners-intl.aliyun.com/help/doc-detail/32047.htm)

 |Note the escape characters in `CallbackBody`.|

**Note:** The Go SDK does not support upload callback currently.

## PostObject usage example {#section_sd1_bx3_wdb .section}

PostObject supports the upload callback, whose callback parameters are carried by the form field `callback` and custom variables are carried by an independent form field. For more information, see[PostObjet](../../../../reseller.en-US/API Reference/Object operations/PostObject.md#).

The following table lists PostObject usage examples.

|SDK|Upload callback example|
|:--|:----------------------|
|Java|[PostObjectSample.java](https://github.com/aliyun/aliyun-oss-java-sdk/blob/master/src/samples/PostObjectSample.java)|
|Python|[object\_post.py](https://github.com/aliyun/aliyun-oss-python-sdk/blob/master/examples/object_post.py)|
|C\#|[PostPolicySample.cs](https://github.com/aliyun/aliyun-oss-csharp-sdk/blob/master/samples/Samples/PostPolicySample.cs)|

## Callback server {#section_jgd_dx3_wdb .section}

The callback server is an HTTP server that processes callback requests and POST messages sent from the OSS. The callback server URL is the value of the upload callback parameter  `callbackUrl`. You can implement your own processing logic on the callback server for recording, review, processing, and statistics of the uploaded data.

Callback signature

The callback server needs to verify the signature of a POST request to make sure that the POST request is from the OSS upload callback.  The callback server also can directly process the message without verifying the signature. To enhance the security of the callback server, we recommend that the callback server verify the message signature. For more information about the callback signature rules, see [Callback signature](../../../../reseller.en-US/API Reference/Object operations/Callback.md#).

**Note:** The OSS callback server example describes how to implement signature verification. We recommend that you directly use the code.

Message processing

The main logic of the callback server is to process the OSS callback request. Note the following items:

-   The callback server must process the POST request of the OSS.
-   The OSS callback time-out time is five seconds. Therefore, the callback server must complete processing within five seconds and return the result.
-   The message body sent from the callback server to the OSS must be in JSON format.
-   The callback server uses its own logic, and the OSS provides examples instead of the specific service logic.

Implementation example

The following table describes the implementation examples of the callback server.

|Language|Example|Running method|
|:-------|:------|:-------------|
|JAVA|[AppCallbackServer.zip](https://gosspublic.alicdn.com/images/AppCallbackServer.zip)|Decompress the package and run `java -jar oss-callback-server-demo.jar 9000`.|
|PHP|[callback-php-demo.zip](https://gosspublic.alicdn.com/callback-php-demo.zip)|Deploy and run the program to in **Apache** environment.|
|Python|[callback\_app\_server.py.zip](https://gosspublic.alicdn.com/images/callback_app_server.py.zip)|Decompress the package and run `python callback_app_server.py`.|
|Ruby|[oss-callback-server](https://github.com/rockuw/oss-callback-server)|Run `ruby aliyun_oss_callback_server.rb`.|

## Debugging procedure {#section_fyl_qx3_wdb .section}

The upload callback debugging includes debugging of the client that uploads a file and the callback server that processes the callback. We recommend that you debug the client first and then the callback server. After independently debugging the two parts, perform the complete upload callback.

-   Client debugging

    You can use the callback server `http://oss-demo.aliyuncs.com:23450` provided by the OSS, that is, the callback parameter `callbackUrl` to debug the client. The callback server only verifies the callback request signature, and does not process the callback request. For callback requests whose signatures are successfully verified, the callback server returns  `{"Status":"OK"}`. For callback requests whose signatures fail to be verified, the callback server returns `400 Bad Request`. For non-POST requests, the callback server returns `501 Unsupported method`. For more information about the code of the callback server example, see [callback\_app\_server.py.zip](https://gosspublic.alicdn.com/images/callback_app_server.py.zip).

-   Callback server debugging

    The callback server is an HTTP server that can process the POST request. You can modify the callback server based on the example provided by the OSS or implement it by yourself. The following table describes the examples of the callback server provided by the OSS.

    |Language|Example|Running method|
    |:-------|:------|:-------------|
    |JAVA|[AppCallbackServer.zip](https://gosspublic.alicdn.com/images/AppCallbackServer.zip)|Decompress the package and run `java -jar oss-callback-server-demo.jar 9000`.|
    |PHP|[callback-php-demo.zip](https://gosspublic.alicdn.com/callback-php-demo.zip)|Deploy and run the program to in **Apache** environment|
    |Python|[callback\_app\_server.py.zip](https://gosspublic.alicdn.com/images/callback_app_server.py.zip)|Decompress the package and run `python callback_app_server.py`.|
    |C\#|[callback-server-dotnet.zip](http://docs-aliyun.cn-hangzhou.oss.aliyun-inc.com/assets/attach/31989/cn_zh/1501048926621/callback-server-dotnet.zip)|Compile the program and run `aliyun-oss-net-callback-server.exe 127.0.0.1 80`.|
    |Go|[callback-server-go.zip](http://docs-aliyun.cn-hangzhou.oss.aliyun-inc.com/assets/attach/31989/cn_zh/1501048745465/callback-server-go.zip)|Compile the program and run `aliyun_oss_callback_server`.|
    |Ruby|[oss-callback-server](https://github.com/rockuw/oss-callback-server)|Run `ruby aliyun_oss_callback_server.rb`.|

    The callback server can be debugged by running the cURL command. The following commands may be used:

    ```
    # Run the following command to send a `POST` request whose message body is `object=test_obj` to the callback server:  
    curl -d "object=test_obj" http://oss-demo.aliyuncs.com:23450 -v
    # Run the following command to send a `POST` request whose message body is `post.txt` to the callback server:  
    curl -d @post.txt http://oss-demo.aliyuncs.com:23450 -v
    # Run the following command to send a `POST` request whose message body is `post.txt` and which carries the specified message header `Content-Type` to the callback server:
    curl -d @post.txt -H "Content-Type: application/json" http://oss-demo.aliyuncs.com:23450 -v
    ```

    **Note:** 

    -   When debugging the callback server, ignore signature verification because it is difficult for `cURL` to simulate the signature function.
    -   The OSS example already provides the signature verification function. We recommend that you directly use it.
    -   We recommend that the callback server provide the logging function to record all messages, facilitating debugging and tracking.
    -   After correctly processing a callback request, the callback server must return `200` instead of 20x.
    -   The message body sent from the callback server to the OSS must be in JSON format, and `Content-Type`  is set to`application/json`.

## Common errors and causes {#section_sxy_by3_wdb .section}

-   InvalidArgument

    ```
    <Error>
      <Code>InvalidArgument</Code>
      <Message>The callback configuration is not json format.</Message>
      <RequestId>587C79A3DD373E2676F73ECE</RequestId>
      <HostId>bucket.oss-cn-hangzhou.aliyuncs.com</HostId>
      <ArgumentName>callback</ArgumentName>
      <ArgumentValue>{"callbackUrl":"8.8.8.8:9090","callbackBody":"{"bucket":${bucket},"object":${object}}","callbackBodyType":"application/json"}</ArgumentValue>
    </Error>
    ```

    Cause: 

    The callback parameter settings are incorrect, or the parameter format is incorrect. The common error is that the callback parameters in `ArgumentValue` are not in valid  JSON format. In JSON, `\` and `"` are escape characters. For example,  `"callbackBody":"{"bucket":${bucket},"object":${object}}"` must be `"callbackBody":"{\"bucket\":${bucket},\"object\":${object}}"`. For more information about the SDKs, see the upload callback examples in the [SDK usage example](#section_gn4_xw3_wdb) part.

    |Character after escape|Character before escape|
    |:---------------------|:----------------------|
    |\\\\|\\\\\\\\|
    |“|\\\\\\”|
    |\\b|\\\\b|
    |\\f|\\\\f|
    |\\n|\\\\n|
    |\\r|\\\\r|
    |\\t|\\\\t|

-   CallbackFailed

    ```
    <Error>
      <Code>CallbackFailed</Code>
      <Message>Response body is not valid json format.</Message>
      <RequestId>587C81A125F797621829923D</RequestId>
      <HostId>bucket.oss-cn-hangzhou.aliyuncs.com</HostId>
    </Error>
    ```

    Cause: 

    The message body sent from the callback server to the OSS is not in JSON format. You can confirm the content by running  `curl -d "<Content>" <CallbackServerURL> -v` or capture packets. We recommend that you use Wireshark to capture packets in Windows, and use tcpdump to capture packets in Linux. Invalid returned messages include: `OK` and `\357\273\277{"Status":"OK"}` \(the BOM header containing the `ef bb bf` bytes\).

    ```
    <Error>
      <Code>CallbackFailed</Code>
      <Message>Error status : -1. OSS can not connect to your callbackUrl, please check it.</Message>
      <RequestId>587C8735355BE8694A8E9100</RequestId>
      <HostId>bucket.oss-cn-hangzhou.aliyuncs.com</HostId>
    </Error>
    ```

    Cause: 

    The processing time of the callback server exceeds  five seconds. Therefore, the OSS determines that a time-out occurs. We recommend that you modify the processing logic of the callback server to asynchronous processing to make sure that it can complete processing within five seconds and returns the result to the OSS.

    ```
    <Error>
      <Code>CallbackFailed</Code>
      <Message> error status:-1 8.8.8.8: 9090 reply timeout, cost: 5000 MS, timeout: 5000 MS (Ernest-4, errno170) </message>
      <RequestId>587C8D382AE0B92FA3EEF62C</RequestId>
      <HostId>bucket.oss-cn-hangzhou.aliyuncs.com</HostId>
    </Error>
    ```

    Cause: 

    The processing time of the callback server exceeds five seconds. Therefore, the OSS determines that a time-out occurs.

    ```
    <Error>
      <Code>CallbackFailed</Code>
      <Message>Error status : 400.</Message>
      <RequestId>587C89A02AE0B92FA3C7981D</RequestId>
      <HostId>bucket.oss-cn-hangzhou.aliyuncs.com</HostId>
    </Error>
    ```

    Cause: 

    The status code of the message sent from the callback server to the OSS is `400`. Check the processing logic of the callback server.

    ```
    <Error>
      <Code>CallbackFailed</Code>
      <Message>Error status : 502.</Message>
      <RequestId>587C8D382AE0B92FA3EEF62C</RequestId>
      <HostId>bucket.oss-cn-hangzhou.aliyuncs.com</HostId>
    </Error>
    ```

    Cause: 

    The callback server is not started, `CallbackUrl` is missing in the callback parameters, or the network between the OSS instance and the callback server is disconnected. We recommend that you deploy the callback server on the ECS, which belongs to the same intranet as the OSS, to save the traffic cost and guarantee the network quality.


## Common links {#section_q4w_nz3_wdb .section}

-   [Callback Guide](../../../../reseller.en-US/Developer Guide/Upload files/Upload callback.md#)
-   [Callback API](../../../../reseller.en-US/API Reference/Object operations/Callback.md#)

