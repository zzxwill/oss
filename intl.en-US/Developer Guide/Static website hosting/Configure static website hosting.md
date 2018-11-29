# Configure static website hosting {#concept_ynd_phc_5db .concept}

In the [OSS console](https://oss.console.aliyun.com/), you can set up your buckets to work in static website hosting mode.

If your selected bucket is located in Hangzhou, after the configuration takes effect, the endpoint of the static website is as follows:

```
http://<Bucket>.oss-cn-hangzhou.aliyuncs.com/
```

**Note:** When you use an OSS endpoint in Mainland China regions or the Hongkong region to access a web file through the Internet , the Content-Disposition: 'attachment=filename;' is automatically added to the Response Header, and the web file is downloaded as an attachment. If you access OSS with a user domain, the Content-Disposition: 'attachment=filename;' will not be added to the Response Header. For more information about using the user domain to access OSS, see [Bind a custom domain name](intl.en-US/Developer Guide/Access and control/Bind a custom domain name.md#).

For users to manage static websites hosted on the OSS more easily, the OSS provides two functions:

-   Index Document Support

    The index document refers to the default index document \(such as index.html\) that is returned by the OSS when a user directly accesses the root domain name of the static website. If static website hosting mode is set for a bucket, you must specify the index document as an object in that bucket. This setting is required.

-   Error Document Support

    The error document refers to the error page the OSS returns to a user if the HTTP 4XX error \(such as 404 "NOT FOUND"\) occurs when the user attempts to access the static website but fails. If static website hosting mode is set for a bucket, you must specify the error document as an object in that bucket. This setting is optional.


For example, if a user sets:

-   The index document support as index.html
-   The error document support as error.html
-   The bucket as oss-sample
-   The endpoint as `oss-cn-hangzhou.aliyuncs.com`

Then:

-   When a user accesses `http://oss-sample.oss-cn-hangzhou.aliyuncs.com/` and `http://oss-sample.oss-cn-hangzhou.aliyuncs.com/directory/`, it is the same as accessing `http://oss-sample.oss-cn-hangzhou.aliyuncs.com/index.html`.
-   When a user accesses `http://oss-sample.oss-cn-hangzhou.aliyuncs.com/object`, and the object does not exist, OSS returns `http://oss-sample.oss-cn-hangzhou.aliyuncs.com/error.html`.

## Detail analysis {#section_ond_vhc_5db .section}

-   Static websites are websites with web pages composed of static content, including scripts such as JavaScript executed on the client. OSS does not support content that needs to be processed by the server, such as PHP, JSP, and ASP.NET content.
-   For access to a bucket-based static website through a user-defined domain name, see [Bind custom domain names](intl.en-US/Developer Guide/Access and control/Bind a custom domain name.md#).
-   When you set a bucket to static website hosting mode, you must specify an index page, the error page is optional.
-   When you set a bucket to static website hosting mode, the specified index page and error page must be an object in the bucket.
-   After a bucket is set to static website hosting mode, the OSS returns the index page for anonymous access to the root domain name of the static website, and returns Get Bucket results for signed access to the root domain name of the static website.
-   After a bucket is set to static website hosting mode, and the user accesses the root domain name of a static website or a nonexistent object, the OSS returns a specified object to the user and bills the return traffic and requests to the bucket owner.

## Reference {#section_ksm_xhc_5db .section}

-   API: [PutBucketWebsite](../../../../intl.en-US/API Reference/Bucket operations/PutBucketWebsite.md#)

-   Console: [Static website hosting](../../../../intl.en-US/Console User Guide/Manage buckets/Host a static website.md#)

-   Java SDK: [Static website hosting](https://www.alibabacloud.com/help/doc-detail/32020.htm)

