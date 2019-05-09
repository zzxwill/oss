# Configure hotlinking protection {#concept_s5b_gjd_5db .concept}

You can use the PutBucketReferer API of OSS to set the referer, so as to prevent unauthorized users from using your OSS data.

**Note:** For more information about the PutBucketReferer API, see [PutBucketReferer](../../../../reseller.en-US/API Reference/Bucket operations/PutBucketReferer.md#).

To configure hotlinking protection, you need to set the following parameters:

-   Referer Whitelist: the referer whitelist that specifies the domains that are allowed to access OSS resources.
-   Allow Empty Referer: indicates whether the Referer field can be left empty in a request. If the Referer field cannot be left empty, users must include the Referer field in the HTTP or HTTPS request header so as to access OSS resources.

For example, you add `https://www.aliyun.com/` to the referer whitelist of a bucket named oss-example. Then, only users who set the Referer field to `https://www.aliyun.com/` in their requests can access the objects in this bucket.

## Operating methods {#section_elb_fjn_mgb .section}

|Operating method|Description|
|----------------|-----------|
|[Console](../../../../reseller.en-US/Console User Guide/Manage buckets/Set anti-leech.md#)|Web application, which is intuitive and easy to use|
|[Java SDK](../../../../reseller.en-US/SDK Reference/Java/Anti-leech.md#)|SDK demos in various languages|
|[Python SDK](../../../../reseller.en-US/SDK Reference/Python/Anti-leech.md#)|
|[PHP SDK](../../../../reseller.en-US/SDK Reference/PHP/Anti-leech.md#)|
|[Go SDK](../../../../reseller.en-US/SDK Reference/Go/Anti-leech.md#)|
|[C SDK](../../../../reseller.en-US/SDK Reference/C/Anti-leech.md#)|
|[.NET SDK](../../../../reseller.en-US/SDK Reference/. NET/Anti-leech.md#)|
|[Node.js SDK](../../../../reseller.en-US/SDK Reference/Node. js/Anti-leech.md#)|
|[Ruby SDK](../../../../reseller.en-US/SDK Reference/Ruby/Anti-leech.md#)|

## Detail analysis {#section_tnx_kjd_5db .section}

-   Referer verification
    -   Hotlinking protection verification is required only when you access an object anonymously or by using a signed URL. Hotlinking protection verification is not required if the request header contains the `Authorization` field.
    -   Hotlinking protection verification is required when the ACL of a bucket is private, public-read, or public-read-write.
-   Referer configuration
    -   A bucket supports multiple Referer parameters. When setting multiple Referer parameters, you can separate them with a line break in the console or with a comma \(,\) through the PutBucketReferer API.
    -   You can use wildcards, such as an asterisk \(\*\) or a question mark \(?\), to set the Referer parameter.
-   Referer effects
    -   If the referer whitelist is empty, OSS does not check whether the Referer field is left empty in requests. Otherwise, all requests are rejected.
    -   If the referer whitelist is not empty and the Referer field cannot be left empty, OSS allows only requests whose Referer field values are in the referer whitelist, and rejects all other requests \(including requests whose Referer fields are left empty\).
    -   If the referer whitelist is not empty and the Referer field can be left empty, OSS allows requests whose Referer fields are left empty and requests whose Referer field values are in the referer whitelist, and rejects all other requests.

## Wildcards {#section_emg_mjd_5db .section}

-   Asterisk \(\*\): used to replace zero or more characters. If you are looking for an object whose name is prefixed with AEW but have forgotten the remaining part, you can enter AEW\* to search for all objects whose names start with AEW, such as AEWT.txt, AEWU.EXE, and AEWI.dll. To narrow down the search scope, you can enter AEW\*.txt to search for all .txt objects whose names start with AEW, such as AEWIP.txt and AEWDF.txt.
-   Question mark \(?\): used to replace one character. If you enter love?, all objects whose names start with love and end with one character are displayed, such as lovey and lovei. To narrow down the search scope, you can enter love?.doc to search for all .doc objects whose names start with love and end with one character, such as lovey.doc and loveh.doc.

## Reference {#section_ztx_kv5_vgb .section}

-   For more information about the hotlinking protection principles, see [Anti-leech](../../../../reseller.en-US/Best Practices/Bucket management/Anti-leech.md#).
-   For more information about FAQs for hotlinking protection, see [OSS hotlinking protection \(referer\) errors and troubleshooting](../../../../reseller.en-US/Errors and Troubleshooting/Referer.md#).

