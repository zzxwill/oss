# Configure CORS rules {#concept_pbw_4df_vdb .concept}

OSS provides Cross-Origin Resource Sharing \(CORS\) in the HTML5 protocol to help users achieve cross-origin access. When OSS receives a cross-origin access request \(or OPTIONS request\) for a bucket, it reads the CORS rules for the bucket and then checks relevant permissions. OSS matches the rules with the request in sequence, uses the first rule that matches to allow the request, and returns the corresponding header. If none of the rules match the request, no CORS header is carried in the returned result.

## Procedure {#section_dwn_ydf_vdb .section}

1.  Log on to the [OSS console](https://partners-intl.console.aliyun.com/#/oss).
2.  In the left-side bucket list, click the name of the bucket that you want to configure CORS rules.
3.  Click the **Basic Settings** tab. In the **Cross-Origin Resource Sharing \(CORS\)** area, click **Configure**.
4.  Click **Create Rule**. In the CORS Rule dialog box, set the following parameters:![](images/33159_en-US.png)

    |Parameter|Required|Description|
    |:--------|:-------|:----------|
    |**Source**|Yes|Specifies the sources of allowed CORS requests. You can configure multiple matching rules for the source. Multiple rules must be configured in separate lines. Up to one asterisk \(\*\) wildcard can be used in a rule. If a rule includes only an asterisk \(\*\) wildcard, it allows CORS requests from all sources.|
    |**Allowed Methods**|Yes|Specifies the allowed CORS request methods.|
    |**Allowed Headers**|No|Specifies the response headers for the allowed CORS requests. You can configure multiple matching rules for the allowed headers. Multiple rules must be configured in separate lines. Up to one asterisk \(\*\) wildcard can be used in a rule.|
    |**Exposed Headers**|No|Specifies the response headers that users are allowed to access from an application \(for example, a Javascript XMLHttpRequest object\). Asterisks \(\*\) cannot be used in exposed headers.|
    |**Cache Timeout**|No|Specifies the cache time for the returned results of browser prefetch \(OPTIONS\) requests to a specific resource.|

    **Note:** You can configure up to 10 CORS rules for a bucket.

5.  Click **OK**.

    **Note:** You can also edit or delete an existing rule.


