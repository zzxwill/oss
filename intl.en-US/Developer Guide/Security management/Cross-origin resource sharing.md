# Cross-origin resource sharing {#concept_bwn_tjd_5db .concept}

Cross-origin access, or the cross-origin of JavaScript, is a browser restriction set with the purpose of security, such as the same-origin policy.  When Website A tries to use the JavaScript code in its webpage to access Website B, the attempt is rejected by the browser because A and B are two websites of different origins.

Cross-origin access must be used frequently, such as when OSS is used at the backend for the user’s website www.a.com. The upload function implemented with JavaScript is provided in the webpage. However, requests could only be sent to www.a.com in the webpage, and all the requests sent to other websites are rejected by the browser. Thus the data uploaded by users has to be relayed to other sites through www.a.com. If cross-origin access is set, users could upload their data directly to OSS instead of relaying it through www.a.com.

## Cross-origin {#section_ifv_vjd_5db .section}

resource sharing \(CORS\) is the standard across-origin solution provided by HTML5. Currently, the CORS standard is supported by OSS for cross-origin access. For more information, see [W3C CORS Norms](http://www.w3.org/TR/cors/). CORS indicates the origin from where the request is originated

1.  by using a header containing the origin of the HTTP request. As in the earlier example, origin header contains www.a.com.
2.  After receiving the request, the server judges based on certain rules whether the request must be accepted or not.  If yes, then the server attaches the Access-Control-Allow-Origin header in the response. The header contains www.a.com, indicating that cross-origin access is allowed.   If the server accepts all the cross-origin requests, set the Access-Control-Allow-Origin header to \*.
3.  The browser determines whether the cross-origin request is successful or not based on whether the corresponding header has been returned or not. If no corresponding header is attached, the browser blocks the request.  If the request is not a simple one, the browser initially send an OPTIONS request to obtain the CORS configuration of the server. If the server does not support the following operations, the browser blocks the following requests.

OSS provides the configuration of the CORS rule, accepting or rejecting corresponding cross-origin requests as required.  The rule is configured at the bucket level.  The details are available in [PutBucketCORS](../intl.en-US/API Reference/Cross-Origin Resource Sharing/PutBucketcors.md#).

Key points

-   Attaching relevant CORS headers and other actions are automatically executed by the browser, and no additional action is required by the user. Only in the browser environment could the CORS operations be meaningful.
-   Whether a CORS request is accepted is completely independent of OSS authentication and other such measures, i.e. the OSS CORS rule is only used to determine whether to attach the relevant CORS headers.  Whether the request is required to be blocked or not, this can be exclusively determined by the browser.
-   When using cross-origin requests, make sure the browser’s cache function is enabled.   For example, the same cross-origin resource have been requested by two webpages running on the same browser \(originated from www.a.com and www.b.com\) at the same time respectively. If the request of www.a.com is received by the server initially, the server returns to the user the resource with the Access-Control-Allow-Origin header “www.a.com”.   When www.b.com initiates its request, the browser returns its previous cached request to the user. As the header content does not match the CORS request, the subsequent request fails.

## Reference {#section_c1b_xjd_5db .section}

-   API: [Introduction](../intl.en-US/API Reference/Cross-Origin Resource Sharing/Introduction.md#)
-   SDK: Java SDK-[CORS](https://www.alibabacloud.com/help/doc-detail/32018.htm)
-   Console: [Set CORS](../intl.en-US/Console User Guide/Manage buckets/Set Cross-Origin Resource Sharing (CORS).md#)

