# Set Cross-Origin Resource Sharing \(CORS\) {#concept_pbw_4df_vdb .concept}

OSS provides Cross-Origin Resource Sharing \(CORS\) in the HTML5 protocol to help users achieve cross-origin access. When the OSS receives a cross-origin request \(or OPTIONS request\), it reads the bucket’s CORS rules and then checks the relevant permissions. The OSS checks each rule sequentially, uses the first rule that matches to approve the request, and returns the corresponding header. If none of the rules match, the OSS does not attach any CORS header.

## Procedure {#section_dwn_ydf_vdb .section}

1.  Log on to the [OSS console](https://oss.console.aliyun.com/).
2.  In the left-side navigation pane, select the target bucket to open the bucket overview page.
3.  Click **Basic Settings**. In the **Cross-Origin Resource Sharing \(CORS\)** area, click **Edit**.
4.  In the cross-origin access page, click **Create Rule**.
5.  Configure the following items:
    -   **Source**: Indicates the origins allowed for cross-origin requests. Multiple matching rules are allowed, which are separated by a carriage return. Only one wildcard \(\*\) is allowed for each matching rule.
    -   **Allowed Methods**: Indicates the allowed cross-origin request methods.
    -   **Allowed Headers**: Indicates the allowed cross-origin request headers. Multiple matching rules are allowed, which are separated by a carriage return. Only one wildcard \(\*\) is allowed for each matching rule.
    -   **Exposed Headers**: Indicates the response headers that users are allowed to access from an application \(for example, a Javascript XMLHttpRequest object\).
    -   **Cache Time**: Indicates the cache time for the returned results of browser prefetch \(OPTIONS\) requests to a specific resource.

        **Note:** A maximum of 10 rules can be configured for each bucket.

6.  Click **OK**.

**Note:** You can also edit or delete an existing rule.

