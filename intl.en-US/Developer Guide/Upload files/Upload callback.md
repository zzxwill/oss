# Upload callback {#concept_ywd_dlb_5db .concept}

## Use cases {#section_rrw_2lb_5db .section}

When an object upload is completed, the OSS can provide a callback to the application server.  To implement the callback, you only need to attach the relevant Callback parameter to the request sent to the OSS. APIs that currently support callbacks include PutObject, PostObject, and CompleteMultipartUpload.

A typical use case for upload callback is to work with the upload by an authorized third-party. The client specifies the callback of the server when it uploads objects to the OSS. After the upload task of the client is completed in the OSS, the OSS automatically initiates an HTTP request for the callback to the application server. This promptly notifies the server that the upload is completed, so the server can complete operations such as database modifications. When the callback request receives a response from the server, the OSS returns the status to the client.

When the OSS sends a POST callback request to the application server, the POST request’s body contains some parameters that carry certain information. Such parameters are divided into two types: system-defined parameters \(such as bucket name and object name\) and user-defined parameters. You can specify user-defined parameters based on the application logic when sending a request including callback to the OSS.  You can use user-defined parameters to carry information relevant to the application logic, such as the user ID of the request initiator. For information on user-defined parameter usage, see [Callback](../../../../reseller.en-US/API Reference/Object operations/Callback.md#).

Appropriate use of the upload callback can decrease the complexity of the client’s logic and reduce the consumption of network resources. The process is as follows:

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4366/15382926411064_en-US.jpg)

**Note:** 

-   Supported regions include Mainland China regions, Hong Kong region, Asia Pacific South 1, Asia Pacific SE 2, US East, US West, Asia Pacific Northeast 1, Middle Europe 1 and Middle East 1.
-   Currently only simple uploads \(PutObject\), form uploads \(PostObject\) and multipart uploads \(Complete Multipart  Upload\) operations support upload callback.

## Reference {#section_mb4_llb_5db .section}

-   API: [Callback](../../../../reseller.en-US/API Reference/Object operations/Callback.md#)
-   SDK: iOS [Callback notification after upload](https://partners-intl.aliyun.com/help/doc-detail/32060.htm)

## Best practices {#section_g1q_mlb_5db .section}

-   [Direct data transfer practices on web clients and upload callback](../../../../reseller.en-US/Best Practices/Direct upload to OSS from Web/Overview of direct transfer on Web client.md#)

-   [How to build a callback application server \(sample code is available for download\)](../../../../reseller.en-US/Best Practices/Application server/Set up data callback for mobile apps.md#)


