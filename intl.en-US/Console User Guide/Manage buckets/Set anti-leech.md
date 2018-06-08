# Set anti-leech {#concept_dwx_xxd_vdb .concept}

OSS is a Pay-As-You-Go service. To reduce extra fees caused in case your data on OSS is stolen by others, OSS supports anti-leech based on the referer field in the HTTP header.  You can configure a referer whitelist for a bucket and configure whether to allow access requests with an empty referer field.

## Procedure {#section_yd4_zyd_vdb .section}

1.  Log on to the [OSS console](https://oss.console.aliyun.com/).
2.  On the bucket list on the left, click the bucket you want to configure anti-leech to open the overview page of the bucket, as shown in the following figure:
3.  Click the **Basic Settings** tab, and click Edit in the **Anti-leech** area, as shown in the following figure:
4.  Enter the following information, and click **Save**, as shown in the following figure:
    -    **Referer** : Add one or more URLs into the whitelist.  Separate URLs with carriage returns.
    -    **Allow Empty Referer**: Configure whether to 
5.  allow empty referers.

## Example {#section_pbk_4b2_vdb .section}

Set the referer whitelist of a bucket named test-1-001 to http://www.aliyun.com. After the referer whitelist is set, only requests with a referer `http://www.aliyun.com` can access the objects in test-1-001 .

