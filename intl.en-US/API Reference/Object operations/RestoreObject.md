# RestoreObject {#reference_mfr_5bx_wdb .reference}

Restores an object of the Archive storage class.

**Note:** 

-   RestoreObject only applies to objects of the Archive storage class but not those of the Standard and IA storage classes.
-   A 202 status code is returned if you call RestoreObject to restore an object for the first time.
-   If you have restored an object by calling RestoreObject, a 200 OK message is returned if you call the API again.

## Billing methods {#section_oyj_p51_ygb .section}

The following fees are incurred when the status of an object is changed:

-   Data retrieval fees are incurred if you restore an archived object.
-   The restored state of an object can be prolonged to a maximum of seven days. No fees are incurred during this period.
-   After a restored object returns to the frozen state, data retrieval fees are incurred if you restore it again.

## Request syntax {#section_ivv_mcx_wdb .section}

```
POST /ObjectName?restore HTTP/1.1
Host: archive-bucket.oss-cn-hangzhou.aliyuncs.com
Date: GMT Date
Authorization: SignatureValue
```

## Examples {#section_qj3_scx_wdb .section}

Example of request initiated to restore a archived object for the first time:

```
POST /oss.jpg? restore HTTP/1.1
Host: oss-archive-example.oss-cn-hangzhou.aliyuncs.com
Date: Sat, 15 Apr 2017 07:45:28 GMT
Authorization: OSS e1Unnbm1rgdnpI:y4eyu+4yje5ioRCr5PB=
```

Response example

```
HTTP/1.1 202 Accepted
Date: Sat, 15 Apr 2017 07:45:28 GMT
Content-Length: 0
Connection: keep-alive
Server: AliyunOSS
x-oss-request-id: 5374A2880232A65C23002D74
```

Example of a request initiated to restore an object being restored:

```
POST /oss.jpg? restore HTTP/1.1
Host: oss-archive-example.oss-cn-hangzhou.aliyuncs.com
Date: Sat, 15 Apr 2017 07:45:29 GMT
Authorization: OSS e1Unnbm1rgdnpI:21qtGJ+ykDVmdy4eyu+NIUs=
```

Response example

```
HTTP/1.1 409 Conflict
Date: Sat, 15 Apr 2017 07:45:29 GMT
Content-Length: 556
Connection: keep-alive
Server: AliyunOSS
x-oss-request-id: 5374A2880232A65C23002D74
<? xml version="1.0" encoding="UTF-8"? >
<Error>
  <Code>RestoreAlreadyInProgress</Code>
  <Message>The restore operation is in progress.</Message>
  <RequestId>58EAF141461FB42C2B000008</RequestId>
  <HostId>10.101.200.203</HostId>
</Error>
```

Example of a request initiated to restore a restored object:

```
POST /oss.jpg? restore HTTP/1.1
Host: oss-archive-example.oss-cn-hangzhou.aliyuncs.com
Date: Sat, 15 Apr 2017 07:45:29 GMT
Authorization: OSS e1Unnbm1rgdnpI:u6O6FMJnn+WuBwbByZxm1+y4eyu+NIUs=
```

Response example

```
HTTP/1.1 200 Ok
Date: Sat, 15 Apr 2017 07:45:30 GMT
Content-Length: 0
Connection: keep-alive
Server: AliyunOSS
x-oss-request-id: 5374A2880232A65C23002D74
```

## SDK {#section_egl_m2c_5gb .section}

The SDKs of this API are as follows:

-   [Java](../../../../../intl.en-US/SDK Reference/Java/Manage objects/Restore an archive object.md)
-   [Python](../../../../../intl.en-US/SDK Reference/Python/Manage objects/Restore an archive object.md)
-   [PHP](../../../../../intl.en-US/SDK Reference/PHP/Manage objects/Restore an archive object.md)
-   [Go](../../../../../intl.en-US/SDK Reference/Go/Manage objects/Restore an archive object.md)
-   [C](../../../../../intl.en-US/SDK Reference/C/Manage objects/Restore an archive object.md)
-   [.NET](../../../../../intl.en-US/SDK Reference/. NET/Manage objects/Restore an archive object.md)

## Error codes {#section_nfp_nfc_5gb .section}

|Error code|HTTP status code|Description|
|:---------|:---------------|:----------|
|NoSuchKey|404|The requested object does not exist.|
|OperationNotSupported|400|The storage class of the requested object is not Archive.|
|RestoreAlreadyInProgress|409|You have called RestoreObject successfully and the object is being restored. Do not initiate RestoreObject requests repeatedly.|

