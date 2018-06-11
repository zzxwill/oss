# Authorized third-party upload {#concept_dl2_3jb_5db .concept}

## Applicable scenarios {#section_ac3_jjb_5db .section}

In standard client/server system architecture, the server is used for receiving and processing requests from the client.  If OSS is used as a backend storage service, the client sends objects to the application server to upload, then forward, the objects to the OSS. In this process, the data need to be transmitted twice.  Regarding high access volume scenarios, the server requires high bandwidth resources to satisfy multiple clients’ simultaneous upload needs, challenging the architecture’s scalability.

To resolve this issue, OSS provides an authorized third-party upload function. This means each client can directly upload files to the OSS, bypassing the need for a server. This reduces the cost for application servers and takes full advantage of the OSS’s ability to process massive data volumes.

Currently, two methods are provided to grant upload permissions: URL signature and STS.

## URL signature {#section_eks_gkb_5db .section}

The URL signature method adds an OSS AccessKeyID and Signature fields to the request URL, allowing users to directly use this URL for an upload. Each URL signature has an expiration time to guarantee security.  For more information, see[Add a signature to the URL](../intl.en-US/API Reference/Access control/Add a signature to a URL.md#).

## Temporary access credentials {#section_dvv_hkb_5db .section}

Temporary access credentials are granted through the Alibaba Cloud Security Token Service and provide users with access authorization. For information on the implementation of temporary access credentials, see [STS Java SDK](https://www.alibabacloud.com/help/doc-detail/28786.htm). The process for temporary access credentials is as follows:

1.  The client initiates an authorized request to the server. The server verifies the legitimacy of the client. If it is a legitimate client, then the server uses its own accesskey to make a request to the STs for authorization. For more information, see [Access control](intl.en-US/Developer Guide/Access and control/Access control.md#).
2.  The server returns the obtained temporary credentials to the client.
3.  The client uses the obtained temporary credentials to initiate an upload request to OSS. For more information, see [Temporary authorization access](../intl.en-US/API Reference/Access control/Temporary authorized access.md#). The client can cache this credential for upload until the credential expires and then request new credentials from the server.

## Best practices {#section_pv2_jkb_5db .section}

-   [RAM and STS User Guide](../intl.en-US/Best Practices/Access control/Overview.md#)
-   [Web client direct data transfer and upload callback](../intl.en-US/Best Practices/Direct upload to OSS from Web/Overview of direct transfer on Web client.md#)

## Additional links {#section_oky_jkb_5db .section}

-   [Upload callback](intl.en-US/Developer Guide/Upload files/Upload callback.md#)
-   [Introduction to mobile development upload scenarios](intl.en-US/Developer Guide/Access OSS/OSS-based app development.md#)
-   [Download uploaded files](intl.en-US/Developer Guide/Download Files/Simple download.md#)
-   [Cloud processing for uploaded images](intl.en-US/Developer Guide/Image Processing.md#)
-   [Cloud Processing for uploaded audio and video files](intl.en-US/Developer Guide/Cloud data processing.md#)
-   [Access control for upload security](intl.en-US/Developer Guide/Access and control/Access control.md#)
-   [Form upload](../intl.en-US/Console User Guide/Manage objects/Upload objects.md#)

