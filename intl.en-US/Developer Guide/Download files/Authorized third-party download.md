# Authorized third-party download {#concept_xpq_p1c_5db .concept}

To authorize a third-party user to download objects from a private bucket, you must use a signed URL or a temporary access credential. You cannot directly provide the AccessKey.

## Signed URL {#section_dkc_r1c_5db .section}

OSS allows users to use a signed URL to download data. You can add a signed URL and forward the URL to a third-party user to authorize access. The third-party user can then send an HTTP GET request and use the URL to download objects.

-   Implementation method

    The following example shows how to generate a signed URL.

    ```
    http://<bucket>.<region>.aliyuncs.com/<object>?OSSAccessKeyId=<user access_key_id>&Expires=<unix time>&Signature=<signature_string> 
    ```

    A signed URL must include at least the following three parameters: Signature, Expires, and OSSAccessKeyId.

    -   OSSAccessKeyId: The AccessKey ID of your Alibaba Cloud account.
    -   Expires: The expected expiration time of the URL.
    -   Signature: The signature string. For more information, see [Add a signature to a URL](../../../../reseller.en-US/API Reference/Access control/Add a signature to a URL.md#).

        **Note:** This link must be URL-encoded.

    -   Operating methods

        |Operating method|Description|
        |----------------|-----------|
        |[Console](../../../../reseller.en-US/Console User Guide/Upload、download and manage objects/Download an object.md#)|Web application, which is intuitive and easy to use|
        |[Java SDK](../../../../reseller.en-US/SDK Reference/Java/Authorized access.md#ul_nh5_wbd_kfb)|SDK demos in various languages|
        |[Python SDK](../../../../reseller.en-US/SDK Reference/Python/Authorized access.md#section_zx1_55k_kfb)|
        |[PHP SDK](../../../../reseller.en-US/SDK Reference/PHP/Authorized access.md#section_m2g_jwr_kfb)|
        |[Go SDK](../../../../reseller.en-US/SDK Reference/Go/Authorized access.md#section_ygd_qxw_kfb)|
        |[C SDK](../../../../reseller.en-US/SDK Reference/C/Authorized access.md#ul_hzs_jhz_kfb)|
        |[.NET SDK](../../../../reseller.en-US/SDK Reference/. NET/Authorized access.md#section_dpf_dp2_lfb)|


## Temporary access credential {#section_ut1_d2c_5db .section}

-   Implementation method

    A third-party user sends a request to the application server to obtain the AccessKey ID, AccessKey Secret, and STS Token issued by STS. The user then uses the obtained AccessKey ID, AccessKey Secret, and STS Token to request the developer's object resources.

-   Operating methods

    |Operating method|Description|
    |----------------|-----------|
    |[Console](../../../../reseller.en-US/Console User Guide/Upload、download and manage objects/Download an object.md#)|Web application, which is intuitive and easy to use|
    |[Java SDK](../../../../reseller.en-US/SDK Reference/Java/Authorized access.md#section_nyk_bzc_kfb)|SDK demos in various languages|
    |[Python SDK](../../../../reseller.en-US/SDK Reference/Python/Authorized access.md#section_zx1_55k_kfc)|
    |[PHP SDK](../../../../reseller.en-US/SDK Reference/PHP/Authorized access.md#section_m2g_jwr_kfc)|
    |[Go SDK](../../../../reseller.en-US/SDK Reference/Go/Authorized access.md#section_ygd_qxw_kfc)|
    |[C SDK](../../../../reseller.en-US/SDK Reference/C/Authorized access.md#section_nyk_bzc_kfb)|
    |[.NET SDK](../../../../reseller.en-US/SDK Reference/. NET/Authorized access.md#section_nyk_bzc_kfb)|
    |[Android SDK](../../../../reseller.en-US/SDK Reference/Android/Authorized access.md#ul_gsn_smg_lfb)|
    |[iOS SDK](../../../../reseller.en-US/SDK Reference/iOS/Authorized access.md#section_tpr_krj_lfb)|


## Best practices {#section_sdz_r2c_5db .section}

-   [RAM and STS User Guide](../../../../reseller.en-US/Developer Guide/Hide/Access control/Overview.md#)

