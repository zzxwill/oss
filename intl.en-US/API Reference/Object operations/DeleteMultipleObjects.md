# DeleteMultipleObjects {#reference_ydg_25v_wdb .reference}

Deletes multiple objects from the same bucket.

You can perform the DeleteMultipleObjects operation to delete up to 1,000 objects with one request. Two response modes are available: the Verbose mode and the Quiet mode.

-   Verbose mode: The message body returned by OSS contains the result of each deleted object.
-   Quiet mode: The message body returned by OSS only contains the results for objects which encountered an error in the DELETE process. If all objects are successfully deleted, no message body is returned.

## Request syntax {#section_iqs_fvv_wdb .section}

```
POST /?delete HTTP/1.1
Host: BucketName.oss-cn-hangzhou.aliyuncs.com
Date: GMT Date
Content-Length: ContentLength
Content-MD5: MD5Value
Authorization: SignatureValue
<?xml version="1.0" encoding="UTF-8"?>
<Delete>
  <Quiet>true</Quiet>
  <Object>
    <Key>key</Key>
  </Object>
…
</Delete>
```

## Request headers {#section_wmb_lvv_wdb .section}

OSS verifies the received message body based on the following headers, and deletes the object only when the attributes of the message body conform to the headers.

|Name|Description|
|:---|:----------|
|encoding-type|Specify the encoding type of the Key in the returned result. Currently, the URL encoding is supported. The Key adopts UTF-8 encoding, but the XML 1.0 Standard does not support parsing certain control characters, such as the characters with ASCII values from 0 to 10. In case that the Key contains control characters not supported by the XML 1.0 Standard, you can specify the encoding-type to encode the returned Key.Data type: String

Default: None

Optional value: url

|

|Header|Type|Required|Description|
|:-----|:---|:-------|:----------|
|Encoding-type|String|No| The Key parameter is UTF-8 encoded. If the Key parameter includes control characters which are not supported by the XML 1.0 standard, you can specify this header to encode the Key parameter in the returned result.

 Default value: None

 Valid value: url

 |
|Content-Length|String|Yes| Indicates the length of the HTTP message body.

 OSS verifies the received message body based on this header, and deletes the object only when the length of the message body is the same as this header.

 |
|Content-MD5|String|Yes| Content-MD5 is a string calculated with the MD5 algorithm. This header is used to check whether the content of the received message is consistent with that of the sent message. If this header is included in the request, OSS calculates the Content-MD5 of the received message body and compares it with the value of this header.

 **Note:** To obtain the value of this header, encrypt the message body of the DeleteMultipleObjects request using the MD5 algorithm to get a 128-bit byte array, and then base64-encode the byte array.

 |

## Request elements {#section_l1c_svv_wdb .section}

|Element|Type|Required|Description|
|:------|:---|--------|:----------|
|Delete|Container|Yes|Specifies the container that stores the DeleteMultipleObjects request.Sub-node: One or more Objects, Quite

Parent node: None

|
|Key|String|Yes|Specifies the name of the object to be deleted. Parent node: Object

|
|Object|Container|Yes|Specifies the container that stores the information about the object.Sub-node: Key

Parent node: Delete

|
|Quiet|Enumerated string|Yes|Enables the Quiet response mode.DeleteMultipleObjects provides the following two response modes:

-   Quiet: The message body of the response returned by OSS only includes objects that fail to be deleted. If all objects are deleted successfully, the response does not include a message body.
-   Verbose: The message body of the response returned by OSS includes the results of all deleted objects. This mode is used by default.

Valid value: true\(enables Quite mode\), false\(enables Verbose mode\)

Default value: false

Parent node: Delete

|

## Response elements {#section_ync_yyv_wdb .section}

|Elements|Type|Description|
|:-------|:---|:----------|
|Deleted|Container|Specifies the container that stores the successfully deleted objects. Sub-node: Key

Parent node: DeleteResult

|
|DeleteResult|Container|Specifies the container that stores the returned results of the DeleteMultipleObjects request. Sub-node: Deleted

Parent node: None

|
|Key|String|Specifies the name of the deleted object.Parent node: Deleted

 |
|EncodingType|String|Specifies the encoding type for the returned results. If encoding-type is specified in the request, the Key is encoded in the returned result.Parent node: Container

|

## Example {#section_mcs_21w_wdb .section}

Request example with Quite mode disabled:

