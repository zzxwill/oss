# PostObject {#concept_nrx_jfj_wdb .concept}

## Introduction {#section_mcj_kfj_wdb .section}

PostObject uploads files to OSS using forms. In Post Object, message entities are encoded in multi-form format multipart/form-data. For more information, see [RFC 2388](https://tools.ietf.org/html/rfc2388). In Put Object, parameters are passed by HTTP headers, while Post Object parameters are passed as form fields of the message body.

A PostObject message consists of the header and the body. The header and the body are separated by `\r\n--{boundary}`. The body consists of a series of form fields in the following format: `Content-Disposition: form-data; name="{key}"\r\n\r\n{value}\r\n--{boundary}`.

Common headers include Host, User-Agent, Content-Length, Content-Type and Content-MD5 while form fields include key, OSSAccessKeyId, Signature, Content-Disposition, object meta \(x-oss-meta-\*\), x-oss-security-token, other HTTP headers \(Cache-Control/Content-Type/Cache-Control/Content-Type/Content-Disposition/Content-Encoding/Expires/Content-Encoding/Expires\) and file. The `file` must be the last field in those form fields.

For more information, see [Post Object](../../../../reseller.en-US/API Reference/Object operations/PostObject.md#).

## PostObject common errors {#section_uxq_lfj_wdb .section}

The following table shows PostObject common errors:

|No.|Error|Cause| Solution|
|:--|:----|:----|:--------|
|1|ErrorCode: MalformedPOSTRequest ErrorMessage: The body of your POST request is not well-formed multipart/form-data|Invalid form field format.|See PostObject form field format following the table for the correct format of form fields.|
|2|ErrorCode: InvalidAccessKeyId ErrorMessage: The OSS Access Key Id You provided does not exist in our records.|`AccessKeyID` was disabled or did not exist, the temporary user AccessKeyID was expired or the temporary user did not provide STS Token.|See [Invalid AccessKeyId Troubleshooting](reseller.en-US/Errors and Troubleshooting/OSS 403.md#) for the troubleshooting method.|
|3|ErrorCode: AccessDenied ErrorMessage: Invalid according to Policy: Policy expired.|The `expiration` in the form field Id `policy`was expired.|Adjust `expiration` in policy while ensuring that the format of `expiration` complies with ISO8601 GMT.|
|4|ErrorCode: AccessDenied ErrorMessage: SignatureDoesNotMatch The request signature we calculated does not match the signature you provided. Check your key and signing method.|Incorrect signature.|See PostObject signature for the signature method.|
|5|ErrorCode: InvalidPolicyDocument ErrorMessage: Invalid Policy: Invalid Simple-Condition: Simple-Conditions must have exactly one property specified.|The policy contains at least one condition in the request.|See PostObject policy format.|
|6|ErrorCode: InvalidPolicyDocument ErrorMessage: Invalid Policy: Invalid JSON: unknown char e|Check the format of `policy` to verify if |`"` was missing and the escape character was \\.|
|7|ErrorCode: InvalidPolicyDocument ErrorMessage: Invalid Policy: Invalid JSON: , or \] expected|Incorrect `policy` format in the request.|Check if `,` or `]` was missing in policy.|
|8|ErrorCode: AccessDenied ErrorMessage: Invalid according to Policy: Policy Condition failed: \[“starts-with”, “$key”, “user/eric/“\]|The `key` specified by the request and that specified by `policy` do not match.|Check the value of the form field `key` in the request.|
|9|ErrorCode: AccessDenied ErrorMessage: Invalid according to Policy: Policy Condition failed: \[“eq”, “$bucket”, “mingdi-bjx”\]|The `bucket` specified by the request and that specified by `policy` do not match.|Check the value of `bucket` in endpoint.|
|10|ErrorCode: AccessDenied ErrorMessage: Invalid according to Policy: Policy Condition failed: \[“starts-with”, “$x-oss-meta-prop”, “prop-“\]|File metadata `x-oss-meta-prop` specified by the request and that specified by policy do not match.|Check the value of `x-oss-meta-prop` in the request.|
|11|ErrorCode: AccessDenied ErrorMessage: Invalid according to Policy: Policy Condition failed: \[“eq”, “$\{field\}”, “$\{value\}”\]|The `{field}` specified in form fields and that specified by policy do not match, or that field was not specified in the request.|Check the value of `{field}` in the request.|
|12|ErrorCode: AccessDenied ErrorMessage: You have no right to access this object because of bucket acl.|Current user did not have the required permission.|See [OSS Permission Problems and Troubleshooting](reseller.en-US/Errors and Troubleshooting/OSS permission.md#).|
|13|ErrorCode: InvalidArgument ErrorMessage: The bucket POST must contain the specified ‘key’. If it is specified, please check the order of the fields|The form field does not specify `key`, or it is placed after the form field `file`.|Add form field `key` or adjust orders.|

-   PostObject form field format

    For the format of PostObject requests, note the following items:

    -   The header must include `Content-Type: multipart/form-data; boundary={boundary}`.
    -   The header and the body are separated by `\r\n--{boundary}`.
    -   The form field format is `Content-Disposition: form-data; name="{key}"\r\n\r\n{value}\r\n--{boundary}`.
    -   Form field names are case-sensitive, such as policy, key, file, OSSAccessKeyId, OSSAccessKeyId, and Content-Disposition.

        **Note:** The form field `file` must be the last form field.

    -   When the value of `bucket` is `public-read-write`, you do not have to specify the form fields OSSAccessKeyId, policy, and Signature. If any of OSSAccessKeyId, policy, and Signature is specified, the other two form fields must be specified no matter whether `bucket` is `public-read-write` or not.
    The following describes an example PostObject request:

    ```
    POST / HTTP/1.1
    User-Agent: Mozilla/5.0 (Windows; U; Windows NT 6.1; zh-CN; rv:1.9.2.6)
    Content-Type: multipart/form-data; boundary=9431149156168
    Host: mingdi-hz.oss-cn-hangzhou.aliyuncs.com
    Accept: text/html, image/gif, image/jpeg, *; q=. 2, */*; q=. 2
    Connection: keep-alive
    Content-Length: 5052
    -- 9431149156168
    Content-Disposition: form-data; name="key"
    test-key
    --9431149156168
    Content-Disposition: form-data; name="Content-Disposition"
    attachment;filename=D:\img\1.png
    --9431149156168
    Content-Disposition: form-data; name="OSSAccessKeyId"
    2NeL********j2Eb
    ```

    **Note:** 

    -   In the preceding sample request, `\r\n` shows a new line, namely a line feed. Also, this applies to the following sample requests.
    -   The preceding sample request is incomplete. For the complete request, see [Post Object](../../../../reseller.en-US/API Reference/Object operations/PostObject.md#).
    If you have any questions, see the sample code:

    -   [C\#](https://github.com/aliyun/aliyun-oss-csharp-sdk/blob/master/samples/Samples/PostPolicySample.cs)  
    -   [Java](https://github.com/aliyun/aliyun-oss-java-sdk/blob/master/src/samples/PostObjectSample.java)  
-   PostObject policy format

    In a PostObject request, the form field `policy`  is used to verify the validity of the request and it declares the conditions that must be met by the PostObject request. Specifically, those conditions are:

    -   UTF-8 JSON text must be encoded with base64 before being passed into the form field `policy`.
    -   The `policy` must include `expiration` and `conditions` where `conditions` must contain at least one item.
    The following shows an example `policy` before base64 encoding.

    ```
    
       {
        "expiration": "2018-01-01T12:00:00.000Z",
        "conditions": [
            ["content-length-range", 0, 104857600]
        ]
    }
    ```

    `expiration` item specifies an expiration time of the request in the  ISO8601 GMT time format. For example, `2018-01-01T12:00:00.000Z` specifies that the request must occur before 12:00 a.m. on January 1st, 2018.

    PostPolicy supports the following “conditions”:

    |Name|Description|Example|
    |:---|:----------|:------|
    |bucket|The bucket name of the uploaded file. Exact match is supported.|\{“bucket”: “johnsmith” \} or \[“eq”, “$bucket”,  “johnsmith”\]|
    |key|The name of the uploaded file. Exact match and prefix match are supported.|\[“starts-with”, “$key”, “user/etc/“\]|
    |content-length-range|The maximum and minimum allowed sizes of the uploaded file.|\[“content-length-range”, 0, 104857600\]|
    |x-oss-meta-\*|The specified object meta. Exact match and prefix match are supported.|\[“starts-with”, “$x-oss-meta-prop”, “prop-“\]|
    |success\_action\_redirect|The redirection URL upon successful upload. Exact match and prefix match are supported.|\[“starts-with”, “$success\_action\_redirect”, “`http://www.aliyun.com`“\]|
    |success\_action\_status|The returned status code upon successful upload if success\_action\_redirect is not specified. Exact match and prefix match are supported.|\[“eq”, “$success\_action\_status”, “204”\]|
    |Cache-Control, Content-Type, Content-Disposition,  Content-Encoding, Expires, and so on|The HTTP headers passed as form fields. Exact match and prefix match are supported.|\[“eq”, “$Content-Encoding”, “ZLIB”\]|

    PostPolicy supports the following escape characters and uses `\` for escape.

    |Escape Character|Description|
    |:---------------|:----------|
    |/|Slash|
    |\\|Backslash|
    |“|Double quotation mark|
    |$| Dollar sign|
    |\\b|Blank|
    |\\f|Form feed|
    |\\n|Line feed|
    |\\r|Enter|
    |\\t|Horizontal tab|
    |\\uxxxx|Unicode character|

    For more information about PostPolicy, see [Post Policy](../../../../reseller.en-US/API Reference/Object operations/PostObject.md#section_d5z_1ww_wdb).

-   PostObject signature

    For a Post request to be verified, it must include AccessKeyID, policy, and Signature form fields. The signature calculation process is as follows:

    1.  Create a policy encoded with `UTF-8`.
    2.  Encode the policy with `base64`. The resulting value is the value to be populated into the `policy` form field, and this value is used as the string to be signed.
    3.  Sign the string with `AccessKeySecret`. Specifically, hash the string with hmac-sha1 and then encode it with base64. The signature method is the same as that for [Header Signature](../../../../reseller.en-US/API Reference/Access control/Add a signature to the header.md#).
    Namely:

    ```
    Signature = base64(hmac-sha1(AccessKeySecret, base64(policy)))
    ```

    Specify the calculated signature in the form field `Signature` as follows:

    ```
    Content-Disposition: form-data; name="Signature"
    {signature}
    -- 9431149156168
    ```

    If you have any questions, see the sample code:

    -   [C\#](https://github.com/aliyun/aliyun-oss-csharp-sdk/blob/master/samples/Samples/PostPolicySample.cs)
    -   [Java](https://github.com/aliyun/aliyun-oss-java-sdk/blob/master/src/samples/PostObjectSample.java)

## FAQs {#section_w3h_jgj_wdb .section}

-   How to specify a key?

    The key is the object  name, which is specified in the form field `key`. The following shows an example:

    ```
    Content-Disposition: form-data; name="key"
    {key}
    --9431149156168
    ```

-   How to specify object content?

    Specify object content in the form field `file`. The following shows an example:

    ```
    Content-Disposition: form-data; name="file"; filename="images.png"
    Content-Type: image/png
    {File-content}
    -- 9431149156168
    ```

    **Note:** 

    -   The form field `file` must be the last field in a form, namely it must be placed after any other form fields.
    -   `filename` is the name of the uploaded local file but not the object name.
-   How to specify `content-type` of the object?

    Specify `content-type` of the object in the form field `file` but not in `content-type` of the header. The following shows an example:

    ```
    Content-Disposition: form-data; name="file"; filename="images.png"
    Content-Type: image/png
    {file-content}
    --9431149156168
    ```

-   How to specify `content-md5` verification for object content?

    Specify  `content-md5` in the Post Object request header. Note that the MD5 value is for the entire body namely for all form fields. The following shows an example request header:

    ```
    POST / HTTP/1.1
    User-Agent: Mozilla/5.0 (Windows; U; Windows NT 6.1; zh-CN; rv:1.9.2.6)
    Content-Type: multipart/form-data; boundary = 9431149156168
    Content-MD5: tdqHe4hT/TuKb7Y4by+nJg==
    Host: mingdi-hz.oss-cn-hangzhou.aliyuncs.com
    Accept: text/html, image/gif, image/jpeg, *; q=. 2, */*; q=. 2
    Connection: keep-alive
    Content-Length: 5246
    --9431149156168
    ```

-   How to specify a signature?

    See `PostObject signature` for the signature calculation method. The signature is carried by the form field  `Signature`.

-   How to implement Post Object with STS Token of a temporary  user?

    The usage of AccessKeyID and AccessKeySecret of a temporary user key is the same as that of a master user key and sub-user key.  `Token` is carried by the form field `x-oss-security-token`. The following shows an example:

    ```
    Content-Disposition: form-data; name="Signature"
    5L0+KaeugxYygfqWLJLoy0ehOmA=
    --9431149156168
    Content-Disposition: form-data; name="x-oss-security-token"
    {Token}
    --9431149156168
    ```

-    How to specify a callback?

    The callback is carried by the form field `callback`. The following shows an example:

    ```
    Content-Disposition: form-data; name="callback"
    eyJjYWxsYmFja0JvZHlUeXBlIjogImFwcGxpY2F0aW9uL3gtd3d3LWZvcm0tdXJsZW5jb2RlZCIsICJjYWxsYmFja0JvZHkiOiAiZmlsZW5hbWU9JHtvYmplY3R9JnNpemU9JHtzaXplfSZtaW1lVHlwZT0ke21pbWVUeXBlfSIsICJjYWxsYmFja1VybCI6ICJodHRwOi8vb3NzLWRlbW8uYWxpeXVuY3MuY29tOjIzNDUwIn0=
    --9431149156168
    ```

    Callback custom parameters are also carried by form fields. The following shows an example:

    ```
    Content-Disposition: form-data; name="x:var1"
    {var1-value}
    --9431149156168
    ```

-   How to specify `Content-Transfer-Encoding`?

    Specify `Content-Transfer-Encoding` in the form field `file`. `file`. The following shows an example `file`form field:

    ```
    Content-Disposition: form-data; name="file"; filename="images.png"
    Content-Type: image/png
    Content-Transfer-Encoding: base64
    {file-content}
    --9431149156168
    ```

-   How to specify custom meta information `Object User Meta`?

    Specify the custom meta information in form fields. The following shows an example:

    ```
    Content-Disposition: form-data; name="x-oss-meta-uuid"
    {uuid}
    --9431149156168
    Content-Disposition: form-data; name="x-oss-meta-tag"
    {tag}
    --9431149156168
    ```

    **Note:** For more information about file meta information, see [File Meta Information Object Meta](../../../../reseller.en-US/Developer Guide/Manage files/Object Meta.md#).

-   How to specify conditions such as expiration, Key, Bucket, size, and header?

    PostObject for OSS supports various conditions and can meet demanding security requirements. Specify conditions in the form field `policy`. The following shows an example policy:

    ```
    
        "expiration": "2018-01-01T12:00:00.000Z",
        "conditions": [
            ["eq", "$bucket", "md-hz"],
            ["starts-with", "$key", "md/conf/"],
            ["content-length-range", 0, 104857600]
        
    
    ```

    In the preceding policy, the conditions for user Post Object operations are as follows:

    -   `bucket` must be `md-hz`.
    -   `key` must be started with `md/conf/`.
    -   The size of the uploaded file must be less than 100 MB.
    -   The request time must be earlier than `2018-01-01T12:00:00.000Z`.
-   How to specify HTTP headers such as Cache-Control, Content-Type, Content-Disposition, Content-Encoding and Expires?

    Specify HTTP headers including `Cache-Control`,  `Content-Type`, `Content-Disposition`, `Content-Encoding`, `Expires` in form fields. For the meanings of those HTTP headers, see [RFC2616](https://tools.ietf.org/html/rfc2616?spm=a2c4g.11186623.2.20.ogoRhr) . However, `Content-MD5` needs to be specified in Post Header.


## Post Object examples {#section_o31_5hj_wdb .section}

-   [C\# Post Demo](https://github.com/aliyun/aliyun-oss-csharp-sdk/blob/master/samples/Samples/PostPolicySample.cs?spm=a2c4g.11186623.2.21.ogoRhr&file=PostPolicySample.cs)
-   [Java Post Demo](https://github.com/aliyun/aliyun-oss-java-sdk/blob/master/src/samples/PostObjectSample.java?spm=a2c4g.11186623.2.22.ogoRhr&file=PostObjectSample.java)

## Common links {#section_tfy_5hj_wdb .section}

-   [Post object](../../../../reseller.en-US/API Reference/Object operations/PostObject.md#)
-   [Java PostObject](https://yq.aliyun.com/articles/30346?spm=a2c4g.11186623.2.25.ogoRhr)

