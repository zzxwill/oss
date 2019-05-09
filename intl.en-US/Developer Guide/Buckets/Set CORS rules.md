# Set CORS rules {#concept_bwn_tjd_5db .concept}

Cross-origin resource sharing \(CORS\) is a standard cross-origin solution provided by HTML5. OSS uses the CORS standard for cross-origin access. You can use the PutBucketcors API of OSS to set CORS rules.

**Note:** 

-   For more information about the PutBucketcors API, see [CORS](../../../../reseller.en-US/API Reference/Cross-Origin Resource Sharing/Introduction.md#).
-   For more information about CORS rules, see [W3C CORS specification](http://www.w3.org/TR/cors/).

Browsers that support JavaScript use the same-origin policy to ensure security. When website A uses the JavaScript code on its Webpage to access website B of another origin, the browser rejects the request. In this case, you can set CORS rules to allow cross-origin requests.

## Operating methods {#section_elb_fjn_mgb .section}

|Operating method|Description|
|----------------|-----------|
|[Console](../../../../reseller.en-US/Console User Guide/Manage buckets/Set anti-leech.md#)|Web application, which is intuitive and easy to use|
|[Java SDK](../../../../reseller.en-US/SDK Reference/Java/CORS.md#)|SDK demos in various languages|
|[Python SDK](../../../../reseller.en-US/SDK Reference/Python/CORS.md#)|
|[PHP SDK](../../../../reseller.en-US/SDK Reference/PHP/CORS.md#)|
|[Go SDK](../../../../reseller.en-US/SDK Reference/Go/CORS.md#)|
|[C SDK](../../../../reseller.en-US/SDK Reference/C/CORS.md#)|
|[.NET SDK](../../../../reseller.en-US/SDK Reference/. NET/CORS.md#)|

## Scenarios {#section_xmb_m5b_5gb .section}

Cross-origin access is commonly used in actual scenarios.

For example, a user uses OSS at the back end of the website www.a.com. On the Webpage, objects can be uploaded by using JavaScript. However, requests on the upload Webpage can only be sent to www.a.com. The browser rejects all requests sent to other websites. As a result, data uploaded by the user must be transferred through www.a.com. If cross-origin access is configured, data can be directly uploaded to OSS, without the need to be transferred through www.a.com.

## CORS implementation {#section_ifv_vjd_5db .section}

CORS is implemented as follows:

1.  If CORS is enabled, an HTTP request contains the Origin field in its header to specify the origin. In the previous example, the Origin field is set to www.a.com.
2.  After receiving the request, the server determines whether to allow the request from the origin based on CORS rules. If the request is allowed, the server includes the Access-Control-Allow-Origin field whose value is www.a.com in the response header, indicating that this cross-origin access is allowed. If the server allows all cross-origin requests, it includes the Access-Control-Allow-Origin field whose value is an asterisk \(\*\) in the response header.
3.  The browser checks whether the Access-Control-Allow-Origin field is returned in the response header to determine whether the cross-origin request is allowed. If this field is not returned, the browser blocks the request. If the request is not a simple one, the browser first sends an OPTIONS request to obtain the CORS configuration of the server. If the server does not allow subsequent CORS operations, the browser blocks subsequent not-so-simple requests.

OSS allows you to set CORS rules so as to determine whether to allow or reject cross-origin requests as needed. You can set CORS rules at the bucket level. For more information, see [PutBucketcors](../../../../reseller.en-US/API Reference/Cross-Origin Resource Sharing/PutBucketcors.md#).

## Detail analysis {#section_zpx_f1l_w2b .section}

-   Browsers automatically include CORS-related header fields. You do not need to perform other operations. CORS operations are applicable only to browsers.
-   Whether a CORS request is allowed is completely independent of OSS authentication. OSS uses CORS rules only to determine whether CORS-related header fields are included. Browsers determine whether to block such a request.
-   Before sending cross-origin requests, you must ensure that the cache feature is disabled for browsers. For example, two Webpages in the same browser, one originated from www.a.com and the other from www.b.com, send a request for the same cross-origin resource simultaneously. If the server receives the request from www.a.com first, it returns the resource with the Access-Control-Allow-Origin field whose value is www.a.com in the response header. When the server receives the request from www.b.com later, the browser returns the cached resource with the Access-Control-Allow-Origin field whose value is www.a.com in the response header. Because the response header field value does not match CORS rules for the request from www.b.com, this request fails.

## FAQ {#section_c1b_xjd_5db .section}

To troubleshoot common errors, see [OSS CORS errors and troubleshooting](../../../../reseller.en-US/Errors and Troubleshooting/OSS CORS.md#).

