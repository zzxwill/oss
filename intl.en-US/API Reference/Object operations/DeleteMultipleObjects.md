# DeleteMultipleObjects {#reference_ydg_25v_wdb .reference}

The DeleteMultipleObjects operation allows you to delete multiple objects in the same bucket with one HTTP request.

You can perform the Delete Multiple  Objects operation to delete up to 1,000 objects with one request. Two response modes are available: the Verbose mode and the Quiet mode.

-   Verbose mode: The message body returned by OSS contains the result of each deleted object.
-   Quiet mode: The message body returned by OSS only contains the results for objects which encountered an error in the DELETE process. If all objects are successfully deleted, no message body is returned.

## Request syntax {#section_iqs_fvv_wdb .section}

```
POST /? delete HTTP/1.1
Host: BucketName.oss-cn-hangzhou.aliyuncs.com
Date: GMT Date
Content-Length: ContentLength
Content-MD5: MD5Value
Authorization: SignatureValue
<? xml version="1.0" encoding="UTF-8"? >
<Delete>
  <Quiet>true</Quiet>
  <Object>
    <Key>key</Key>
  </Object>
…
</Delete>
```

## Request parameters {#section_wmb_lvv_wdb .section}

During the Delete Multiple Objects operation, you can use encoding-type to encode the Key in the returned result.

|Name|Description|
|:---|:----------|
|encoding-type|Specify the encoding type of the Key in the returned result. Currently, the URL encoding is supported. The Key adopts UTF-8 encoding, but the XML 1.0 Standard does not support parsing certain control characters, such as the characters with ASCII values from 0 to 10. In case that the Key contains control characters not supported by the XML 1.0 Standard, you can specify the encoding-type to encode the returned Key.Data type: String

Default: None

Optional value: url

|

## Request elements {#section_l1c_svv_wdb .section}

|Name|Type|Description|
|:---|:---|:----------|
|Delete|Container|Specify the container that saves the Delete Multiple Objects request.Sub-nodes: one or more object elements and the optional quiet element

Parent node: None

|
|Key|String|Specify the name of the deleted object. Parent node: Object

|
|Object|Container|Specify the container that saves the information about the object.Sub-node: key

Parent node: Delete

|
|Quiet|enumerative string|Enables the “Quiet” response mode.Valid values: true, false

Default: false

Parent node: Delete

|

## Response elements {#section_ync_yyv_wdb .section}

|Name|Type|Description|
|:---|:---|:----------|
|Deleted|Container|Specify the container that saves the successfully deleted objects. Sub-node: key

Parent node: DeleteResult

|
|DeleteResult|Container|Specify the container that saves the result of the Delete Multiple Objects request. Sub-node: Deleted

Parent node: None

|
|Key|String|Specify the name of the object on which OSS performs the Delete operation.Parent node: Deleted

 |
|EncodingType|String|Indicates the type used by the encoding in the return result. Specify the encoding type for the returned results. If encoding-type is specified in the request, the Key is encoded in the returned result.Parent node: Container

|

## Detail analysis {#section_xfy_rzv_wdb .section}

-   The Content-Length and Content-MD5 fields must be specified in the Delete Multiple Objects request. OSS verifies that the received message body is correct based on the two fields before performing the Delete operation.
-   Method to generate the Content-MD5 field: Encrypt the MD5 value of the Delete Multiple Objects request to obtain a 128-byte array, and encode the array using Base64. The final string obtained is the content of the Content-MD5 field.
-   The return mode of the Delete Multiple Objects request is Verbose by default.
-   If the Delete Multiple Objects request is used to delete a non-existing object, the operation is still regarded as successful.
-   The Delete Multiple Objects request can contain a message body of up to 2 MB. If the size of the message body exceeds 2 MB, the system returns the MalformedXML error code.
-   The Delete Multiple Objects request can be used to delete up to 1,000 objects at a time. If the number of objects to be deleted at a time exceeds 1,000, the system returns the MalformedXML error code.
-   If you have uploaded the Content-MD5 request header, OSS calculates the body’s Content-MD5 and check if the two are consistent. If the two are different, the error code InvalidDigest is returned.

## Example {#section_mcs_21w_wdb .section}

**Request example I:**

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

**Response example:**

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

**Request Example II:**

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

**Response example:**

```
HTTP/1.1 200 OK
x-oss-request-id: 559CC9BDC755F95A64485981
Date: Wed, 29 Feb 2012 12:33:45 GMT
Content-Length: 0
Connection: keep-alive
Server: AliyunOSS
```

