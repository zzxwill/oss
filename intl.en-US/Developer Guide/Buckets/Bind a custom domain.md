# Bind a custom domain {#concept_rz2_xg5_tdb .concept}

After you upload an object to a bucket in OSS, a URL is automatically generated for the object. You can use this URL to access the object in the bucket. To access an uploaded object by using a custom domain, you must bind the custom domain to the bucket where the object is stored and add a CNAME record that directs you to the Internet endpoint of the bucket.

## Operating methods {#section_tkz_t55_vgb .section}

|Operating method|Description|
|----------------|-----------|
|[Console](../../../../reseller.en-US/Console User Guide/Manage buckets/Manage a domain/Attach a custom domain name.md#)|Web application, which is intuitive and easy to use|
|[PHP SDK](../../../../reseller.en-US/SDK Reference/PHP/Bind a custom domain name.md#)|SDK demos in various languages|
|[Node.js SDK](../../../../reseller.en-US/SDK Reference/Node. js/CNAME.md#)|
|[Browser.js SDK](../../../../reseller.en-US/SDK Reference/Browser.js/CNAME.md#)|
|[Ruby SDK](../../../../reseller.en-US/SDK Reference/Ruby/CNAME.md#)|

## Common concepts {#section_slr_q55_cgb .section}

Before binding a custom domain, you must understand the following concepts:

-   Custom domain: the domain that you buy from a DNS service provider.
-   OSS endpoint or bucket endpoint: the domain that OSS assigns to your bucket. You can use this domain to access the resources in your bucket. To access an OSS bucket by using your user domain, you must bind the user domain to the OSS endpoint, that is, add a CNAME record to Alibaba Cloud Domain Name System \(DNS\).
-   Alibaba Cloud CDN domain: The accelerating domain that Alibaba Cloud Content Delivery Network \(CDN\) assigns to your user domain. To use the CDN acceleration service to access the resources in your bucket, you must bind your user domain to a CDN accelerating domain, that is, add a CNAME record to Alibaba Cloud DNS.
-   Auto CDN cache update: If you update an object in your bucket but cache duration of the object cached on the CDN node does not expire, users can only access the object that is not updated. In this case, you must manually update the object cache on the CDN node. To simplify operations, OSS provides the auto CDN cache update feature. After you enable this feature, all updates on the objects in your bucket are automatically synchronized to the CDN node. For more information, see [Enable auto CDN cache update](reseller.en-US/Console User Guide/Manage buckets/Manage a domain/Attach a CDN acceleration domain name.md#cdncache).

## Scenarios {#section_wwj_1gv_tdb .section}

For example, user A has a website with the domain `img.abc.com`, and the website contains an image with the URL of `http://img.abc.com/logo.png`. For easier management, user A wants to redirect all requests for the image to OSS without modifying the code, that is, keep the URL of the image unchanged. In this case, user A can bind a custom domain. The process is as follows:

1.  User A creates a bucket named abc-img in OSS and uploads the image to the bucket.
2.  User A binds the custom domain `img.abc.com` to abc-img in the OSS console.
3.  After `img.abc.com` is bound to abc-img, OSS maps the custom domain to the bucket.
4.  User A adds a CNAME rule on the DNS server to map `img.abc.com` to `abc-img.oss-cn-hangzhou.aliyuncs.com`, which is the OSS endpoint of abc-img.
5.  After receiving a request for `http://img.abc.com/logo.png`, OSS redirects the request to abc-img based on the mapping relationship between `img.abc.com` and abc-img. That is, users who access the image with the URL of `http://img.abc.com/logo.png` are redirected to the following URL: `http://abc-img.oss-cn-hangzhou.aliyuncs.com/logo.png`.

The following table compares the access processes before and after user A binds the custom domain.

| |Before user A binds the custom domain|After user A binds the custom domain|
|:-|:------------------------------------|:-----------------------------------|
|Access process| 1.  A user sends a request for `http://img.abc.com/logo.png`.
2.  DNS resolves the IP address of user A's server from the request.
3.  The user accesses the image logo.png on user A's server.

 | 1.  A user sends a request for `http://img.abc.com/logo.png`.
2.  DNS resolves the OSS endpoint `abc-img.oss-cn-hangzhou.aliyuncs.com` from the request.
3.  The user accesses the image logo.png in the OSS bucket abc-img.

 |

## Reference {#section_bxj_1gv_tdb .section}

-   To set CDN acceleration, see [Bind a CDN accelerating domain](reseller.en-US/Console User Guide/Manage buckets/Manage a domain/Attach a CDN acceleration domain name.md#).
-   To access OSS resources from a static Webpage, see [Configure static website hosting](reseller.en-US/Console User Guide/Manage buckets/Host a static website.md#) and [Tutorial: Use a custom domain to configure static website hosting](reseller.en-US/Developer Guide/Static website hosting/Tutorial: Host a static website using a custom domain name.md#).
-   To access OSS resources through HTTPS, see [Certificate hosting](../../../../reseller.en-US/Console User Guide/Manage buckets/Manage a domain/Certificate hosting.md#).

