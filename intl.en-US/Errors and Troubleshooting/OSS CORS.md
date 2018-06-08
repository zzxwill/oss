# OSS CORS {#concept_ry5_l1j_wdb .concept}

For Cross Origin Resource Sharing \(CORS\) introduction and configuration, see CORS best practices.

## Configuration items {#section_a4y_n1j_wdb .section}

CORS configurations have the following items:

-   Origin \(AllowedOrigin\)

    Allowed origins for CORS request. Multiple origins can be specified at the same time. Complete domain information like `http://10.100.100.100:8001` or `https://www.aliyun.com` must be entered when this parameter is configured.  It must be noted that the protocol name http or https must not be omitted. If the port is not `80` by default, the port must also be configured. If the domain name cannot be determined, you can activate the debugging function of the browser to check the `Origin` in the header. The wildcard `*` can be used in the domain name and only one `*` can be used in each domain name, for example, `https://*.aliyun.com`. If `*` is specified as the origin, cross-domain requests of all origins are allowed.

-   Method

    Select the allowed methods as required. You can select all of them for the debugging process.

-   Allow Header

    The allowed cross-origin request header. Multiple match rules can be configured and must be separated with a carriage return. Each header specified by Access-Control-Request-Headers must match a value in Allowed Header. Header is readily missing. Unless otherwise specifically required, we recommend it is configured as `*` indicating that all of headers are allowed. The header is not case-sensitive.

-   Expose Header

    List of headers exposed to the browser, that is, the response headers allowing users to access from an application \(for example, a Javascript XMLHttpRequest object\). No wildcard is allowed. The specific configuration depends on the demands of the application. Only expose the required header. If you do not need to expose this information, you can leave this field blank. The header is not case-sensitive. This is an optional item.

-   Cache time \(MaxAgeSeconds\)

    The cache time for the results returned from browser prefetch requests \(OPTIONS requests\) for a specific resource. The unit is second. Normally, a relatively large value can be set for the cache time, for example, 60s. This is an optional item.


Generally, the CORS configuration method sets individual rules for each origin that may access the service. If possible, do not include multiple origins in a single rule, and avoid overlap or conflict among multiple rules. For other options, you need only to grant the required permissions.

## Troubleshooting {#section_zvm_q1j_wdb .section}

Error reporting

CORS configuration errors are reported as follows:

-   The browser reports the following errors:

    ```
    OPTIONS http://bucket.oss-cn-beijing.aliyuncs.com/
    XMLHttpRequest cannot load http://bucket.oss-cn-beijing.aliyuncs.com/. Response to preflight request doesn't pass access control check: No 'Access-Control-Allow-Origin' header is present on the requested resource. Origin '{yourwebsiet}' is therefore not allowed access. The response had HTTP status code 403.
    ```

-   The OSS reports the following errors:

    ```
    <Code>AccessForbidden</Code>
    <Message>CORSResponse: This CORS request is not allowed. This is usually because the evalution of Origin, request method / Access-Control-Request-Method or Access-Control-Requet-Headers are not whitelisted by the resource's CORS spec.</Message>
    ```


**Note:** 

-   CORS errors are generally caused by site applications. You can view the request details on the browser. Taking Chrome as an example, you can press `F12`  to open `Developer Tool` and then view corresponding elements on `Network`.
-   Errors returned from OSS can be obtained through packet capture. If Wireshark is used, you can specify `host host bucket-name.oss-cn-beijing.aliyuncs.com` as the filter.
-   Errors returned from OSS can also be obtained through the CORS debugging program [oss-h5-upload-js-direct](https://docs-aliyun.cn-hangzhou.oss.aliyun-inc.com/internal/oss/0.0.4/assets/sample/oss-h5-upload-js-direct.zip) .

For other errors, see [OSS 403 errors and troubleshooting](intl.en-US/Errors and Troubleshooting/OSS 403.md#).

Troubleshooting

Possible CORS errors include:

-   Origin \(AllowedOrigin\) is set incorrectly.
-   Method \(AllowedMethod\) is set incorrectly.
-   Allow Header is set incorrectly.
-   Expose Header is set incorrectly.

Debugging procedures:

-   Set Origin \(AllowedOrigin\) to `*` and confirm that this configuration item is correct. If upload is successful after this parameter is set to `*`, it means that Origin \(AllowedOrigin\) has been configured incorrectly and therefore needs to be checked carefully according to rules.
-   Select all options \(GET, PUT, DELETE, POST, and HEAD\) of Method \(AllowedMethod\) and confirm that this configuration item is correct.
-   Set Allow Header to `*` and confirm that this configuration item is correct.
-   Set Expose Header to a specified value or leave the field blank, and confirm that this configuration item is correct.

**Note:** On the OSS console, select**“Bucket” and configure the aforementioned items by navigation to Bucket Attribute \>** \> ** Cross Origin**Setting.

