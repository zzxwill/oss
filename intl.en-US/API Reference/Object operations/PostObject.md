# PostObject {#reference_smp_nsw_wdb .reference}

The PostObject operation is used to upload a file to a specified bucket using the HTML form.

As a substitute of Put Object, Post Object makes it possible to upload files to a bucket based on the browser. The message body of Post Object is encoded using multipart/form-data. In the Put Object operation, parameters are transferred through the HTTP request header.

## Post object {#section_xwr_rsw_wdb .section}

**Request syntax**

```
POST / HTTP/1.1 
Host: BucketName.oss-cn-hangzhou.aliyuncs.com
User-Agent: browser_data
Content-Length：ContentLength
Content-Type: multipart/form-data; boundary=9431149156168
--9431149156168
Content-Disposition: form-data; name="key"
key
--9431149156168
Content-Disposition: form-data; name="success_action_redirect"
success_redirect
--9431149156168
Content-Disposition: form-data; name="Content-Disposition"
attachment;filename=oss_download.jpg
--9431149156168
Content-Disposition: form-data; name="x-oss-meta-uuid"
myuuid
--9431149156168
Content-Disposition: form-data; name="x-oss-meta-tag"
mytag
--9431149156168
Content-Disposition: form-data; name="OSSAccessKeyId"
access-key-id
--9431149156168
Content-Disposition: form-data; name="policy"
encoded_policy
--9431149156168
Content-Disposition: form-data; name="Signature"
signature
--9431149156168
Content-Disposition: form-data; name="file"; filename="MyFilename.jpg"
Content-Type: image/jpeg
file_content
--9431149156168
Content-Disposition: form-data; name="submit"
Upload to OSS

```

**Form fields**

|Name|Type|Description|Required or Optional|
|:---|:---|:----------|:-------------------|
|OSSAccessKeyId|string|Specify the AccessKey ID of the bucket owner.Default value: none

Restriction: This form field is required when the bucket does not allow public-read-write, or when the Policy \(or Signature\) form field is provided.

|Conditional|
|policy|string|Specify validity of the form fields in the request. A request that does not contain the Policy form field is treated as an anonymous request, and can only access buckets that allow public-read-write. For more information, see 5.7.4.1 Post policy.Default value: none

Restriction: This form field is required when the bucket does not allow public-read-write, or when the OSSAccessKeyId \(or Signature\) form field is provided.

|Conditional|
|Signature|string|Specify the signature information that is computed based on the Access Key Secret and Policy. The OSS checks the signature information to verify validity of the Post Object request. For more information, see 5.7.4.2 Post Signature.Default value: none

Restriction: This form field is required when the bucket does not allow public-read-write, or when the OSSAccessKeyId \(or Policy\) form field is provided.

|Conditional|
|Cache-Control, Content-Type, Content-Disposition, Content-Encoding, Expires|string|REST request headers. For more information, see the related descriptions in Put Object. Default value: none

|Optional|
|file|string|Specify the file or text content. It must be the last field in the form. The browser automatically sets Content-Type based on the file type, and overwrites the user setting. The OSS can only upload one file at a time.Default value: none

|Required|
|key|string|Specify the object name of the uploaded file. If the object name contains forward slashes \(/\), such as a/b/c/b.jpg, OSS will create the corresponding directory. Default value: none

 |Required|
|success\_action\_redirect|string|Specify the URL to which the client is redirected after successful upload. If this form field is not specified, the returned result is specified by success\_action\_status. If upload fails, the OSS returns an error code, and the client is not redirected to any URL.Default value: none

|Optional|
|success\_action\_status|string|Specify the status code returned to the client after the previous successful upload if success\_action\_redirect is not specified. Valid values include 200,  201, and 204 \(default\). If this field is set to 200 or 204, the OSS returns an empty file and a corresponding status code. If this field is set to 201, the OSS returns an XML file and the 201 status code. If this field is not specified or set to an invalid value, the OSS returns an empty file and the 204 status code.Default value: none

| |
|x-oss-meta-\*|string|Specify the user meta value set by the user. The OSS does not check or use this value.Default value: none

|Optional|
|x-oss-server-side-encryption|string|Specify the server-side encryption algorithm when the OSS creates an object.Valid value: AES256

|Optional|
|x-oss-object-acl|string|Specify the access permission when the OSS creates an object.Valid values: public-read, private, and public-read-write

|Optional|
|x-oss-security-token|string|If STS temporary authorization is used for this access, you must specify the item to be the SecurityToken value. At the same time, OSSAccessKeyId must use a paired temporary AccessKeyId. The signature calculation is consistent with the general AccessKeyId signature.Default value: none

|Optional|

**Response header**

