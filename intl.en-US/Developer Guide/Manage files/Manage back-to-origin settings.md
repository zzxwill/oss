# Manage back-to-origin settings {#concept_n34_q1z_5db .concept}

Configuring back-to-origin rules allows OSS to retrieve requested data from origin sites in multiple ways, meeting the requirements of frequently accessed \(hot\) data migration and specific request redirection requirements.

This setting enables the URL of each OSS GET request to be matched, which then specifies an origin retrieval method.  A maximum of five rules can be configured. Requests are compared to the rules in a set sequence, until matched to a valid rule.  The specified method can be either mirroring or redirection.

## Mirroring {#section_s4b_s1z_5db .section}

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4379/15483090091580_en-US.png)

The process is as follows: A client requests data of an object. OSS determines the object does not exist, and forwards the request to the source URL. The source URL returns the object through the OSS, which goes to the client. OSS simultaneously writes the object data to process future requests.

**Example scenario**

Mirroring write-back is designed to seamlessly migrate data to OSS. This means any service that is already running on a user-established site, or on another cloud product, can be migrated to OSS without interruption to the service. A detailed scenario is as follows:

-   An origin site is generating new hot data, and also has legacy cold data stored.

    First, a user can use the migration tool [ossimport](../../../../../reseller.en-US/Tools/ossimport/Architecture and configuration.md#) to migrate cold data to the OSS.   During migration, the user can configure mirroring write-back and set the origin site’s URL to OSS. Even if some newly generated data does not migrate when the domain name is switched to the OSS, the user can still access it through OSS and the files are saved to OSS after they have been accessed for the first time. After switching the domain name for an origin site that no longer produces new data, the site is scanned, and all non-migrated data is imported to the OSS. In this situation, the user may disable mirroring write-back.

    If the configured origin site is an IP address, after the domain name is migrated to the OSS, data can still be mirrored to the origin site. 

-   However, if it is a domain name, no mirroring can be produced because the domain name is resolved to the OSS or CDN. In this situation, the user can apply for another domain name to mirror the origin site. 

    This domain name and the in-service domain name would both be resolved to the same IP address. This allows origin site imaging to continue when the service domain name is migrated.


**Usage rules**

-   OSS only executes mirroring write-back to request an object from the origin site when GetObject\(\) returns a 404 code.
-   The URL requested from the origin site is `MirrorURL+object`and the name of the file written back to the OSS is object. For example, assume that: A bucket is named`example-bucket` . Mirroring write-back is configured. The MirrorURL is `http://www.example-domain.com/`. The file `object.jpg`does not exist in this bucket. To download the file, the OSS initiates a GET request to `http://www.example.com/object.jpg`, records the result, and returns it to the user. The file is then available on OSS as `object.jpg`. This is the same as migrating an object with the same name to the OSS. Note that if the MirrorURL carries path information, such as `http://www.example-domain.com/dir1/`, the process is the same as the preceding example, but the OSS origin retrieval URL is `http://www.example-domain.com/dir1/image/example_object.jpg` although the object written to the OSS remains as `object.jpg`. This process is the same as migrating an object from an origin site directory to the OSS.
-   The header and querystring information transmitted to the OSS is not sent to the origin site.
-   If the origin site returns data in chunks, the OSS returns data to the user in chunks.
-   The OSS returns and saves the following header information from the origin site:

    ```
    Content-Type
    Content-Encoding
    Content-Disposition
    Cache-Control
    Expires
    Content-Language
    Access-Control-Allow-Origin
    ```

-   An x-oss-tag response header is added to mirroring write-back files, with the value “MIRROR” + space + url\_decode \(origin retrieval URL\). In the proceeding example, this would be

    ```
    x-oss-tag:MIRROR http%3a%2f%2fwww.example-domain.com%2fdir1%2fimage%2fexample_object.jpg
    ```

    After the file is written back to the OSS, so long as it is not overwritten again, this header is added each time it is downloaded to indicate that it is taken from mirroring.

-   Assuming that the file has already been written to the OSS through mirroring write-back, if the corresponding file on the origin site is changed, the OSS does not update the file that exists on the OSS because this file which is already present on the OSS does not meet the mirroring write-back conditions.
-   If the file does not exist in the mirroring source, the returned result is the HTTP status 404, which is forwarded through the OSS to the user. If the mirroring source returns another non-200 status code \(including file retrieval failure due to network-related causes\), the OSS returns 424 to the user, the error code for `MirrorFailed`.

## Redirection {#section_tcp_mbz_5db .section}

The URL redirection function returns a 3xx hop to the user based on user-defined conditions and corresponding hop configurations. Users can use this hop function to redirect files and provide various services based on this action. The process is as follows:

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4379/15483090091591_en-US.png)

Application scenarios

-   Migrate data sources to OSS

    Users can asynchronously migrate data to the OSS. In this way, requests for un-migrated data use the URL rewrite method to return a 302 redirect request to the user. The user’s client then returns the data from the user’s data source based on the location in the 302 redirect request.

-   Configure page redirect function

    If a user wants to hide objects using a certain header prefix, a customized page can be displayed to visitors.

-   Configure the redirected page when a 404 or 500 error occurs

    If a 404 or 500 error occurs, the user can be redirected to a live page. This makes sure that OSS errors are undetected by a user.


## Reference {#section_zsq_rbz_5db .section}

-   Console: [Origin retrieval Rule Management](../../../../../reseller.en-US/Console User Guide/Manage buckets/Set back-to-origin rules.md#)

