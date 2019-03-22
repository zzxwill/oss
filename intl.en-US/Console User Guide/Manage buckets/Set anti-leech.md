# Set anti-leech {#concept_dwx_xxd_vdb .concept}

OSS is a Pay-As-You-Go service. To reduce extra fees caused in case your data on OSS is used by unauthorized users, OSS supports the anti-leech function based on the Referer header field in HTTP or HTTPS requests. You can configure a Referer whitelist for a bucket and configure whether to allow access requests with an empty Referer field in the OSS console.

## Procedure {#section_yd4_zyd_vdb .section}

1.  Log on to the [OSS console](https://oss.console.aliyun.com/).
2.  In the left-side the bucket list, click the bucket that you want to set anti-leech for.
3.  In the overview page of the bucket, click the **Basic Settings** tab, and then click **Configure** in the **Anti-Leech** area.
4.  Enter the following information:
    -    **Referer Whitelist**: Add one or more Referers into the whitelist and separate them with line breaks. Referers are usually URLs and support asterisks \(\*\) and question marks \(?\) as wildcards.

        If you set the Referer whitelist for a bucket named test-1-001 to http://www.aliyun.com, only requests in which the Referer field is `http://www.aliyun.com` can access the objects in the bucket test-1-001.

    -    **Allow Empty Referer**: Configure whether to allow access requests in which the Referer field is empty. If you does not allow empty referer, only HTTP or HTTPs request which include the Referer header can access the objects in the bucket.
5.  Click **Save**.

## Reference {#section_ksm_vfj_dhb .section}

-   For more information about the anti-leech function, see [Set anti-leech](../../../../../reseller.en-US/Developer Guide/Buckets/Anti-leech settings.md#).
-   For more information about the principle of anti-leech, see [Anti-leech](https://help.aliyun.com/document_detail/31937.html#concept-n5g-qd2-vdb).
-   For FAQs and troubleshooting for anti-leech, see [Referer](https://help.aliyun.com/document_detail/44198.html#concept-kh3-c3j-wdb).

