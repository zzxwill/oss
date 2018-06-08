# Cross-origin resource sharing \(CORS\) {#concept_vb1_2vd_vdb .concept}

## Same-origin policy {#section_wyh_fvd_vdb .section}

Cross-origin access, or cross-origin of JavaScript, is a type of browser restriction for security consideration, namely, the same-origin policy. When Website A tries to use the JavaScript code on its webpage to access Website B, the attempt is rejected by the browser because A and B are two websites of different origins.

However, cross-origin access is a commonly used on a day-to-day basis. For example, OSS is used at the backend for the website www.a.com. The JavaScript-based upload function is provided on the webpage. However, requests on the webpage are only sent to www.a.com, whereas all requests sent to other websites are rejected by the browser. As a result, user-uploaded data must be relayed to other sites through www.a.com. If cross-origin access is configured, data can be uploaded directly to OSS instead of relaying it through www.a.com.

## CORS overview {#section_gz5_gvd_vdb .section}

CORS is a standard cross-origin solution provided by HTML5. For the specific CORS rules, see [W3C CORS Norms](http://www.w3.org/TR/cors/).

CORS is a set of control policies followed by the browsers, which use HTTP headers for interaction. When identifying a request initiated as a cross-origin request, a browser adds the Origin header to the HTTP request and sends the request to the server. In the preceding example, the Origin header is www.a.com. After receiving the request, the server determines based on certain rules whether to permit the request. If the request is permitted, the server attaches the Access-Control-Allow-Origin header to the response. The header contains www.a.com, indicating that cross-origin access is allowed. In case, server permits all cross-origin requests, set the Access-Control-Allow-Origin header to \*. The browser determines whether the cross-origin request is successful based on whether the corresponding header is returned. If the corresponding header is not attached, the browser blocks the request. This is only a simple description.

The preceding content is a simple scenario. CORS norms classify requests into two types: simple requests and precheck requests. Precheck is a protection mechanism that prevents unauthorized requests from modifying resources. Before sending the actual request, the browser sends an OPTIONS HTTP request to determine whether the server permits the cross-origin request. If the request is not permitted, the browser rejects the actual request.

No precheck request is required only if both of the following conditions are met:

-   The request method is one of the following:

    -   GET
    -   HEAD
    -   POST
-   All headers are in the following lists:

    -   Cache-Control
    -   Content-Language
    -   Content-Type
    -   Expires
    -   Last-Modified
    -   Pragma

Precheck requests provide information about the subsequent request to the server, that includes:

-   Origin: Request origin information.
-   Access-Control-Request-Method: Type of the subsequent request, for example, POST or GET.
-   Access-Control-Request-Headers: List of headers explicitly set and included in the subsequent request.

After receiving the precheck request, the server determines whether to permit the cross-origin request based on the attached information. The return information is also sent using the following headers:

-   Access-Control-Allow-Origin: list of permitted origins for cross-origin requests
-   Access-Control-Allow-Methods: List of permitted cross-origin request methods.
-   Access-Control-Allow-Headers: List of permitted cross-origin request headers.
-   Access-Control-Expose-Headers: List of headers permitted to be exposed to JavaScript code.
-   Access-Control-Max-Age: Maximum browser cache time in seconds.

Based on the returned information, the browser determines whether to send the actual request. If none of these headers is received, the browser rejects the subsequent request.

**Note:** The preceding actions are performed automatically by the browser, and you can ignore the details. If the server is correctly configured, the process is the same for non-cross-origin requests.

Scenarios

Access permission control applies to browsers rather than servers, CORS is only applicable in scenarios where a browser is used. Hence, you do not need to worry about cross-origin issues when using other clients.

Applications that use CORS primarily, use Ajax in a browser to directly access OSS, instead of requiring traffic to be redirected through application servers. This applies to the upload and download processes. For websites powered by both OSS and Ajax technology, CORS is recommended for direct communication with OSS.

## OSS support for CORS {#section_b32_nvd_vdb .section}

OSS supports CORS rule configuration for permitting or rejecting corresponding cross-origin requests as required. CORS rules are configured at the bucket level. For more information, see [PutBucketCORS](../intl.en-US/API Reference/Cross-Origin Resource Sharing/PutBucketcors.md#). 

Whether a CORS request is permitted is independent of OSS identity verification. That is, the OSS CORS rules are only used to determine whether to attach relevant CORS headers. Whether the request is blocked is only determined by the browser.

When using cross-origin requests, make sure the browser's cache function is enabled. For example, the same cross-origin resource is requested respectively by two webpages in the same browser \(originated from www.a.com and www.b.com\) at the same time. If the request of www.a.com is received by the server in the first place, the server returns the resource with the Access-Control-Allow-Origin header "www.a.com". When www.b.com initiates its request, the browser returns its previous cached request. As the header content does not match the CORS request, the subsequent request fails.

**Note:** Currently, all OSS object-related interfaces provide CORS verification. In addition, multipart interfaces fully support CORS verification.

## Cross-origin GET request example {#section_tj2_rvd_vdb .section}

In this example, Ajax is used to retrieve data from OSS. For simplified description, all used buckets are public. The CORS configuration for accessing a private bucket is the same and only requires a signature to be attached to the request.

Getting started

Create a bucket. For example, create the bucket oss-cors-test with the access right set to public-read. Then create the text file named test.txt, and upload it to the bucket.

The test.txt access address is \`http://oss-cors-test.oss-cn-hangzhou.aliyuncs.com/test.txt\`.[http://oss-cors-test.oss-cn-hangzhou.aliyuncs.com/test.txt?spm=a2c4g.11186623.2.6.cdVyuR&file=test.txt](http://oss-cors-test.oss-cn-hangzhou.aliyuncs.com/test.txt?spm=a2c4g.11186623.2.6.cdVyuR&file=test.txt)

**Note:** Replace the following address with your test address.

Use curl to directly access the file:

```
curl http://oss-cors-test.oss-cn-hangzhou.aliyuncs.com/test.txt
just for test
```

The file can be accessed properly.

The following code describes how to directly access this website using Ajax. It is the simplest HTML code for access. You can copy the following code, save it as a local HTML file, and open it through your browser. Because no custom headers and hence are included, this request does not require a precheck.

```
<! DOCTYPE html> 
<html>
<head>
<script type="text/javascript" src="./functions.js"></script>
</head>
<body>
<script type="text/javascript">
// Create the XHR object.
function createCORSRequest(method, url) {
  var xhr = new XMLHttpRequest();
  if ("withCredentials" in xhr) {
    // XHR for Chrome/Firefox/Opera/Safari.
    xhr.open(method, url, true);
  } else if (typeof XDomainRequest ! = "undefined") {
    // XDomainRequest for IE.
    xhr = new XDomainRequest();
    xhr.open(method, url);
  } else {
    // CORS not supported.
    xhr = null;
  
  return xhr;

// Make the actual CORS request.
function makeCorsRequest() {
  // All HTML5 Rocks properties support CORS.
  var url = 'http://oss-cors-test.oss-cn-hangzhou.aliyuncs.com/test.txt';
  var xhr = createCORSRequest('GET', url);
  if (! xhr) {
    alert('CORS not supported');
    Return;
  
  // Response handlers.
  xhr.onload = function() {
    var text = xhr.responseText;
    var title = text;
    alert('Response from CORS request to ' + url + ': ' + title);
  
  xhr.onerror = function() {
    alert('Woops, there was an error making the request.');
  
  xhr.send();

</script>
<p align="center" style="font-size: 20px;">
<a href="#" onclick="makeCorsRequest(); return false;">Run Sample</a>
</p>
</body>
</html>
```

After opening the file, click the link \(Chrome is used in this example\). Check that the link cannot be accessed.

Use Chrome developer tools to identify the cause of the error.

The error is due to the fact that no Access-Control-Allow-Origin header is found. This is because the server is not configured with CORS.

Return to the header interface to check that the browser sends a request with an Origin header. Hence, the request is a cross-origin request. On Chrome, the origin is null because the file is a local file.

Configure Bucket CORS settings

Once the problem is located, you can configure CORS settings for the bucket to make sure successful execution of the preceding operation attempt. To facilitate understanding, the following describes how to configure CORS settings on the console. We recommend that CORS be configured on the console if CORS settings are not complex.

CORS settings are composed of individual rules. When the system looks for matches, each rule is checked as a match starting with the first rule. The first matched rule applies. The following shows how to add a rule with the loosest configuration:

This indicates that access is permitted to all origins, all request types, and all headers, and the maximum cache time is 1s.

Once the configuration is completed, perform the test again. The result is as follows:

Access requests can be sent properly.

If you are required to troubleshoot cross-origin access problems, you can configure CORS as shown in the preceding figure. This configuration permits all cross-origin requests. If an error occurs under this configuration, the error is not related to CORS.

Besides the loosest configuration, a more refined control mechanism can be configured for targeted control. The following shows the strictest configuration for a successful match:

In most cases, we recommend that you use the strictest configuration applicable in their use scenarios to guarantee maximum security at minimal configuration.

## Use cross-origin requests for POST upload {#section_md4_kzd_vdb .section}

The following provides a more complex example where a POST request with a signature is used, and the browser must send a precheck request.

[PostObjectSample](https://docs-aliyun.cn-hangzhou.oss.aliyun-inc.com/internal/oss/0.0.4/assets/sample/postobject.tar.gz)

**Note:** After downloading the preceding code, modify all the following sections to meet your requirements. Then run it on your server.

![](images/1670_en-US.png)

The following describes how to use the bucket oss-cors-test for testing. Before testing, delete all CORS rules to restore the configuration to its initial state.

Access this webpage and select a file to upload. 

Start the developer tools, and you can view the following content. Based on the previous GET example, it is easy to find the same cross-origin error. Different from the GET request, the request requires a precheck. As shown in the following figure, the operation fails because the OPTIONS response does not have CORS headers.

Modify the CORS configuration accordingly.

You can perform the operation again to get a successful result. The console displays the newly uploaded file.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4409/1675_en-US.png)

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4409/1676_en-US.png)

Test content:

```
$curl http://oss-cors-test.oss-cn-hangzhou.aliyuncs.com/events/1447312129218-test1.txt
post object test
```

CORS configuration caveats

CORS configuration items include:

-   Source: Provide the complete domain information during configuration, for example, `http://10.101.172.96:8001` as shown in the preceding figure.

    Do not omit the protocol name, for example, http. Include the port number if the default one has been changed. If you are not sure, use the browser's debugging function to view the Origin header. This field supports the wildcard \*, but only one such symbol can be used. You can perform configuration based on your needs.

-   Method: Select the allowed methods based on your requirements.
-   Allow Header: Indicates the list of allowed headers. To avoid header omission, we recommend that you set this field to \* unless otherwise specified. The header is not case-sensitive.
-   Expose Header: Indicates the list of headers exposed to the browser. Wildcards cannot be used. The specific configuration must be selected according to your application. Expose only required headers, for example, ETag headers. If you do not need to expose this information, you can leave this field blank. You can specify headers individually. This field is not case-sensitive.
-   Cache Time: In normal cases, you can set a relatively large value, for example, 60s.

The CORS configuration method sets individual rules for each origin that may access the service. If possible, do not include multiple origins in a single rule, and avoid overlap or conflict among multiple rules. For other permissions, you only need to grant the required permissions.

Troubleshooting advice

It is easy to mix up other errors with CORS errors when similar programs are debugged.

For example, when an access request is rejected because of any incorrect signature, the return result may not contain CORS header information because permission verification precedes CORS verification. In this case, some browsers directly report a CORS error, but the actual CORS configuration on the server is correct. The following two methods can be used to address the preceding problem:

-   View the HTTP request's return value. Because CORS verification is an independent process that does not affect core processes, a return value such as 403 is not produced by CORS. You must first rule out the program-related causes. If a precheck request is sent previously, you can view the precheck request results. If the correct CORS headers are returned, the actual request is permitted by the server. Therefore, the error can only be caused by another component.

-   Set the server's CORS configuration to the loosest setup shown in the preceding example. Use wildcards to permit all origins and request types. Then re-verify the configuration. If the verification still fails, it is possible that other type of errors have occured.


