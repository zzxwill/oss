# Upload callback {#concept_ywd_dlb_5db .concept}

After an object is uploaded, OSS can start a callback process for the application server. To request callback, you only need to send a request that contains relevant callback parameters to OSS.

**Note:** 

-   For more information about how to use OSS APIs to implement callbacks, see [Callback](../../../../reseller.en-US/API Reference/Object operations/Callback.md#).
-   APIs that support callbacks include [PutObject](../../../../reseller.en-US/API Reference/Object operations/PutObject.md#), [PostObject](../../../../reseller.en-US/API Reference/Object operations/PostObject.md#), and [CompleteMultipartUpload](../../../../reseller.en-US/API Reference/Multipart upload operations/CompleteMultipartUpload.md#).

## Scenarios {#section_rrw_2lb_5db .section}

Upload callback is typically used together with authorized third-party uploads. When uploading an object to OSS, the client requests a callback to the server. After completing the upload task of the client, OSS automatically sends an HTTP request for the callback to the application server to notify the server that the upload is completed. Then, the server performs operations such as modifying the database and responds to the callback request. After receiving the response, OSS returns the upload status to the client.

When sending a POST callback request to the application server, OSS includes parameters in the POST request body to carry specific information. Such parameters are divided into two types: system-defined parameters such as the bucket name and the object name, and user-defined parameters. You can specify user-defined parameters based on the application logic when sending a request that contains callback parameters to OSS. You can use user-defined parameters to carry information relevant to the application logic, such as the ID of the user who sends the request. For more information about how to use user-defined parameters, see [Callback](../../../../reseller.en-US/API Reference/Object operations/Callback.md#).

You can properly use upload callback to simplify the client logic and save network resources. The following figure shows the process.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4366/15573939831064_en-US.jpg)

**Note:** 

-   The following regions support upload callback: regions in Mainland China, Hong Kong, Singapore, Australia \(Sydney\), US \(Virginia\), US \(Silicon Valley\), Japan \(Tokyo\), Germany \(Frankfurt\), and UAE \(Dubai\).
-   Currently, only simple upload \(through the PutObject API\), form upload \(through the PostObject API\), and multipart upload \(through the CompleteMultipartUpload API\) support upload callback.

## Operating methods {#section_bdy_cv3_kgb .section}

|Operating method|Description|
|----------------|-----------|
|[Java SDK](../../../../reseller.en-US/SDK Reference/Java/Upload objects/Upload callback.md#)|SDK demos in various languages|
|[Python SDK](../../../../reseller.en-US/SDK Reference/Python/Upload objects/Upload callback.md#)|
|[PHP SDK](../../../../reseller.en-US/SDK Reference/PHP/Upload objects/Upload callback.md#)|
|[C SDK](../../../../reseller.en-US/SDK Reference/C/Upload objects/Upload callback.md#)|
|[.NET SDK](../../../../reseller.en-US/SDK Reference/. NET/Upload objects/Upload callback.md#)|

## Reference {#section_mb4_llb_5db .section}

-   [Upload callback errors and troubleshooting](../../../../reseller.en-US/Errors and Troubleshooting/Upload callback.md#)
-   [Direct data transfer on Web clients and upload callback](../../../../reseller.en-US//Overview of direct transfer on Web client.md#)
-   [Set up upload callback for mobile applications](../../../../reseller.en-US/Best Practices/Application server/Set up data callback for mobile apps.md#)

