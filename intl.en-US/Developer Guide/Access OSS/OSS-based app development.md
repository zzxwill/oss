# OSS-based app development {#concept_e4j_x51_5db .concept}

## Development sequence diagram {#section_azy_z51_5db .section}

Typical OSS-based app development involves the following four components:

-   OSS: Provides functions such as upload, download, and upload callback.
-   Developer's mobile client \(mobile application or webpage application, called the client for short\): Uses the service provided by the developer to access OSS.
-   Application server: Interacts with the client. This server is used for the developer's service.
-   Alibaba Cloud STS: Issues temporary credentials.

## Best practices {#section_ahj_bv1_5db .section}

-   [Set up direct data transfer for mobile apps](../intl.en-US/Best Practices/Application server/Set up direct data transfer for mobile apps.md#)
-   [Set up data callback for mobile apps](../intl.en-US/Best Practices/Application server/Set up data callback for mobile apps.md#)
-   [Permission control](../intl.en-US/Best Practices/Application server/Permission control.md#)

## Service development process {#section_ims_dv1_5db .section}

-   Data upload with temporary credential authorization

    The following figure shows the process of data upload with temporary credential authorization:

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4352/1027_en-US.png)

    The description of the process is as follows:

    1.  The client sends the application server the request of uploading data to OSS.
    2.  The application server sends a request to STS.
    3.  STS returns temporary credentials \(STS AccessKey and token\) to the application server.
    4.  The client obtains the authorization \(STS AccessKey and token\) and calls the mobile client SDK to upload data to OSS.
    5.  The client successfully uploads data to OSS. If callback is not set, the process is complete. If callback is set, OSS calls the relevant interface.
    Note:

    -   The client does not have to request authorization from the application server for each upload attempt. Each time the authorization is obtained, the client caches temporary credentials returned by STS until it expires.
    -   STS provides fine-grained access control for upload, which restricts client access permissions at the object level. This completely isolates the objects uploaded to OSS by different clients, and thus greatly enhances application security.
    For more information, see [Authorized third-party upload](intl.en-US/Developer Guide/Upload files/Authorized third-party upload.md#)

-   Data upload with signed URL or form authorization

    The following figure shows the process of data upload with signed URL or form authorization:

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4352/1030_en-US.png)

    The description of the process is as follows:

    1.  The client sends the application server the request of uploading data to OSS.
    2.  The application server returns credentials \(signed URL or form\) to the client.
    3.  The client obtains authorization \(signed URL or form\) and calls the mobile client SDK to upload data or uses form upload to directly upload data to OSS.
    4.  The client successfully uploads data to OSS. If callback is not set, the process is complete. If callback is set, OSS calls the relevant interface.
    For more information, see [Authorized third-party upload](intl.en-US/Developer Guide/Upload files/Authorized third-party upload.md#).

-   Data download with temporary credential authorization

    The process of data download with temporary credential authorization is similar to that of data download with temporary credential authorization:

    1.  The client sends the application server the request of downloading data from OSS.
    2.  The application server sends a request to STS and obtains temporary credentials \(STS AccessKey and token\).
    3.  The application server returns the temporary credentials \(STS AccessKey and token\) to the client.
    4.  The client obtains the authorization \(STS AccessKey and token\) and calls the mobile client SDK to download data from OSS.
    5.  The client successfully downloads data from OSS.
    Note:

    -   For download, the client also caches temporary credentials to increase access speed.
    -   STS also provides fine-grained access control for download. The access control for both upload and download helps to isolate the OSS storage space for each mobile client.
-   Data download with signed URL authorization

    The process of signed URL authorization for download is similar to that of signed URL authorization for upload:

    1.  The client sends the application server the request of downloading data from OSS.
    2.  The application server returns the signed URL to the client.
    3.  The client obtains authorization \(signed URL\) and calls the mobile client SDK to download data from OSS.
    4.  The client successfully downloads data from OSS.
    **Note:** The client cannot store the developer's accesskey, you can get only the URL signed by the application server or the temporary credentials issued via STS \(that is, the access key of the STS. and token \).


## Best practices {#section_sjx_ww1_5db .section}

-   [What is RAM and STS](../intl.en-US/Best Practices/Access control/Overview.md#)

## Reference {#section_ip1_zw1_5db .section}

-   Android SDK: [Upload objects](https://www.alibabacloud.com/help/doc-detail/32047.htm)
-   iOS SDK: [Upload objects](https://www.alibabacloud.com/help/doc-detail/32060.htm)