|Name|Type|Description|
|:---|:---|:----------|
|x-oss-server-side-encryption|string|If x-oss-server-side-encryption is specified in the request, the response contains this header, which indicates the encryption algorithm used.|

Response elements

|Name|Type|Description|
|:---|:---|:----------|
|PostResponse|container|Specify the container that saves the result of the Post Object request.Sub-nodes: Bucket, ETag, Key, and Location

|
|Bucket|string|Specify the bucket name.Parent node: PostResponse

|
|ETag|string|Specify the entity tag \(ETag\) that is created when an object is generated. For an object created by Post Object, the ETag value is the UUID of the object, and can be used to check whether the content of the object has changed.Parent node: PostResponse

|
|Location|string|Specify the URL of the newly created object.Parent node: PostResponse

|

**Detail analysis**

-   To perform the Post Object operation, you must have the permission to write the bucket. If the bucket allows public-read-write, you can choose not to upload the signature information; otherwise, signature verification must be performed on the Post Object operation. Unlike Put Object, Post Object uses AccessKeySecret to compute the signature for the policy. The computed signature string is used as the value of the Signature form field. The OSS checks this value to verify validity of the signature.
-   No matter whether the bucket allows public-read-write, once any one of the OSSAccessKeyId, Policy, and Signature form fields is uploaded, the remaining two form fields are required. If the remaining two form fields are missing, the OSS returns the error code: InvalidArgument.
-   Form encoding submitted by the Post Object operation must be "multipart/form-data". That is, Content-Type in the header must be in the `multipart/form-data; boundary=xxxxxx` format, where boundary is the boundary string.
-   The URL of the submitted form can be the domain name of the bucket. It is not necessary to specify the object in the URL.  That is, the request line is `POST / HTTP/1.1`, and cannot be written as `POST /ObjectName HTTP/1.1`.
-   The policy specifies the valid values of form fields in the Post Object request. The OSS checks validity of the request based on the policy. If the request is invalid, the OSS returns the error code: AccessDenied. When checking validity of the policy, the OSS does not check irrelevant form fields in the policy.
-   The form and policy must be encoded with UTF-8. The policy is a JSON text encoded with UTF-8 and Base64.
-   The Post Object request can contain extra form fields. The OSS checks validity of these form fields based on the policy.
-   If you have uploaded the Content-MD5 request header, the OSS calculates the body's Content-MD5 and check if the two are consistent. If the two are different, the error code InvalidDigest is returned.
-   If the Post Object request contains the Header signature or URL signature, the OSS does not check these signatures.
-   If the Put Object request carries a form field prefixed with x-oss-meta-, the form field is treated as the user meta, for example, x-oss-meta-location. A single object can have multiple similar parameters, but the total size of all user meta cannot exceed 8 KB.
-   The total length of the body in the Post Object request cannot exceed 5 GB. When the file length is too large, the system returns the error code: EntityTooLarge.
-   If the x-oss-server-side-encryption header is specified when you upload an object, the value of this header must be set to AES256 or KMS. Otherwise, the system returns 400 and the error code: InvalidEncryptionAlgorithmError. After this header is specified, the response header also contains this header, and the OSS stores the encryption algorithm of the uploaded object. When this object is downloaded, the response header contains x-oss-server-side-encryption, the value of which is set to the encryption algorithm of this object.
-   Form fields are not case-sensitive, but their values are case-sensitive.

**Examples**

-   Request example:

    ```
    POST / HTTP/1.1
    Host: oss-example.oss-cn-hangzhou.aliyuncs.com
    Content-Length: 344606
    Content-Type: multipart/form-data; boundary=9431149156168
    --9431149156168
    Content-Disposition: form-data; name="key"
    /user/a/objectName.txt
    --9431149156168
    Content-Disposition: form-data; name="success_action_status"
    200
    --9431149156168
    Content-Disposition: form-data; name="Content-Disposition"
    content_disposition
    --9431149156168
    Content-Disposition: form-data; name="x-oss-meta-uuid"
    uuid
    --9431149156168
    Content-Disposition: form-data; name="x-oss-meta-tag"
    metadata
    --9431149156168
    Content-Disposition: form-data; name="OSSAccessKeyId"
    44CF9590006BF252F707
    --9431149156168
    Content-Disposition: form-data; name="policy"
    eyJleHBpcmF0aW9uIjoiMjAxMy0xMi0wMVQxMjowMDowMFoiLCJjb25kaXRpb25zIjpbWyJjb250ZW50LWxlbmd0aC1yYW5nZSIsIDAsIDEwNDg1NzYwXSx7ImJ1Y2tldCI6ImFoYWhhIn0sIHsiQSI6ICJhIn0seyJrZXkiOiAiQUJDIn1dfQ==
    --9431149156168
    Content-Disposition: form-data; name="Signature"
    kZoYNv66bsmc10+dcGKw5x2PRrk=
    --9431149156168
    Content-Disposition: form-data; name="file"; filename="MyFilename.txt"
    Content-Type: text/plain
    abcdefg
    --9431149156168
    Content-Disposition: form-data; name="submit"
    Upload to OSS
    --9431149156168--
    ```

