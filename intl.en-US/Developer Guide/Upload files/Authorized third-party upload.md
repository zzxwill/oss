# Authorized third-party upload {#concept_dl2_3jb_5db .concept}

This topic describes how to authorize a third-party user to upload objects directly to OSS without the need to forward objects through the server.

## Scenarios {#section_ac3_jjb_5db .section}

In a standard client/server system architecture, the server is responsible for receiving and processing requests from the client. If OSS is used as a back-end storage service, the client sends objects to be uploaded to the application server, which then forwards them to OSS. In this process, the data needs to be transmitted twice, once from the client to the server and once from the server to OSS. In the case of bursts of access requests, the server must provide sufficient bandwidth resources for multiple clients to upload objects simultaneously. This presents a challenge to the architecture scalability.

To resolve this issue, OSS provides authorized third-party upload. By using this feature, each client can upload objects directly to OSS without transmitting them to the server. This reduces the cost for application servers and maximizes the OSS capability to process large amounts of data. In this case, you can focus on your business, without worrying about the bandwidth and concurrency limits.

Currently, OSS supports two methods to grant upload permissions: signed URL and temporary access credential.

## Signed URL {#section_eks_gkb_5db .section}

In this method, you can use a request URL that contains the OSSAccessKeyId and Signature fields to directly upload objects. Each signed URL has expiration time to ensure security.

-   Operating methods

    |Operating method|Description|
    |----------------|-----------|
    |[Java SDK](../../../../reseller.en-US/SDK Reference/Java/Authorized access.md#ul_nh5_wbd_kfb)|SDK demos in various languages|
    |[Python SDK](../../../../reseller.en-US/SDK Reference/Python/Authorized access.md#section_zx1_55k_kfb)|
    |[PHP SDK](../../../../reseller.en-US/SDK Reference/PHP/Authorized access.md#section_m2g_jwr_kfb)|
    |[Go SDK](../../../../reseller.en-US/SDK Reference/Go/Authorized access.md#section_ygd_qxw_kfb)|
    |[C SDK](../../../../reseller.en-US/SDK Reference/C/Authorized access.md#ul_hzs_jhz_kfb)|
    |[.NET SDK](../../../../reseller.en-US/SDK Reference/. NET/Authorized access.md#section_dpf_dp2_lfb)|

-   For more information, see [Add a signature to a URL](../../../../reseller.en-US/API Reference/Access control/Add a signature to a URL.md#).

## Temporary access credential {#section_dvv_hkb_5db .section}

Alibaba Cloud uses Security Token Service \(STS\) to grant temporary access credentials to authorize users. STS is a Web service that provides temporary access tokens for cloud computing users. Through STS, you can grant a third-party application or a RAM user \(who you can manage its user identity\) an access credential with a custom validity period and permissions.

-   Operating methods

    |Operating method|Description|
    |----------------|-----------|
    |[Java SDK](../../../../reseller.en-US/SDK Reference/Java/Authorized access.md#section_nyk_bzc_kfb)|SDK demos in various languages|
    |[Python SDK](../../../../reseller.en-US/SDK Reference/Python/Authorized access.md#section_zx1_55k_kfc)|
    |[PHP SDK](../../../../reseller.en-US/SDK Reference/PHP/Authorized access.md#section_m2g_jwr_kfc)|
    |[Go SDK](../../../../reseller.en-US/SDK Reference/Go/Authorized access.md#section_ygd_qxw_kfc)|
    |[C SDK](../../../../reseller.en-US/SDK Reference/C/Authorized access.md#section_nyk_bzc_kfb)|
    |[.NET SDK](../../../../reseller.en-US/SDK Reference/. NET/Authorized access.md#section_nyk_bzc_kfb)|
    |[Android SDK](../../../../reseller.en-US/SDK Reference/Android/Authorized access.md#ul_gsn_smg_lfb)|
    |[iOS SDK](../../../../reseller.en-US/SDK Reference/iOS/Authorized access.md#section_tpr_krj_lfb)|

-   For more information about operation examples, see [Access OSS with a temporary access token provided by STS](reseller.en-US/Developer Guide/Identity authentication/Access OSS with a temporary access token provided by STS.md#).

