# Manage back-to-origin configurations {#concept_n34_q1z_5db .concept}

After you set back-to-origin rules, OSS retrieves requested data from the origin in multiple ways to meet your requirements such as hot data migration and specific request redirection.

**Note:** For more information about how to set back-to-origin rules, see [Set back-to-origin rules](../../../../reseller.en-US/Console User Guide/Manage buckets/Set back-to-origin rules.md#).

OSS matches the URL of each GET request based on the preset rules, and retrieves the requested data from the origin accordingly. You can set a maximum of five rules. OSS matches requests based on the rules in sequence until a valid rule is matched. Mirroring back-to-origin and redirection back-to-origin are available.

## Mirroring {#section_s4b_s1z_5db .section}

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4379/15574524401580_en-US.png)

The process is as follows: If a client sends a GET request to request an object that does not exist in OSS, OSS requests the object from the origin. After obtaining the object, OSS returns it to the client and stores it to process subsequent requests.

**Scenarios**

Mirroring back-to-origin is used to seamlessly migrate data to OSS. You can migrate any service that is already running in a self-built origin or in another cloud product to OSS without service interruption. Specific scenarios are as follows:

-   The origin has an amount of cold data and is constantly generating hot data.

    You can use the migration tool [OssImport](../../../../reseller.en-US/Tools/ossimport/Architecture and configuration.md#) to migrate cold data to OSS and configure mirroring back-to-origin to set the origin URL in OSS. Even if some newly generated data is not migrated when you switch the domain of your application to OSS \(or Alibaba Cloud CDN, which retrieves data from OSS\), you can still access the data through OSS. The data is stored in OSS after the first access. After the domain is switched, the origin no longer generates data. You can scan the origin again and import data that is still not migrated to OSS at a time. Then, you can delete the mirroring back-to-origin configuration to complete data migration.

    If the configured origin URL is an IP address, OSS can still retrieve requested data from the origin after you switch the domain of your application to OSS. However, if the configured origin URL is a domain, mirroring back-to-origin does not work because the domain is resolved to OSS or CDN. In this case, you can apply for another domain as the origin and configure this domain to be resolved to the same IP address as the serving domain. In this way, after you switch the serving domain to OSS, OSS can still retrieve requested data from the origin.

-   Only some traffic of the origin is switched to OSS or CDN, and the origin continues to generate data.

    The migration method is similar to that described in the preceding scenario. After switching a portion of traffic to OSS, you do not need to delete the mirroring back-to-origin configuration. This ensures that OSS can still retrieve requested data from the origin for the traffic switched to OSS or CDN.


**Detail analysis**

-   OSS uses mirroring back-to-origin to request an object from the origin only when GetObject\(\) returns HTTP status code 404.
-   OSS uses `MirrorURL + object` as the back-to-origin URL to request data from the origin, where MirrorURL indicates the origin URL and object indicates the name of the requested object. For example, you configure mirroring back-to-origin for the bucket `example-bucket`, with MirrorURL set to `http://www.example-domain.com/`. The bucket does not include the object `image/example_object.jpg`. If a user needs to download this object, OSS sends a GET request to obtain the object from `http://www.example-domain.com/image/example_object.jpg`, stores the obtained object, and returns the object to the user. After the object is downloaded, it is available in OSS as `image/example_object.jpg`. This process functions the same as that of migrating an object with the same name from the origin to OSS. If MirrorURL carries path information, such as `http://www.example-domain.com/dir1/`, the process is the same as that in the preceding example. The back-to-origin URL is `http://www.example-domain.com/dir1/image/example_object.jpg`. OSS also obtains the object `image/example_object.jpg`. This process functions the same as that of migrating an object from a directory of the origin to OSS.
-   OSS does not send the header information transmitted by a user to the origin. Whether OSS sends the QueryString information to the origin depends on the back-to-origin configuration in the console.
-   If the origin returns chunked-encoded data, OSS also returns chunked-encoded data to the user.
-   OSS returns to the user and stores the following header information received from the origin:

    ```
    Content-Type
    Content-Encoding
    Content-Disposition
    Cache-Control
    Expires
    Content-Language
    Access-Control-Allow-Origin
    ```

-   When returning objects obtained through mirroring back-to-origin, OSS adds the x-oss-tag field to the response header and sets its value to MIRROR + Space + url\_decode \(back-to-origin URL\). The following code provides an example.

    ```
    x-oss-tag:MIRROR http%3a%2f%2fwww.example-domain.com%2fdir1%2fimage%2fexample_object.jpg
    ```

    After OSS obtains an object from the origin, so long as it is not overwritten, OSS adds the x-oss-tag field to the response header each time a user downloads the object. This header field indicates that the object is sourced from mirroring back-to-origin.

-   After OSS obtains an object through mirroring back-to-origin, if the corresponding object is modified in the origin, OSS does not update the obtained object. In this case, this object is already in OSS and does not meet the mirroring back-to-origin conditions.
-   If a requested object does not exist in the origin, the origin returns HTTP status code 404 to OSS, and OSS returns the same code to the user. If the origin returns another non-200 HTTP status code to indicate an error, such as object retrieval failure due to network-related causes, OSS returns HTTP status code 424 to the user, with the error code MirrorFailed.

## Redirection {#section_tcp_mbz_5db .section}

The URL redirection feature enables OSS to return a 3XX redirect based on user-defined conditions and corresponding redirection configuration. You can use this redirection feature to redirect objects and use various services accordingly. The following figure shows the process.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4379/15574524401591_en-US.png)

**Scenarios**

-   Seamless data migration from other data sources to OSS

    A user asynchronously migrates data from the user's own data source to OSS. In this process, if the user's client requests data that is not migrated to OSS, OSS returns a 302 redirect to the client through URL rewriting. Then, the client reads data from the user's data source based on the Location field in the 302 redirect.

-   Page redirection

    A user wants to hide objects with a certain prefix and return a specific page to visitors.

-   Redirect page for a 404 or 500 error

    If a 404 or 500 error occurs, users can be redirected to a preset page. This prevents OSS from exposing system errors to users.