-   Response example:

    ```
    HTTP/1.1 200 OK
    x-oss-request-id: 61d2042d-1b68-6708-5906-33d81921362e 
    Date: Fri, 24 Feb 2014 06:03:28 GMT
    ETag: 5B3C1A2E053D763E1B002CC607C5A0FE
    Connection: keep-alive
    Content-Length: 0  
    Server: AliyunOSS
    ```


## Post Policy {#section_d5z_1ww_wdb .section}

The policy form field requested by POST is used to verify the validity of the request. The policy is a JSON text encoded with UTF-8 and Base64. It states the conditions that a Post Object request must meet. Although the post form field is optional for uploading public-read-write buckets, we strongly suggest using this field to limit POST requests.

**Policy example**

```
{ "expiration": "2014-12-01T12:00:00.000Z",
  "conditions": [
    {"bucket": "johnsmith" },
    ["starts-with", "$key", "user/eric/"]
  ]
}
```

In the Post Object request, the policy must contain expiration and conditions.

**Expiration**

Expiration specifies the expiration time of the policy, which is expressed in ISO8601 GMT. For example, "2014-12-01T12:00:00.000Z" means that the Post Object request must be sent before 12:00 on December 1, 2014.

**Conditions**

Conditions is a list that specifies the valid values of form fields in the Post Object request. Note: The value of a form field is extended after the OSS checks the policy. Therefore, the valid value of the form field set in the policy is equivalent to the value of the form field before extension. The following table lists the conditions supported by the policy:

|Name|Description|
|:---|:----------|
|content-length-range|Specify the acceptable maximum and minimum sizes of the uploaded file. This condition supports the content-length-range match mode.|
|Cache-Control, Content-Type, Content-Disposition, Content-Encoding, Expires|HTTP request headers. This condition supports the exact match and starts-with match modes.|
|key|Specify the object name of the uploaded file. This condition supports the exact match and starts-with match modes.|
|success\_action\_redirect|Specify the URL to which the client is redirected after successful upload. This condition supports the exact match and starts-with match modes.|
|success\_action\_status|Specify the status code returned after successful upload if success\_action\_redirect is not specified. This condition supports the exact match and starts-with match modes.|
|x-oss-meta-\*|Specify the user meta set by the user. This condition supports the exact match and starts-with match modes.|

If the Post Object request contains other form fields, these extra form fields can be added to Conditions of the policy. The OSS does not check validity of the form fields that are not contained in the conditions.

**Condition match modes**

|Condition match modes|Description|
|:--------------------|:----------|
|Exact match|The value of a form field must be exactly the same as the value declared in the conditions. For example, if the value of the key form field must be a, the conditions must be: \{“key”: “a”\}, or: \[“eq”, “$key”, “a”\]|
|Starts With|The value of a form field must start with the specified value. For example, if the value of key must start with /user/user1, the conditions must be: \[“starts-with”, “$key”, “/user/user1”\]|
|Specified file size|Specify the maximum and minimum sizes of the files that can be uploaded. For example, if the acceptable file size is 1–10 bytes, the conditions must be: \["content-length-range", 1, 10\]|

**Escape characters**

In the policy form field of the Post Object request, $ is used to indicate a variable. Therefore, to describe $, the escape character must be used. In addition, some characters in JSON strings are escaped. The following chart describes characters in the JSON string of the policy form field of a Post Object request.

|Escape characters|Description|
|:----------------|:----------|
|\\/|Slash|
|\\|Backslash|
|\\”|Double quotation marks|
|\\$|Dollar sign|
|Space|Space|
|\\f|Form feed|
|\\n|Newline|
|\\r|Carriage return|
|\\t|Horizontal tab|
|\\uxxxx|Unicode character|

## Post Signature {#section_wny_mww_wdb .section}

For a verified Post Object request, the HTML form must contain policy and signature. Policy specifies which values are acceptable in the request. The procedure for computing signature is as follows:

1.  Create a UTF-8 encoded policy.
2.  Encode the policy with Base64. The encoding result is the value of the policy form field, and this value is used as the string to be signed.
3.  Use AccessKeySecret to sign the string. The signing method is the same as the computing method of the signature in the Header, that is, replacing the string to be signed with the policy form field.

