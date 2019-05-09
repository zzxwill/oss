# Configure static website hosting {#concept_ynd_phc_5db .concept}

You can use the PutBucketWebsite API of OSS to set your bucket to the static website hosting mode and access the static website from the bucket endpoint.

**Note:** For more information about the PutBucketWebsite API, see [PutBucketWebsite](../../../../reseller.en-US/API Reference/Bucket operations/PutBucketWebsite.md#).

If your selected bucket is located in China \(Hangzhou\), after the configuration takes effect, the domain of the static website is as follows:

```
http://<Bucket>.oss-cn-hangzhou.aliyuncs.com/
```

**Note:** When you use the default endpoint to access a Webpage object in OSS in Mainland China or Hong Kong through the Internet, `Content-Disposition:'attachment=filename;'` is automatically added to the response header. That is, when you use a browser to access a Webpage object, the object is downloaded as an attachment. If you use a custom domain to access OSS, the information is not added to the response header. For more information about how to use a custom domain to access OSS, see [Bind a custom domain](reseller.en-US/Developer Guide/Buckets/Attach a custom domain name.md#).

OSS provides the following features to help you manage static websites hosted in OSS more easily:

-   Index document support

    The index document is the default index page \(such as index.html\) that OSS returns when a user directly accesses the root domain of a static website. If you set a bucket to the static website hosting mode, you must specify the index document.

-   Error document support

    The error document is an error page that OSS returns if an HTTP 4XX error \(such as 404 NOT FOUND\) occurs when a user accesses a static website. By specifying the error document, you can provide your users with appropriate error messages.


For example, you set the index document to index.html, the error document to error.html, the bucket name to oss-sample, and the endpoint to oss-cn-hangzhou.aliyuncs.com.

-   When a user accesses `http://oss-sample.oss-cn-hangzhou.aliyuncs.com/` and `http://oss-sample.oss-cn-hangzhou.aliyuncs.com/directory/`, the user actually accesses `http://oss-sample.oss-cn-hangzhou.aliyuncs.com/index.html`.

-   If the object does not exist when a user accesses `http://oss-sample.oss-cn-hangzhou.aliyuncs.com/object`, OSS returns `http://oss-sample.oss-cn-hangzhou.aliyuncs.com/error.html`.


## Operating methods {#section_bdy_cv3_kgb .section}

|Operating method|Description|
|----------------|-----------|
|[Console](../../../../reseller.en-US/Console User Guide/Manage buckets/Host a static website.md#)|Web application, which is intuitive and easy to use|
|[Java SDK](../../../../reseller.en-US/SDK Reference/Java/Static website hosting.md#)|SDK demos in various languages|
|[Python SDK](../../../../reseller.en-US/SDK Reference/Python/Static website hosting.md#)|
|[PHP SDK](../../../../reseller.en-US/SDK Reference/PHP/Static website hosting.md#)|
|[Go SDK](../../../../reseller.en-US/SDK Reference/Go/Static website hosting.md#)|
|[C SDK](../../../../reseller.en-US/SDK Reference/C/Static website hosting.md#)|
|[.NET SDK](../../../../reseller.en-US/SDK Reference/. NET/Static website hosting.md#)|
|[Node.js SDK](../../../../reseller.en-US/SDK Reference/Node. js/Static website hosting.md#)|
|[Ruby SDK](../../../../reseller.en-US/SDK Reference/Ruby/Static website hosting.md#)|

## Detail analysis {#section_ond_vhc_5db .section}

-   On a static website, all Webpages are composed of static content, including scripts such as JavaScript that are run on the client. OSS does not support content that needs to be processed by the server, such as PHP, JSP, and ASP.NET content.
-   To access a bucket-based static website by using a custom domain, you can [bind a custom domain](reseller.en-US/Developer Guide/Buckets/Attach a custom domain name.md#).
-   When you set a bucket to the static website hosting mode:

    -   The index document is required and the error document is optional.
    -   The specified index document and error document must be objects in the bucket.
    **Note:** If you use an Archive bucket, you must specify Standard objects or restored Archive objects as the index document and error document. Otherwise, the static website cannot be accessed.

-   After you set a bucket to the static website hosting mode:
    -   OSS returns the index document for anonymous access to the root domain of the static website, and returns the result of the GetBucket operation for authorized access to the root domain of the static website.
    -   OSS returns a specified object to users who access the root domain of a static website or an object that does not exist in OSS. OSS also charges a fee for such requests and the generated traffic.

## Reference {#section_z21_d2j_wgb .section}

-   [Tutorial: Host a static website using a custom domain name](reseller.en-US/Developer Guide/Static website hosting/Tutorial: Host a static website using a custom domain name.md#)
-   [Tutorial: Configure static website hosting](../../../../reseller.en-US/Best Practices/Bucket management/Static website hosting.md#)