```
POST /? delete HTTP/1.1
Host: oss-example.oss-cn-hangzhou.aliyuncs.com
Date: Wed, 29 Feb 2012 12:26:16 GMT
Content-Length:151
Content-MD5: ohhnqLBJFiKkPSBO1eNaUA==
Authorization: OSS qn6qrrqxo2oawuk53otfjbyc:+z3gBfnFAxBcBDgx27Y/jEfbfu8=
<? xml version="1.0" encoding="UTF-8"? >
<Delete> 
  <Quiet>false</Quiet>  
  <Object> 
    <Key>multipart.data</Key> 
  </Object>  
  <Object> 
    <Key>test.jpg</Key> 
  </Object>  
  <Object> 
    <Key>demo.jpg</Key> 
  </Object> 
</Delete>
```

Response example:

```
HTTP/1.1 200 OK
x-oss-request-id: 78320852-7eee-b697-75e1-b6db0f4849e7
Date: Wed, 29 Feb 2012 12:26:16 GMT
Content-Length: 244
Content-Type: application/xml
Connection: keep-alive
Server: AliyunOSS
<? xml version="1.0" encoding="UTF-8"? >
<DeleteResult xmlns=”http://doc.oss-cn-hangzhou.aliyuncs.com”>
    <Deleted>
       <Key>multipart.data</Key>
    </Deleted>
    <Deleted>
       <Key>test.jpg</Key>
    </Deleted>
    <Deleted>
       <Key>demo.jpg</Key>
    </Deleted>
</DeleteResult>
```

Request example with Quite mode enabled:

```
POST /? delete HTTP/1.1
Host: oss-example.oss-cn-hangzhou.aliyuncs.com
Date: Wed, 29 Feb 2012 12:33:45 GMT
Content-Length:151
Content-MD5: ohhnqLBJFiKkPSBO1eNaUA==
Authorization: OSS qn6qrrqxo2oawuk53otfjbyc:WuV0Jks8RyGSNQrBca64kEExJDs=
<? xml version="1.0" encoding="UTF-8"? >
<Delete> 
  <Quiet>true</Quiet>  
  <Object> 
    <Key>multipart.data</Key> 
  </Object>  
  <Object> 
    <Key>test.jpg</Key> 
  </Object>  
  <Object> 
    <Key>demo.jpg</Key> 
  </Object> 
</Delete>
```

Response example:

```
HTTP/1.1 200 OK
x-oss-request-id: 559CC9BDC755F95A64485981
Date: Wed, 29 Feb 2012 12:33:45 GMT
Content-Length: 0
Connection: keep-alive
Server: AliyunOSS
```

## SDK {#section_egl_m2c_5gb .section}

The SDKs of this API are as follows:

-   [Java](../../../../../intl.en-US/SDK Reference/Java/Manage objects/Delete objects.md)
-   [Python](../../../../../intl.en-US/SDK Reference/Python/Manage objects/Delete objects.md)
-   [PHP](../../../../../intl.en-US/SDK Reference/Python/Manage objects/Delete objects.md)
-   [Go](../../../../../intl.en-US/SDK Reference/Go/Manage objects/Delete objects.md)
-   [C](../../../../../intl.en-US/SDK Reference/C/Manage objects/Delete objects.md)
-   [.NET](../../../../../intl.en-US/SDK Reference/. NET/Manage objects/Delete objects.md)
-   [iOS](../../../../../intl.en-US//Manage objects.md)
-   [Node.js](../../../../../intl.en-US//Manage objects.md)
-   [Browser.js](../../../../../intl.en-US/SDK Reference/Browser.js/Manage objects.md)
-   [Ruby](../../../../../intl.en-US/SDK Reference/Ruby/Manage objects.md)

## Error codes {#section_nfp_nfc_5gb .section}

|Error code|HTTP status code|Description|
|:---------|:---------------|:----------|
|InvalidDigest|400|If you specify the Content-MD5 header in the request, OSS calculates the Content-MD5 of the message body and compares it with this header. If the two values are different, this error code is returned.|
|MalformedXML|400| -   A DeleteMultipleObjects request can contain a message body of up to 2 MB. If the size of the message body exceeds 2 MB, this error code is returned.
-   A Delete Multiple Objects request can be used to delete up to 1,000 objects at a time. If the number of objects to be deleted at a time exceeds 1,000, this error code is returned.

 |

